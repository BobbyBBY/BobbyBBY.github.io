---
layout: post # 页面类型
title: ModelSim 10.X 安装教程 # 标题
subtitle: 轻量级硬件仿真软件 # 子标题
cover-img: /img/modelsiminstallation/cover.png # 封面图片
tags: [modelsim, 技术] # 标签
comments: true
catalog: true
---
这篇文章教大家如何以学习为目的安装、破解ModelSim 10.X。   
官网:  
[www.mentor.com](https://www.mentor.com)  

---

## 说明

测试过的系统：win10 64bit  
参考文章：[https://blog.csdn.net/weixin_42693097/article/details/90523692](https://blog.csdn.net/weixin_42693097/article/details/90523692)  

## 下载  

安装包大约500MB，安装后约1.2GB。  

1. 百度云（非年费会员限速）  
   链接：[https://pan.baidu.com/s/14ZxTLwfZKD_Qjjlk4PCkYQ](https://pan.baidu.com/s/14ZxTLwfZKD_Qjjlk4PCkYQ)  
   提取码：nxcm  
2. OneDrive（不限速，但需要梯子）  
   链接：[https://1drv.ms/u/s!AhsQ6cw4ZjWRiJxDklImteAMxlBJ0g?e=j4U541](https://1drv.ms/u/s!AhsQ6cw4ZjWRiJxDklImteAMxlBJ0g?e=j4U541)  
   提取码：nxcm  
3. 百度云（非年费会员限速，有个小广告）  
   链接：[https://pan.baidu.com/s/1k8f7dgQ2AYjssUDPTt3ToA](https://pan.baidu.com/s/1k8f7dgQ2AYjssUDPTt3ToA)  
   提取码：d36q  
4. 在官网下载学生版，无需破解，需要注册  
   链接：[https://www.mentor.com/company/higher_ed/modelsim-student-edition](https://www.mentor.com/company/higher_ed/modelsim-student-edition)  

## 解压

![decompression](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/decompression.png)  

## 运行安装程序  

1. 双击运行安装软件  
   ![install](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/install.png)  

2. 进入欢迎界面  
   ![welcome](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/welcome.png)  

3. 点击next  
   ![next](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/next.png)  

4. 选择安装路径，应避免中文  
   ![location](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/location.png)  

5. 阅读须知后点击agree  
   ![agree](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/agree.png)  

6. 安装中，请等待  
   ![installing](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/installing.png)  

7. 根据喜好添加桌面快捷方式  
   ![shortcut](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/shortcut.png)  

8. 添加环境变量，尽量选择yes  
   ![path](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/path.png)  

9.  点击no  
    ![HW](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/HW.png)  
    
10. 安装完成  
    ![finish](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/finish.png)  

## 破解

1. Crack文件夹中的这两个文件复制到安装目录的win64文件夹下面, 如有重名，直接覆盖  
   ![crack](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/crack.png)  
   
   ![crack_path](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/crack_path.png)  

2. 修改mgls64.dll的可读性，去掉只读复选框  
   ![mgls64](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/mgls64.png)  

3. 以管理员身份运行patch_dll.bat。如果命令行窗口一闪而过，或者没有出现步骤7的情况，请继续步骤4  
   ![patch1](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/patch1.png)  

   ![patch2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/patch2.png)  

4. 右击开始，使用管理员打开PowerShell  
   ![powershell](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/powershell.png)  

5. 进入ModelSim安装目录的win64目录，注意路径最好嵌在单引号内。如果没有单引号，无法进入带空格的路径。请手动输入下面的命令，如果直接复制下面这条命令至powershell，单引号会被过滤掉。  
   ```bat
   cd ‘C:\Program Files\ModelSim\win64’
   ```  
   ![powershell1](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/powershell1.png)  

   ![powershell2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/powershell2.png)  

6. 依次执行以下命令，注意第二行的英文句号  
   ```bat
   attrib -r mgls.dll
   MentorKG.exe -patch . 
   attrib +r mgls.dll
   ```  
   ![powershell3](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/powershell3.png)  

7. 执行第二条命令时会稍微卡顿，并弹出记事本。将记事本另存到ModelSim的安装目录。如有重名，直接覆盖。  
   ![txt](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/txt.png)  

   ![txt_path](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/txt_path.png)  
   
8. 新建环境变量（环境变量在“我的电脑–属性–高级系统设置–环境变量”），变量名为MGLS_LICENSE_FILE，变量值为上面LICENSE.TXT文件的路径，如我的是C:\Program Files\Modelsim\LICENSE.TXT  
   ![path1](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/path1.png)  

   ![path2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/modelsiminstallation/path2.png)  
