## git笔记

* 1  安装  
    在https://git-scm.com/downloads上安装git
***
* 2 开始  
    * 2.1  开始创建库
    ``` 
        $ mkdir gitName  
        $ cd gitName
        $ pwd --显示当前目录
    
    ```
    * 2.2  把文件夹变成git库
    ```
        $ git init 把上面的目录编程git可以管理的仓库
    ```
    * 2.3  
    把文件添加到版本库
    * 2.4 添加文件
     ``` 
        $ git add readme.txt 
    ```  
        把文件修改添加到暂存区
    * 2.5 提交文件
    ```
         $ git commit -m "提交说明" 提交之前添加的文件
    ```  
        把全部内容从暂存区提交到当前分支
    * 2.6 查看状态
    ```
        $ git status
    ```  
        Change  or Untracked
    * 2.7 查看提交更新的内容
    ```
        $ git diff readme.txt
        
        查看文件
        $ cat readme.txt
    ```
    * 2.8 查看修改日志
    ```
        $ git log
        $ git log --pretty=oneline
    ```
    * 2.9 回退版本  
        根据提交的commitId 前几位，来进行回退
        ```
        $ git reset --hard sd234b
        
        $ git reset HEAD readme.txt
        把指定文件的暂存区的修改回退到工作区
        head 表示最新的版本
        ```
        相同语句 也可以用于取消回退  
    * 2.10 查看命令记录  
     也可用于回退/取消的操作
    ```
        $ git reflog
    ```
    * 2.11 丢弃工作区修改  
    ```
        $ git checkout -- readme.txt
        
        撤销工作区所有修改
        让此文件回到最近一次 git commit/add时的状态
    ```
    * 2.12 删除文件  
        ```
        $ rm readme.txt
        
        $ git status 查看
        
        选择 1. 确定删除
            $ git rm readme.txt
            $ git commit -m "remove readme.txt"
        选择 2. 错删
            $ git checkout -- readme.txt
            注意空格
        ```
        
    
* 3 工作区  
     * 3.1   
      　　Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
   * 3.2   
     　　用 **$ git diff HEAD -- readme.txt**
        来查看工作区和版本库里最新版本的区别。  
    * 3.3  
     一次commit，会把两次add`合并提交`到git.
* 4 远程仓库  
    　　使用远程仓库来进行分布式版本控制
    * 4.1 创建SSH Key
        ```
        $ ssh-keygen -t rsa -C "MyEmail@xxx.com"
        全部回车，默认设置即可
        .ssh目录中，id_rsa私钥(不可泄露)
                    id_rsa.pu公钥
        ```
    * 4.2 登录github  
        　　在GitHub个人设置>SSH>新建SSH key
        输入title，把公钥内容粘贴进文本框保存。
    * 4.2 在GitHub上创建新库  
        然后在本地仓库下运行：
        ```
            $ git remote add origin https://github.com/FFeat/learngit.git
            
        ```
    * 4.2 把本地所有内容推送到远程库  
        ```
        $ git push -u origin master
        
        ```
        　　　把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

        由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
        
        ```
        可以使用命令：
        $ git push origin master
        
        ```
* 5 从远程库克隆  
    * 5.1  新建远程库  
        本地运行命令：
    ```
        $ git clone https://github.com/FFeat/learngit.git
    ```
        然后就会在本地库的路径上创建learngit文件，并且放入拷贝的文件。
***
## 分支管理
### 一、创建分支
 
     $ git checkout -b dev
    -b参数表示创建并切换 相当于两条命令
    $ git branch dev
    $ git checkout dev
    
### 二、查看分支

    $ git branch
上述命令会列出所有分支，当前分支 会用*标记。
### 三、开始提交
    $ git add FileName
    $ git commit -m "分支提交"
分支工作完成后，可以切换回master分支  

    $ git checkout master  
### 四、准备合并：  
 
    $ git merge dev
    Fast forward模式合并
merge命令用于合并指定分支dev到当前分支。
Fast forward会在删除分之后，丢失分支信息。  
　　　可以用：  
　　　
    ``` 
    $ git merge --no-ff -m "merge with no-ff" dev
    ```  
--no-ff表示禁用快速合并。  
　　因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
    
### 五、准备删除分支  

    $ git branch -d dev
    然后再用
    $ git branch查看，result is OK!
### 六、无法自动合并分支
    首先解决冲突，再提交 合并。
    把Git合并失败的文件手动编辑为我们希望的内容，再提交。
    同
    $ git log --graph 查看分支合并图
## 分支策略
####  master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活。
　　都在dev分支上，版本发布时，再合并到master分支。
## bug分支
如果遇到在dev上的工作没结束，接了个紧急bug任务，需要创建别的分支来处理，可用git的stash功能，把当前工作区存储起来。  

    $ git stash
首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

    $ git checkout master
    $ git checkout -b inssue-101
