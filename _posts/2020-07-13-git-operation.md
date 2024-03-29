---
layout: post # 页面类型
title: Git操作 # 标题
subtitle: Git命令之创建、切换、回滚 # 子标题
cover-img: https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/cover.jpg # 封面图片
tags: [git,技术] # 标签
comments: true
catalog: true
image: https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/cover.jpg
# gh-repo:
# gh-badge:
---
Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。本文以生动的形式介绍Git的基本操作，包括创建仓库、分支切换、操作回滚等。  

---

### 简介  

* Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。  
* Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。  
* Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。  

可以把Git简单得比作游戏的存档系统，若存档系统是按照时间排序的，肯定早忘了谁是谁了，而Git这个存档系统则可以帮你管理存档间的关系。  

![game](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/game.png)  

下图是常见的Git分支示意图。这里与游戏存档不一样的就是Git的分支是可以合并的。下文就不要用游戏存档系统看待Git了，当涉及到分支合并的时候他们就已经不是同一个东西了。  

![git](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/git.png)  

Git对文件的管理涉及3个区域——工作区、缓冲区和历史记录区。  

1. **工作区**就是你修改文件的地方。  
2. **缓冲区**顾名思义就是提交前的缓存区，存在的意义是为了更好的控制commit的粒度。例如已经上线了功能1后，正在开发功能2的过程中发现功能1出现了bug，这时需要紧急修改功能1。修复完成后，如果将新功能1和开发了一半的功能2一起commit了，就会造成版本历史的混乱，这时只需要将新功能1相关的文件放入暂存区，只commit新功能1即可。这样即保证了commit的粒度。  
3. **历史记录区**即是存放历史版本的区域。  

下图是三个区域的关系：  
![rbw](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/rbw.png)

---

### 创建仓库

仓库就是上文 “ 历史记录区 ” 的存放地点，因此使用Git的第一步就是创建仓库。
基本操作有创建本地仓库、创建远程仓库、克隆远程仓库  

#### 在本地创建仓库

```shell
git init
```

创建仓库后，首次git commit默认创建master分支  

#### 在远程创建仓库

这里的远程指的是GitHub

1. 在repository页面点击 “ New ”
![newR](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/newR.png)  

2. 在之后的页面完善仓库名、介绍以及其他选项
![newR2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/newR2.png)  

除了在GitHub直接创建远程仓库外，还可以将本地仓库发布为远程仓库，这里不赘述（关键词 “ git发布本地仓库 ” ）。  

#### 克隆远程仓库

```shell
git clone [url]
```

克隆GitHub远程仓库时，在GitHub仓库页复制url，请注意选择 “ Use HTTPS ” 。如何使用SSH协议克隆参见[Git SSH使用SS加速](/2020-06-04-git-speedup)  

![clone](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/clone.png)  

1. 亦可以克隆别人发布在自己服务器的仓库，这时就需要别人给你提供url  
2. 克隆的远程仓库默认只有master分支，需要手动拉去其他分支  
3. 在分支阶段与远程仓库相关的命令大多涉及origin关键词  

---

### 分支切换

&nbsp;

#### 查看分支

查看本地所有分支：

```shell
git branch
```

查看本地＋远程所有分支：

```shell
git branch -a
```

&nbsp;

#### 创建分支

创建本地分支：  

```shell
git branch [分支名]
```

创建远程分支：  

![newB](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/newB.png)

#### 切换分支

```shell
git checkout [分支名]
```

&nbsp;

#### 更新分支

pull、fetch都可以，fetch是更新到本地仓库，pull是直接拉到工作区，即 pull = fetch + merge 。  

git fetch是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到本地分支中。  

git pull 则是将远程主机的最新内容拉下来后直接合并。  

冲突解决在**合并分支**里。  

示意图如下:  

![pull](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/pull.png)

指定分支名更新：  

```shell
git pull origin [远程分支名]:[自定义本地分支名]
git fetch origin [远程分支名]:[自定义本地分支名]
```

更新当前分支（前提是远程存在此分支）：  

```shell
git pull
git fetch
```

&nbsp;

#### 推送分支

推送分支即将本地内容上传到远程，一般要先fetch再push。因为涉及到团队合作时，如果别人在你上一次fetch后push了新的内容，你的这次push就会失败。  

指定分支名推送：  

```shell
git push origin [本地分支名]:[自定义远程分支名]
```

推送当前分支（前提是远程存在此分支）：  

```shell
git push
```

&nbsp;

#### 删除分支

删除分支的操作本地与远程是分别操作的。

删除本地分支：  

```shell
git branch -d [本地分支名]
```

删除远程分支：  

```shell
git push origin --delete [远程分支名]
```

&nbsp;

#### 合并分支

不能直接合并远程的分支，只能将两下载到本地，合并后再推送到远程  

合并分支：  

```shell
git merge [分支名]
```

将命令中指定的分支合并到当前分支，即不会影响[分支名]分支。  

合并分支很有可能会遇到**冲突！**  

冲突解决：  

如果出现冲突，只能手动更改。
冲突示意图：  

![conflic1](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/conflic1.png)  

如果遇到冲突，git会提示：  

![conflic2](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/conflic2.png)  

打开文件可以看到出现冲突的地方被git自动标注了：  

![conflic3](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/conflic3.png)  

这时只能手动修改出现冲突的地方，先修改尖括号间的内容，再删除尖括号（避免污染别的内容）。  

![conflic4](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/conflic4.png)  

#### 一种简单的分支维护方式

