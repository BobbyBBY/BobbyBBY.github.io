---
layout: post # 页面类型
title: Git SS 加速 # 标题
subtitle: Git使用SS加速clone # 子标题
cover-img: /img/gitclonespeedup/cover.jpg # 封面图片
tags: [git, shadowsocks] # 标签
---
本方法讲述在linux命令行下的git-clone加速方法，有可视化桌面的版本亦可使用本教程，但是有更简单的方法，搜索关键词即可。  
有个小问题说在前面，ss代理必须一直打开，如果不想使用代理了，需要删除在ssh配置文件中修改的的内容，否则git将无法联网。  

# 本文目录：
1. GitHub远程克隆方式
2. 配置ssh
3. 安装shadowsocks以及插件
4. 配置shadowsocks
5. 加速！！！
  
# GitHub远程克隆方式  
GitHub可以选择https和ssh方式克隆仓库，https方式在更新私人仓库时需要每次输入账户和密码，ssh方式需要手动生成密钥，但是只需配置一次。  
https方式:  
在这里复制url  
![httpsurl](/img/gitclonespeedup/https.png)
  
然后执行git clone [url]
![https](/img/gitclonespeedup/githubhttps.png)
  
如果是私人仓库，以后的每次连接远程仓库进行操作的时候，都需要输入账户和密码。  

ssh方式:  
在这里复制url  
![sshurl](/img/gitclonespeedup/ssh.png)
  
然后执行git clone [url]
![sshnokey](/img/gitclonespeedup/sshnokey.png)
  
这时提示没有权限，因为我们没有配置ssh。  

# 配置ssh
首先，使用以下命令生成 SSH Key：
```
ssh-keygen -t rsa -C "[github账户]"
```
之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。  
![sshkey](/img/gitclonespeedup/sshkey.png)  
成功的话会在 ~/ 下生成 .ssh 文件夹，进文件夹，打开 id_rsa.pub，复制里面的 key，包括“ssh-rsa”。  
![sshkeycopy](/img/gitclonespeedup/sshkeycopy.png)
  
打开github设置页  
![githubsetting](/img/gitclonespeedup/githubsetting.png)

点击“SSH and GPG keys”-“New SSH key”，粘贴刚才复制的key并确认
![githubssh](/img/gitclonespeedup/githubssh.png)
![githubnewssh](/img/gitclonespeedup/githubnewssh.png)
![githubnewssh2](/img/gitclonespeedup/githubnewssh2.png)
  
为了验证是否成功，输入以下命令：
```
ssh -T git@github.com
```
如果看到成功提示，则表示连接成功。（本文没有提供此处的错误解决方案）
![gitsshsucceed](/img/gitclonespeedup/gitsshsucceed.png)
  
再执行git clone [url]，即可成功clone
![gitsshclone](/img/gitclonespeedup/gitsshclone.png)
  
这时，你是不是发现clone的速度特别慢……
   

# 安装shadowsocks以及插件

Arch Linux
```
sudo pacman -Syu
sudo pacman -S shadowsocks-libev simple-obfs
```

Ubuntu >= 16.10
```
sudo apt update
sudo apt install shadowsocks-libev simple-obfs
```

Ubuntu <= 16.04
```
sudo apt-get install software-properties-common -y
sudo add-apt-repository ppa:max-c-lv/shadowsocks-libev -y
sudo apt-get update
sudo apt install shadowsocks-libev -y

sudo apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libc-ares-dev libev-dev asciidoc xmlto automake git
git clone https://github.com/shadowsocks/simple-obfs.git
cd simple-obfs
git submodule update --init --recursive
./autogen.sh
./configure
make
sudo make install
cd ..
rm -rf simple-obfs
```

# 配置shadowsocks
账户申请地址  
如果下述地址申请有问题，麻烦手动填写邀请码，谢谢你这么优秀还填我的邀请码(●'◡'●)
```
8278528127
```  
[https://www.yunkly.com/home/ref/8278528127](https://www.yunkly.com/home/ref/8278528127)  

可以根据上述网站的教程界面通过命令加参数的方式自己启动shadowsocks  
  
当然也可以通过本文下述方式启动，我当然推荐下述方式啦  
  
在某路径下生成配置文件[文件名].json，其中填入账户申请网站提供的信息，一般来说会给你几个节点，有几个节点就配置几个文件
```
{
    "server":"……",
    "server_port":……,
    "local_port":1080,
    "password":"……",
    "timeout":60,
    "method":"aes-256-cfb",
    "fast_open":true,
    "plugin":"obfs-local",
    "plugin_opts":"obfs=tls;obfs-host=apple.com"
}
```

然后执行以下命令即可启动  
```
ss-local -c ……[文件名].json
```
使用bash命令再简化启动过程
配置好你的ss配置文件后，放入shadowsocks文件夹  
并生成命令shadowsocks.bash
```
if [ $1 == "us" ]
then
   ss-local -c /home/[username]/shadowsocks/us_config.json &
elif [ $1 == "jp" ]
then
   ss-local -c /home/[username]/shadowsocks/jp_config.json &
elif [ $1 == "hk" ]
then
   ss-local -c /home/[username]/shadowsocks/hk_config.json &
elif [ $1 == "sg" ]
then
   ss-local -c /home/[username]/shadowsocks/sg_config.json &
elif [ $1 == "kr" ]
then
   ss-local -c /home/[username]/shadowsocks/kr_config.json &
elif [ $1 == "tw" ]
then
   ss-local -c /home/[username]/shadowsocks/tw_config.json &
else
   echo "默认加载us"
   ss-local -c /home/[username]/shadowsocks/us_config.json &
fi
```
执行以下命令即可通过hk节点启动代理  
```
bash shadowsocks.sh hk
```
![ssstart](/img/gitclonespeedup/ssstart.png)

可能遇到的问题，提示  
This system doesn't provide enough entropy to quickly generate high-quality random numbers  
![errorrandom](/img/gitclonespeedup/errorrandom.png)
只需要按照提示安装软件即可
```
sudo apt-get install rng-tools
```
    
     

# 加速！！！

这时还需要配置代理才可以真正实现加速  
  
首先，需要安装connect-proxy命令
```
sudo apt-get install connect-proxy
```
root用户修改 /etc/ssh/ssh_config 文件  
普通用户修改  ~/.ssh/config 文件  
没有则新建文件，并将权限至少设为600/644  
文件追加，注意linux和windows空格、回车或者制表符的区别：
```
Host github.com *.github.com
    ProxyCommand connect-proxy -S 127.0.0.1:1080 %h %p
    # 这里的ip和端口都是你自己代理设置的ip和端口
    IdentityFile ~/.ssh/id_rsa
    # rsa公钥位置
    User git
```
再执行git clone [url]，可以看到速度飞起
![speed](/img/gitclonespeedup/speed.png)



祝大家加速愉快(●'◡'●)

