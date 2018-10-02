---
title: Hello World
categories: 博客搭建
tags: [hexo]
mathjax: true
---

这篇文章主要是讲一讲这个博客是如何搭建起来的，重点讲一讲有哪些额外的配置。

首先看一看[手把手教你使用Hexo + Github Pages搭建个人独立博客](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)。我使用了基于node.js的hexo，因为Github默认的jekyll是基于Ruby的实在不熟悉。在里面会讲到各种配置特别是**部署**的方法。

## Theme

我使用了[**Hacker**](https://github.com/CodeDaraW/Hacker)这个主题。如果喜欢比较完善的theme的同学可以尝试[iissnan/hexo-theme-*next*](https://github.com/iissnan/hexo-theme-next)。

## Disqus

Hacker主题自带Disqus支持。去Disqus上注册一个site shortname（注册完成后的页面的url的一部分）放到Hacker的_config.yml里即可。

## 分页

照着[Configuration](https://hexo.io/docs/configuration.html)配置site的_config.yml，找到`per_page`选项设置每页最多显示的post个数。

## 自动摘要

如果一个post太长，应该只显示前几段。首先，如果你在page里手动添加`<!- -more- ->`使得post从这里被折叠起来。但是在没有这个标记的时候，应该也能在post太长的时候自动折叠。Hacker本身不支持这个功能所以我仿照着[Hexo博客首页自动添加ReadMore标记](https://itguangzhi.github.io/2018/03/10/Hexo%20%E5%8D%9A%E5%AE%A2%E9%A6%96%E9%A1%B5%E8%87%AA%E5%8A%A8%E6%B7%BB%E5%8A%A0Read%20More%E6%A0%87%E8%AE%B0/)实现了一下。

修改后的`themes\Hacker\layout\components\article.ejs`：

```ejs
 <div class="article-content">
    <div class="entry">
      <% if (item.excerpt && index){ %>
        <%- item.excerpt %>
      <% } else { %>
        <% let br=item.content.indexOf('\n') %>
        <% if (br!==-1&&index&&theme.enable_excerpt){ %>
            <%- item.content.substring(0, br) %>
            <p>More...</p>
        <% } else { %>
            <%- item.content %>
        <% } %>
      <% } %>
    </div>
```

在`theme\Hacker\_config.yml`中添加

```yml
# auto excerpt
enable_excerpt: true
```

会找到第一个换行并从这里断开。

## 访客统计

见[手把手教你使用Hexo + Github Pages搭建个人独立博客](https://linghucong.js.org/2016/04/15/2016-04-15-hexo-github-pages-blog/)。

## 标签和类别

Hacker支持标签和类别，具体如何使用可以参考[Hacker作者自己用Hacker写的博客的repo](https://raw.githubusercontent.com/CodeDaraW/Blog/master/source/_posts/a-problem-about-gulp-del.md)。比如我的md首部是

```yaml
---
title: Hello World
categories: 博客搭建
tags: [hexo]
mathjax: true
---
```

但是，作者在文档里描述的可以在index添加标签和类别的索引页的方法

```
menu:
  Home: /
  Archives: /archives
  Categories: /categories
  Tags: /tags
```

我试了之后却在点击 Categories和Tags时遇到404。观察`public\categories`文件夹，可以发现只生成了每个Category有哪些文章的页面，没有生成一个显示有哪些Category的页面。综上我没有在index添加这两个按钮。

## Mathjax

Markdown自身是不带Latex数学公式支持的，要配置的话参考[hexo下LaTeX无法显示的解决方案](https://www.jianshu.com/p/d95a4795f3a8)。Hacker不原生支持Mathjax，所以我又自己实现了一下。

在`theme\Hacker\_config.yml`中添加

```yaml
# Mathjax support
mathjax:
  enable: true,
  per_page: true
  src: //cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML
```

在`themes\Hacker\layout\index.ejs`最后添加

```ejs
<% if (theme.mathjax.enable) { %>
    <script src="<%- theme.mathjax.src %>"></script> <% }
%>
```

在`themes\Hacker\layout\components\article.ejs`最后添加

```ejs
<% if (!index&&theme.mathjax.enable&&(!theme.mathjax.per_page||page.mathjax)) { %>
  <script src="<%- theme.mathjax.src %>"></script>
<% } %>
```

另外，只要看一下官方文档的这个页面[Variables](https://hexo.io/docs/variables.html)，就可以尝试修改ejs文件了。

我们可以现在测试一下公式$$lim_{1\to+\infty}P(|\frac{1}{n}\sum_i^nX_i-\mu|<\epsilon)=1, i=1,...,n$$ 

下面的多行公式显示起来仍然有问题，不过我现在不用多行公式的话已经够用了。

\begin{align} 
x &= a +b+c+d\\
 &= e +f\\
 &= g
\end{align}

如果要改进的话看[Hexo 书写 LaTeX 公式时的一些问题及解决方法](https://jdhao.github.io/2017/10/06/hexo-markdown-latex-equation/)。

## 图片

如果引用图片请看[Asset Folders](https://hexo.io/docs/asset-folders)。一个site有个公共的资源存放位置`source`，比如我把图片放在`source\images\narcissu.jpg`，然后在md中写`![narcissu](/images/narcissu.jpg)`，效果如图

![narcissu](/images/narcissu.jpg)

如果在`_config.yml`里面设置`post_asset_folder: true`，那么每个md都在同一个目录下有个同名文件夹。比如`hello-world.md`同目录下有个文件夹`hello-world`，里面有`avatar.jpg`，那么在md里写`{` `% asset_image avatar.jpg avatar%` `}`，效果如图

{% asset_image avatar.jpg avatar %}

##### TODO 站内搜索

<del>待我写了足够多博客的时候吧</del>