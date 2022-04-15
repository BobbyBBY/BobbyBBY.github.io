---
layout: post # 页面类型
title: 服务器装机 # 标题
subtitle: Dell Poweredge r730 安装ubuntu2004 # 子标题
cover-img: https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/cover-inner.jpg # 封面图片
tags: [技术] # 标签
comments: true
catalog: true
image: https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/cover-outer.jpg
# gh-repo:
# gh-badge:
---

个人电脑有傻瓜装机软件，服务器可没有  

此教程用以记录服务器安装操作系统的过程  

---

### 此次教程使用的服务器

1.	服务器型号
Dell PowerEdge r730  

2.	硬件大致配置
处理器：至强系列  
内存：24GB 1333MHz  
磁盘：1TB  

3.	额外硬件
需要额外的显示器、键盘、鼠标（Lifecycle Control界面需要鼠标）  

4.	管理界面
F2 为miniBIOS  
F10 为Lifecycle Control，服务器管理系统  
F11 为BIOS  
提示：需要开机后就连续点按，以免错过识别窗口；先不插入启动U盘，进入相关界面后再插U盘，否则可能进不去相关界面  

5.	额外配置
此服务器无需RAID、已经配置好虚拟磁盘、已经配置好启动顺序，所以没有相关配置演示  
此服务器Lifecycle Control版本过低或驱动不完整，无法通过Lifecycle Control安装系统  


### 下载操作系统镜像文件

1.	Ubuntu  
[https://ubuntu.com/download/server](https://ubuntu.com/download/server)  

2.	Centos  
[https://www.centos.org/download/](https://www.centos.org/download/)  


### 制作启动盘

1.	启动盘选择
使用windows电脑制作启动盘  
启动盘至少需要2GB容量，依据操作系统体积，可能需要16GB及以上容量的U盘，推荐16GB  
制作过程会丢失U盘所有数据，包括U盘的所有分区  

2.	下载、安装并启动UltraISO工具
[https://www.ultraiso.com/](https://www.ultraiso.com/)  

3.	选择镜像文件
文件-打卡-选择镜像文件  
![0.1](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/0.1.png)  

4.	插入U盘

5.	写入硬盘映像
启动-写入硬盘映像  
![0.2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/0.2.png)  

6.	编辑配置
磁盘驱动器-选择合适的U盘  
写入方式-USB-HDD+  
写入  
![0.3](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/0.3.png)  

7.	等待制作完成


### 上机安装

1.	此教程以安装Ubuntu为例
参考  
[https://www.cnblogs.com/alonely/p/10299802.html](https://www.cnblogs.com/alonely/p/10299802.html)  

2.	如需安装CentOS
参考  
[https://www.yisu.com/zixun/131834.html](https://www.yisu.com/zixun/131834.html)  
[https://www.cnblogs.com/love-sherry/p/7216242.html](https://www.cnblogs.com/love-sherry/p/7216242.html)  

3.	如需设置RAID、虚拟磁盘、启动顺序
参考  
[https://www.cnblogs.com/love-sherry/p/7216242.html](https://www.cnblogs.com/love-sherry/p/7216242.html)  

4.	提前配置
此教程使用的服务器无需RAID、已经配置好虚拟磁盘、已经配置好启动顺序，所以没有相关配置演示  

5.	启动服务器前插入启动盘
![1](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/1.jpg)  

6.	启动后自动进入U盘装机界面
![2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/2.jpg)  
![3](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/3.jpg)  

7.	选择语言
推荐English  
![5](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/5.jpg)  

8.	选择键盘布局
推荐English（US）  
![6](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/6.jpg)  

9.	配置网络
![8](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/8.jpg)  
    *	选择对应网口  
    *	Subnet（子网掩码，使用后缀表示法）：例192.168.1.0/24  
    *	Address（IP地址）：例192.168.1.101  
    *	Gateway（网关）：例192.168.1.1  
    *	Name servers（DNS）：192.168.1.1  


10.	配置代理
推荐跳过  
![9](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/9.jpg)  

11.	配置镜像
修改apt-get源，后续可以修改，推荐跳过  
![10](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/10.jpg)  

12.	配置存储系统
由于该服务器文件系统已经配置好，所以这里跳过  
![11](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/11.jpg)  
![12](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/12.jpg)  
![13](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/13.jpg)  

13.	配置初始化用户名和密码
![14](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/14.jpg) 
    *	Your name：识别名，不重要
    *	Your server‘s name：主机名，与其他设备通信时的主机名
    *	Pick a username：操作系统登录名
    *	Choose a password：登录操作系统的密码
    *	Confirm your password：确认密码
 

14.	高级扩展
付费购买高级扩展，在此输入token，推荐跳过  
![15](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/15.jpg)  

15.	配置SSH
推荐安装SSH，勾选“Install OpenSSH server“，以便后续通过SSH连接  
![16](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/16.jpg)  

16.	配置快照
这些是服务器环境中常见的快照。使用空格选择或取消选择，按Enter键可查看可用的软件包、发布者和版本的详细信息。推荐跳过  
![17](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/17.jpg)  

17.	等待配置完成
直至底部出现“Reboot Now”  
![18](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/18.jpg)  
![19](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/server_installation/19.jpg)  

18.	拔掉启动U盘
“Reboot Now”  

19.	开机
第一次开机会有初始化配置过程，直至可以登录为止  


### 常见问题


1.	开机慢
开机卡在“A start job is running for wait for network to be Configured”  
参考  
[https://blog.csdn.net/baidu_19452317/article/details/99297399](https://blog.csdn.net/baidu_19452317/article/details/99297399)