然后开始修复bug,修复完成后，添加再提交  

    $ git add bugFilaName
    $ git commit -m "fix bug 101"
再切换到master分支，合并，并删除inssue-101分支  

    $ git checkout master
    $ git merge --no-ff -m "merge bug 101" issue-101
处理完bug后，开始原有dev的工作  
    
    $ git checkout dev
    $ git status 发现是空的
    $ git stash list 查看stash，发现工作区还在
    恢复之前存储的工作区：
    1.
        $ git stash apply (stash内容不会删除)
        需要配合
        $ git stash drop来删除
    2.
        $ gti stash pop (恢复的同事清空stash内容)
如果有多个stash，  
先用 $ git stash list 查看，然后恢复指定stash  

     $ git stash apply stash@{0}
     
## Feature分支
针对实验性的代码，在不影响主分支的情况下，最好用feature分支来开发。
#### 开始
    $ git checkout -b feature-developName
开发完毕后，  
    
    $ git add developName.py
    $ git status
    $ git commit -m "add feature xx"
切换回dev ,准备合并  

    $ git checkout dev
    .....
合并删除完成后，如果遇到特殊情况，需要销毁之前开发的功能。  

    $ git branch -d feature-developName
如果是没有合并过的分支，会销毁失败，那么强行删除：  

    $ git branch -D feature-developName
### 推送分支
把该分支上的所有本地提交推送到远程库。 

    $ git push origin master
    or 
    $ git push origin dev
        
### 抓取分支
如果遇到同事在相同的分支项目上做了修改，并且push到了远程上，会冲突。用git pull把最新的提交抓取下来，本地合并，再推送。
如果没有指定本地 分支和远程分支的链接，会pull失败。  

    $ git branch --set-upstream-to=origin/dev dev
    再
    $ git pull
解决冲突，再提交，再push.
#### Rebase
    $ git rebase
rebase操作可以把本地未push的分叉提交历史整理成直线
### tag标签
类似于分支，其实也是指向某个commit的指针.  

    $ git branch  切换到需要打标签的分支上
    $ git tab v1.0 打上标签
    $ git tab 查看所有标签
默认标签是打在最新的commit上。  
如果忘记打了，可找到历史commitID 然后打上。  

    $ git log --pretty=oneline --abbrev-commit
    $ git tag v0.9 f42s32
    $ git tag 查看，打上了
    $ git show v0.9 查看这个标签的信息
    
    $ git tag -a  v0.1 -m "版本说明" 1eak13
    再查看，就有了文字说明。
删除标签  

    $ git tag -d v0.1
    创建的标签，只存储在本地
    $ git push origin v1.0 推送标签至远程库
    $ git push origin --tags 推送所有标签至远程库
    
删除远程库标签，要先删除本地标签。  

    $ git tag -d v1.0
    $ git push origin : refs/tags/v1.0
    是否删除成功，可登陆网站查看
### 更好托管平台
先移除现有绑定的远程库。  

    $ git remote rm origin
    再关联其他的远程库
    $ git remote add origin  xxxxx
如果同时向关联两个托管库。  
先删除已关联的origin库。  

    $ git remote rm origin 
    $ git remote add github xxx
    $ git remote add gitee  xxx
    $ git remote -v 查看，已关联两库。
push:  

    $ git push github master
    $ git push gitee master
### 忽略文件
过滤垃圾/敏感文件的提交,  
　　在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。  

    # Windows:
    Thumbs.db
    ehthumbs.db
    Desktop.ini
如果想强制添加：  

    $ git add -f index.html
检查ignore文件：  

    $ git check-ignore -v index.html
### 创建快捷输入
用st 代替status:  

    $ git config --global alias.st status
    $ git config --global alias.co checkout
    $ git config --global alias.ci commit
    $ git config --global alias.br branch
    $ git config --global alias.last 'log -1'
    配置一个git last，让其显示最后一次提交信息
    $git lg 等于如下：
    $ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
#### 配置文件：
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

配置文件放哪了？每个仓库的Git配置文件都放在.git/config文件中。  

    $ cat .git/config 
## 搭建git服务器（简单了解）
搭建Git服务器
GitHub就是一个免费托管开源代码的远程仓库。但是对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用。

搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的apt命令就可以完成安装。

假设你已经有sudo权限的用户账号，下面，正式开始安装。

第一步，安装git：

    $ sudo apt-get install git
第二步，创建一个git用户，用来运行git服务：

    $ sudo adduser git
第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

    $ sudo git init --bare sample.git
Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

    $ sudo chown -R git:git sample.git
第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

    git:x:1001:1001:,,,:/home/git:/bin/bash
改为：

    git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：

    $ git clone git@server:/srv/sample.git
    Cloning into 'sample'...
    warning: You appear to have cloned an empty repository.
剩下的推送就简单了。