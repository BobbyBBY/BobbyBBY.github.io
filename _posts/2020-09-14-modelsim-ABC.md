---
layout: post # 页面类型
title: ModelSim 10.4 仿真入门实例 # 标题
subtitle: 轻量级硬件仿真软件 # 子标题
cover-img: /img/modelsimABC/cover.png # 封面图片
tags: [modelsim, 技术] # 标签
comments: true
catalog: true
---
这篇文章以一个实例带大家入门ModelSim 10.4。  

---

## 说明

语言：Verilog HDL  
参考：[https://mp.weixin.qq.com/s?__biz=MzAwOTU5NjY5OQ==&mid=401524002&idx=1&sn=a7c6c8307a807b65260c44caf201f676&scene=6#wechat_redirect](https://mp.weixin.qq.com/s?__biz=MzAwOTU5NjY5OQ==&mid=401524002&idx=1&sn=a7c6c8307a807b65260c44caf201f676&scene=6#wechat_redirect)  


## 打开软件

打开ModelSim软件，进入主界面  
![modelsim](/img/modelsimABC/modelsim.png)  

![main](/img/modelsimABC/main.png)  

## 新建项目

进入ModelSim主窗口后，选择File菜单下的“New->Project”，新建一个项目。在弹出的对话框中，给该工程命名并指定一个存放的路径，应避免出现中文  
![newproject1](/img/modelsimABC/newproject1.png)  

![newproject2](/img/modelsimABC/newproject2.png)  

## 进行开发

1. 在新建的目录中，使用别的编辑器进行开发。例如VS Code  
   ![dev1](/img/modelsimABC/dev1.png)  

2. 若使用VS Code进行开发，建议下载下图所示的插件  
   ![dev2](/img/modelsimABC/dev2.png)  

## 添加文件

1. 开发完成后，将新的文件加入刚才的项目中。  
2. 右击工作区（右击空白部分、文件也都可以），“Add to Project->Existing File”  
   ![addfile1](/img/modelsimABC/addfile1.png)  

3. 可以同时选中多个文件  
   ![addfile2](/img/modelsimABC/addfile2.png)  
   
   ![addfile3](/img/modelsimABC/addfile3.png)  

## 编译

1. 右击工作区，“Compile->Compile All”。也可以选择某个文件后，单独编译一个文件，“Compile->Compile Selected”  
   ![compile1](/img/modelsimABC/compile1.png)  

2. 编译能拦截语法和部分逻辑问题  
   ![compile2](/img/modelsimABC/compile2.png)  

3. 双击控制台红色提示部分，可以看到具体的错误提示  
   ![compile3](/img/modelsimABC/compile3.png)  

4. 修改完成后重新编译，可以看到文件的status都是绿色的对勾  
   ![compile4](/img/modelsimABC/compile4.png)  

## 添加仿真配置文件

只有新建项目或项目发生重大改变后才需要新建、修改仿真配置文件。右击工作区，“Add to Project->Simulation Configuration”  
![configuration1](/img/modelsimABC/configuration1.png)  

1. 点击右下角“Optimization Options”  
2. 打开选项卡“Options”  
3. 选择“Optimization Level”为“Disable optimizations(-O0)”  
4. 点击右下角“OK”  
   ![configuration2](/img/modelsimABC/configuration2.png)  

5. 再去掉复选框“Enable optimization”  
6. 展开“work”目录  
7. 选择Test Bench文件，这个例子中是“test_counter8”  
8. 点击右下角“Save”  
9.  如果不关闭优化选项的话，有时候ModelSim软件会报错，导致不能正常进行仿真  
    ![configuration3](/img/modelsimABC/configuration3.png)  


## 进入仿真界面	

双击新建的仿真配置文件  
![sim1](/img/modelsimABC/sim1.png)  

![sim2](/img/modelsimABC/sim2.png)  

## 根据需要添加波形显示

1. 在左侧“sim”选项卡中，选择需要添加波形显示的模块  
2. 右击“Objects”的空白部分，“Add to->Wave->Signal in Region”，以添加该模块所有信号量。也可以右击某个信号量，“Add Wave”。  
   ![wave1](/img/modelsimABC/wave1.png)  

   ![wave2](/img/modelsimABC/wave2.png)  

3. 一个信号量可以重复被添加波形显示，可以右击示波器某个信号，对其进行编辑  
   ![wave3](/img/modelsimABC/wave3.png)  

4. 如果有多个模块，可以同时将不同模块的信号量添加到一个示波器  
   ![wave4](/img/modelsimABC/wave4.png)  

5. 如果示波器不够大，也可以新建第二个示波器。右击“Objects”的空白部分，“Add Wave New”  
   ![wave5](/img/modelsimABC/wave5.png)  

## 根据需要修改信号类型

右击示波器某个信号，“Radix->……”  
![redix](/img/modelsimABC/redix.png)  

## 开始仿真

在控制台输入开始仿真命令“run 1us”。时间根据自己设计而定。仿真结束后，在示波器上出现了波形显示。如果想要新添加信号，需要重新仿真（步骤12）后才能看到新的波形。  
![run](/img/modelsimABC/run.png)  

## 查看波形

1. 点击示波器，左右滑动，可以看到某时刻数值  
   ![wave6](/img/modelsimABC/wave6.png)  

2. 先点击示波器以选中焦点，再点击工具栏的放大镜，可以缩小放大时间轴。  
   ![wave7](/img/modelsimABC/wave7.png)  

   ![wave8](/img/modelsimABC/wave8.png)  

## 修改、编译、重新仿真

1. 如果出现逻辑错误，需要打开编辑器修改代码。  
2. 修改完成后，选中左侧“Project”选项卡，可以看到被修改的文件“Status”变为了问号，需要重新编译（步骤5）。  
   ![modify1](/img/modelsimABC/modify1.png)  

3. 点击工具栏的“Restart”按钮后，再进行仿真  
   ![modify2](/img/modelsimABC/modify2.png)  

4. 也可以双击仿真配置文件从头开始仿真（不建议，除非工程发生了重大改变）。即重新选择信号量及数据类型。  
   ![modify3](/img/modelsimABC/modify3.png)  


## 退出仿真

控制台命令，“quit -sim”  

## 打开现有项目

1. 菜单栏，“File->Open…”  
   ![exist1](/img/modelsimABC/exist1.png)  

2. 找到项目目录，选择文件类型，“Project Files (*.mpf)”  
   ![exist2](/img/modelsimABC/exist2.png)  

3. 打开mpf文件即可打开现有项目  
   ![exist3](/img/modelsimABC/exist3.png)  
   
   ![exist4](/img/modelsimABC/exist4.png)  
