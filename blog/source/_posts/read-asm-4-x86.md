---
title: 读X86汇编
tags: [asm]
date: 2018-10-06 22:39:10
categories: 汇编
mathjax: true
---

> memory access and **handshaking**指什么？

书中给出的Irvine32.lib工作在保护模式中，而Irvine16.lib工作在实模式中。

### Flat memory model

CPU似乎能够访问到一个线性地址空间的任何位置的内存，不需要考虑分段分页（和访问权限？）之类的问题。

Two hexadecimal digits together represent a byte.

Windows通过切换代码页(Code page)来使用更多种类的字符。

- [ ] UTF-8和Unicode是如何转换的

### CPU结构

- 寄存器
- ALU算术逻辑单元
- CU控制单元
- clock时钟

### 三条总线

- data bus
- control bus
- address bus

浮点数运算单元在ALU外面。

只有AX、BX、CX、DX可以分别访问高低字节。

在实模式下地址是20bit的，为了用16bit的integer表示，用段寄存器的$segment$和偏移$offset$表示，$address=segment\times 16+offset$。

现代的操作系统都使用flat segmentation model，每个段可以超过64kB，由OS控制具体的物理地址，GDT里存放段的基址、边界和访问权限。

- [x] 汇编立即数的后缀r是什么？encoded real，用16进制编码表示的IEEE浮点数。

`.stack 100h`设定栈的大小为256字节。

- [ ] label在不同的procedure中可以同名吗

`imul eax, ebx, 5`会把`ebx * 5`的结果存放到`eax`里。

`nop`指令，占用一字节，什么都不做，用来进行二进制文件对齐。

- [ ] `END main`这句话表明了proc main是整个文件的入口了吗？

- [ ] 因为Windows库函数是stdcall的，所以要设置stdcall格式才能调用Windows函数？

- [ ] 是所有函数名都是在link时被解析的还是当前文件外的函数名在link时被解析？

OS有个loader加载可执行文件。

在`.inc`文件中可以用`C .NOLIST ... C .LIST`来让中间的代码不出现在listing file中。

`TBYTE`类型按照1字节2个十进制位的格式存储，最高字节只控制正负，支持18位十进制。

如何把浮点数存进`TBYTE`里面？

```masm
.data
posVal REAL8 1.5
bcdVal TBYTE ?
.code
fld posVal
fbstep bcdVal
```

必须要使用`.data?`才能缩小可执行文件的大小。

凡是`=`右边不是一个整数的，都应该用`EQU`声明。

`EQU`不能被重定义，但是`=`和`TEXTEQU`可以被重定义。

`lahf`、`sahf`

- [ ] `.data`那里是地址0吗

`ALIGN 2`让之后开始的地址对齐到2的整倍数。

- [ ] 我们在32位中总是使用NEAR pointer吗？

- [ ] `PTR`到底是个怎样的类型？有哪些用法？

跳转指令主要是通过`ecx`和`eflags`的状态决定跳转。

`LOOP`指令的目标地址只能距离当前地址`[-128,127]`的位置。

- [ ] 它和手写`ECX`和`jmp`的区别？它是硬件实现的？

如果`ecx`初值是0，`loop`会下溢出错。

如果在procedure中使用`label::`，就可以让这个label在procedure外被看见。

- [ ] `jecxz`必须要在刚操作过`ecx`后使用吗？