1. 在master分支维护用于发布的版本  
2. 在dev分支开发新功能，通过测试后merge到master分支  
3. 在test分支上随时对需要的地方进行测试  

![branch](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/branch.png)  

---

### 回滚操作

简介提到Git有存档功能，意味着它是可以回滚的。  

#### 撤回缓存区的内容

撤回缓存区仅仅是将缓存区的修改移到工作区，不会修改文件内容。  
如果 “ checkout ” 后面跟上文件名，就只撤回该文件的修改，如果跟上 “ . ” ，则撤回所有修改。  

```shell
git checkout -- filename
git checkout .
```

&nbsp;

#### 查看分支历史

有时候 git log 显示的内容太多，可以加上oneline参数。  
如果显示的内容超过一页，需要通过回车换行，输入 “ :q ” 退出。

```shell
git log
git log --oneline
git log --graph
```

![gitlog](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/gitlog.png)  

![gitlog-oneline](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/gitlog-oneline.png)  

#### 恢复到存档点

恢复存档点的意思是撤销该存档点**及**以后的所有操作，并将该状态设置为新存档点。  
这里的含义有点绕，看图更明了。  

![revert](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/revert.gif)  

```shell
git revert [存档点名称]
```

存档点名称可以不全，可以只填写前部分名称，只需要能够区分不同的存档点即可。  
例如使用 git log --oneline 命令查到的名称就是不全的。

#### 回滚到存档点

回滚存档点的意思是撤销并删除该存档点以后的所有操作。  
看图以注意reset与revert的区别。  

![reset](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/reset.gif)  

```shell
git reset [存档点名称]
```

---

### Git忽略

Git忽略通过 “ .gitignore ” 实现，如果在这个项目中有文件夹名或文件名符合 “ .gitignore ” 中的规则，那么该文件夹和文件就不会被提交到版本库中。  

#### 通配语法

基本的两个匹配方式如下，含义为忽略后缀为 “ .txt ” 的文件，并忽略 “ single_alpha_alpha/ ” 目录下的所有文件：  

```shell
*.txt
single_alpha_alpha/
```

.gitignore忽略规则的详细匹配语法[转载自CSDN](https://blog.csdn.net/dataiyangu/article/details/82781155)：  

1. 空格不匹配任意文件，可作为分隔符，可用反斜杠转义。  
2. 以 “ ＃ ” 开头的行都会被 Git 忽略。即#开头的文件标识注释，可以使用反斜杠进行转义。  
3. 可以使用标准的glob模式匹配。所谓的glob模式是指shell所使用的简化了的正则表达式。  
4. 以斜杠"/"开头表示目录；"/"结束的模式只匹配文件夹以及在该文件夹路径下的内容，但是不匹配该文件；"/"开始的模式匹配项目跟目录；如果一个模式不包含斜杠，则它匹配相对于当前 .gitignore 文件路径的内容，如果该模式不在 .gitignore 文件中，则相对于项目根目录。  
5. 以星号"\*"通配多个字符，即匹配多个任意字符；使用两个星号"\*\*" 表示匹配任意中间目录，比如\"a/\*\*/z\"可以匹配 a/z, a/b/z 或 a/b/c/z等。  
6. 以问号"?"通配单个字符，即匹配一个任意字符。  
7. 以方括号"\[\]"包含单个字符的匹配列表，即匹配任何一个列在方括号中的字符。比如\[abc\]表示要么匹配一个a，要么匹配一个b，要么匹配一个c；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配。比如\[0-9\]表示匹配所有0到9的数字，\[a-z\]表示匹配任意的小写字母）。  
8. 以叹号"\!"表示不忽略(跟踪)匹配到的文件或目录，即要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（\!）取反。需要特别注意的是：如果文件的父目录已经被前面的规则排除掉了，那么对这个文件用"\!"规则是不起作用的。也就是说"!"开头的模式表示否定，该文件将会再次被包含，如果排除了该文件的父级目录，则使用"\!"也不会再次被包含。可以使用反斜杠进行转义。  

* git对于.ignore配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效。  
* 如果某个文件以及被纳入版本管理，忽略规则对它无效  

&nbsp;

#### 已被纳入管理

某个文件已经被纳入版本管理，如何忽略它？  

首先设置好你的忽略规则，然后将这个文件移出项目目录，commit一次，再将其移回原位即可。  

#### 版本库瘦身

一般是问：某个超大文件在前期已经被纳入版本管理，如何忽略它以减小版本库的大小？  

高危操作：  

* 复制项目文件到新目录，重新创建一个仓库，抛弃之前所有的存档点。
* 将项目文件复制到一个新的地方（不包含.git文件夹，以及.gitignore文件），reset到某个想要的存档点，再将项目文件移回原目录

安全操作：  

* Git 自带的垃圾回收命令：  

   ```shell
   git gc
   ```

* 指定版本深度  

   想要减小版本库大小，一般是遇到clone缓慢的问题。可以参考 “ **已被纳入管理** ” ，在之后的clone中指定本版深度，只要高于这次删除大文件的版本就可以了。  

   以下命令指定深度为1，即只克隆最新的版本：  

   ```shell
   git clone [url] --depth 1
   ```

   下图表示在存档4之后，通过 “ **已被纳入管理** ” 方法在之后的版本中忽略了大文件，同时克隆时指定版本深度，就不会把曾经包含大文件的版本克隆过来了。  

   ![depth](https://bobbybby.oss-cn-zhangjiakou.aliyuncs.com/img/gitoperation/depth.png)  

* 直接加速  

   [Git SSH使用SS加速](/2020-06-04-git-speedup)  
