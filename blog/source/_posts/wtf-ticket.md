---
title: WeChatTicket作业中的技术要点
tags: [travis,wechat]
date: 2018-10-20 00:48:56
categories: 软工
---

[WeChatTicket](https://github.com/ThssSE/WeChatTicket)是《软件工程3》的4人组队小作业，目标是实现一个通过微信公众号进行抢票的票务系统。我负责实现handlers.py中回复用户微信消息的部分。现在我在这里总结一下我遇到的技术问题和解决方案。

## handler架构

微信服务器向我们服务器发送post请求，内容为xml格式。由`WeChatView`接收这个请求后将xml解析成python字典。`CustomWeChatView`是`WeChatView`的子类，里面有很多handler，`CustomWeChatView`会逐个调用列表中handler的`check()`方法，将请求交给第一个返回`True`的handler处理。我们的任务就是检查解析好的请求信息，判断是否应该由我们处理，然后从数据库查询数据后用handler基类提供的API构造response返回数据。

## 解析用户文本信息

我们需要解析"抢票 软件工程"这样的用户输入。在解析用户输入的文字上，采用了先以一个空格为界将文本分成两半（`str.split(' ',1)`），然后对后一半`strip()`的方法。这种方法支持活动中有空格，并且支持用户输入的"抢票"与活动名称之间出现多个空格，增强了鲁棒性。但是这种操作要求活动名称不能以空格开头或结尾。

## 数据库加锁

mysql支持对并发修改进行加锁，同时django提供了加锁的API，可以让我们不用自己写SQL语句来实现加锁。`with transaction.atomic()`可以让这个code block内的代码以事务的方式进行，然后`objects.select_for_update()`相当于SQL中的`SELECT ... FOR UDPATE`语句，可以将当前这行加锁。

注意对上锁的数据进行修改时，一定要修改使用了`select_for_update()`再选出来的对象，如果在上锁之前就已经得到了这个对象，需要上锁后再在数据库中再搜索一遍取出来。

在抢票时将减少票数的代码独立出来，对数据库进行加锁，并将尽量少的代码放到加锁的区域中。

参考了<https://medium.com/@hakibenita/how-to-manage-concurrency-in-django-models-b240fed4ee2>。

## unique_id生成

在出票时要为每张票分配一个独立的字符串id。使用随机生成的uuid(`uuid.uuid4()`)重复的几率已经几乎为0了，但是由于uuid的生成是基于网卡地址和当时时间的毫秒数的，所以在高并发下仍然有可能重复。最终采用先生成随机的uuid，再把用户的id和活动id拼起来作为种子再生成一次uuid（`uuid.uuid3()`），这样就可以做到门票id不重复，而且用户id等信息也得到了保护。

## 几个提醒

- 将`str`转成`int`时最好先用`str.isdigit()`进行判断，不然如果不能解析为`int`的话，会抛出异常。
- 注意`Activity`出现在数据库中不一定能说明这个活动就存在，它的`status`必须是PUBLISHED才表示这个活动是用户能看见的。
- handler回复的消息大多是纯文本信息格式的，这是也可以用提供的`get_message()`加载HTML模板，将文字信息的模板放在独立的HTML文件中，方便文案修改。
- 如果你的队友在写`APIView`，你会经常看见他们用抛异常的方式结束这次请求。然而这是因为`APIView`在调用之前框架就`try ... catch`包裹了一遍。在handler里面不能用抛出异常来结束这次请求，不然没有人`catch`，整个服务器就崩了。
- 返回图文信息有8条的限制，如果发送超过8条，微信会直接向用户提示公众号故障，所以要在发送回复前进行处理，只取前8条。
- 模板xml中的url字段可能会包含`&`字符（在query string中），为了防止它被转义成`&amp;`，需要使用`{% autoescape off %}`将url包裹起来。

## 测试设计

由于用户发送和接受的格式都是xml，为了测试我们应该模仿`WeChatView`，将发送文字和点击按钮的行为抽象出来，并且解析出返回的文本消息和图文消息。

为了测试系统刚好在抢票开始和结束时刻的行为，可以mock`timezone.now()`，将这个函数改为返回一个固定的时间的函数。这可以用于观察系统在特定时刻的行为。

