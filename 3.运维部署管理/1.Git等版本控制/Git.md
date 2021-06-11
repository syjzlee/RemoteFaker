<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Git](#git)
  - [公司常用的GitFlow工作流程](#%E5%85%AC%E5%8F%B8%E5%B8%B8%E7%94%A8%E7%9A%84gitflow%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B)
    - [1.项目启动](#1%E9%A1%B9%E7%9B%AE%E5%90%AF%E5%8A%A8)
    - [2.准备开发环境](#2%E5%87%86%E5%A4%87%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)
    - [3.测试流程](#3%E6%B5%8B%E8%AF%95%E6%B5%81%E7%A8%8B)
    - [4.发布流程](#4%E5%8F%91%E5%B8%83%E6%B5%81%E7%A8%8B)
    - [5.hotfix流程](#5hotfix%E6%B5%81%E7%A8%8B)
  - [Git在自己的服务器上创建远程私人仓库](#git%E5%9C%A8%E8%87%AA%E5%B7%B1%E7%9A%84%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E5%88%9B%E5%BB%BA%E8%BF%9C%E7%A8%8B%E7%A7%81%E4%BA%BA%E4%BB%93%E5%BA%93)
    - [1. 创建管理私人仓库的linux用户](#1-%E5%88%9B%E5%BB%BA%E7%AE%A1%E7%90%86%E7%A7%81%E4%BA%BA%E4%BB%93%E5%BA%93%E7%9A%84linux%E7%94%A8%E6%88%B7)
    - [2. 利用已经创建好的linux用户git，在服务器创建git仓库](#2-%E5%88%A9%E7%94%A8%E5%B7%B2%E7%BB%8F%E5%88%9B%E5%BB%BA%E5%A5%BD%E7%9A%84linux%E7%94%A8%E6%88%B7git%E5%9C%A8%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%88%9B%E5%BB%BAgit%E4%BB%93%E5%BA%93)
    - [3. 实现免密码公钥配对方式进行git clone](#3-%E5%AE%9E%E7%8E%B0%E5%85%8D%E5%AF%86%E7%A0%81%E5%85%AC%E9%92%A5%E9%85%8D%E5%AF%B9%E6%96%B9%E5%BC%8F%E8%BF%9B%E8%A1%8Cgit-clone)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Git
## [公司常用的GitFlow工作流程](https://www.cnblogs.com/fangsmile/p/11535302.html)
涉及到的角色有git管理员、开发人员、测试人员。(往往git管理员一般也是开发人员比如开发主管)
对应涉及到的版本控制环境 git管理员的本地仓库、开发人员的本地仓库、测试人员的本地仓库、以及远程仓库。
### 1.项目启动
**概述:第一次创建本地git仓库然后关联推送到远程仓库**  
**参与角色:Git管理员**  
**涉及环境:Git管理员本地仓库、远程仓库**  
**涉及分支:main或者main, develop分支**
1. 如果是新建的本地项目，需要初始化仓库
    - git init  把项目目录变成git可以管理的仓库，ls -all可以看到隐藏的.git目录
    - git add . 
    - git commit -m '项目启动说明'
2. github官网/个人团队的云服务器 创建远程仓库 
    - github官网
        - 利用可视化界面按钮直接创建就行
    - 如果是个人或团队的云服务器
        - git init --bare RemoteFaker.git   //创建了名为RemoteFaker的仓库   
         
    本质github上新建的远程仓库也仅仅是一个远程服务器上新建的git仓库，如果是自己的项目团队且有自己的服务器或者云服务器完全可以自己搭建远程git仓库；可以照着我这篇[文章](http://dazuili.cn/blog/32)实践下，
包括创建专门管理远程仓库的linux用户，创建团队的私人仓库，授予成员ssh公钥配对进行clone的权限等。
3. 新建的/已存在的本地仓库与远程空仓库进行关联
    - git remote add origin git@github.com:syjzlee/RemoteFaker.git
    - 如果是私人远程仓库: git remote add origin gitUser@域名:/***/gitUser/gitRepo/gittest.git  // gitUser是创建私人仓库的linux用户 
4. push到远程仓库
    - git branch -M main    //这是因为本地初始化init的分支名称默认是master，而远程github官网新建的仓库默认分支名称是main. -M强制重命名当前分支。
    - git push -u origin main  //-u 表示之后的git push操作默认推送到master分支，即git push==git push origin master。
    - git branch -a //查看本地以及远程git的所有分支。
5. 新建开发分支develop并推送到远程git仓库
    - git branch develop
    - git push origin develop:develop
    - git branch -a   
     
![GitFlow流程](https://img-blog.csdnimg.cn/img_convert/dd02e21f448173ca9649312dcfe27818.png)
    
### 2.准备开发环境
**概述:新项目需要第一次克隆项目的开发分支代码(develop)**
**参与角色:Git管理员(配置权限)，开发人员(迁出dev开发分支，开始执行项目)**  
**涉及环境:Git管理员本地仓库、开发人员的本地仓库、远程仓库**  
**涉及分支:develop分支**
1. 开发人员直接克隆项目的dev分支到本地，本地有且仅有一条dev分支
    - git clone -b develop git@github.com:syjzlee/RemoteFaker.git
2. 开发人员先克隆项目，然后再pull拉取dev分支到本地分支。
    - git clone git@github.com:syjzlee/RemoteFaker.git
    - git pull origin develop:local_develop  //拉取远程分支到本地新建的local_develop分支上。没有分支key:key参数则为当前分支。

![GitFlow流程](https://img-blog.csdnimg.cn/img_convert/c25205f4e503c28be7651157b96999dc.png)
    
### 3.测试流程
**概述:多个人员协作开发解决冲突以后进入测试流程**
**参与角色:Git管理员(配置测试release分支)，开发人员+测试人员(迁出release测试分支，测试修改bug)**  
**涉及环境:git管理员的本地仓库、开发人员的本地仓库、测试人员的本地仓库、以及远程仓库。**   
**涉及分支:develop分支,release分支**
1. 提测，申请测试，Git管理员基于开发完的远程的develop分支迁出新的本地release分支并推送到远程
    - git branch release   //需要在develop分支上新建release分支
    - git push origin release:release
    - 如果发现新建的release分支不是在develop分支基础上建立的，需要执行删除操作的话:
        - git push origin -d release  // 删除远程分支release
        - git branch -d release //删除本地分支release
        - 重新迁移到develop分支上建立release分支并推送，但是git管理员发现在当前分支hotfix上有修改，则：
            - git stash 或者 git stash save "修改提示信息，便于之后恢复"  //stash命令可以把工作区和暂存区的内容保存在堆栈中
            - git checkout develop  //切换回develop分支再进行最上面的新建release并push远程即可。
            - git checkout hotfix //切回原先的hotfix分支  
            - git stash pop 或者 git stash apply stash@{id}  //恢复hotfix当时修改的内容，继续改bug。stash堆栈可以恢复到任意指定的分支上。
    - Git管理员通知测试人员以及开发人员在该release分支上进行测试以及修改bug
2. 测试，测试人员从远程仓库迁出该release分支到本地进行测试
    - git clone -b release git@github.com:syjzlee/RemoteFaker.git
3. 修改Bug，开发人员从远程仓库迁出该release分支到本地仓库，修改bug后再提交到远程
    - git pull origin release:release
    - 修改Bug
    - git push origin release:release
4. Bug校验，测试人员接着从远程仓库迁出该release分支到本地进行bug校验。
    - git pull origin release:release  
    
![GitFlow流程](https://img-blog.csdnimg.cn/img_convert/42ab33c1856081002cdca4b27d02fe4b.png)
    
### 4.发布流程
**概述:所有Bug关闭以后，进行版本发布**
**参与角色:Git管理员(合并release以及master分支，更新合并develop和release分支，删除release分支)**  
**涉及环境:git管理员的本地仓库、以及远程仓库。**   
**涉及分支:release分支、master分支、develop分支**
1. Git管理员迁出测试通过的release分支到本地仓库。
    - git pull origin release:release
2. 将本地的release分支合并到本地master分支，然后推送到远程仓库
    - git checkout main
    - git merge release
    - git push origin main:main
3. 给本地master分支打Tag，推送到远程
    - git tag -a v0.0.1 -m 'v0.0.1 更新GitFlow流程'  //发布版本要用带附注的标签，私人时候采用轻量标签
    - git tag   //查看所有标签, tag是git版本库的一个标记，指向某个commit的指针。
    - git push origin v0.0.1  //推送标签到远程仓库。
4. 迁出最新的develop分支到本地，用于合并。将本地的release分支合并到本地的develop分支，推送到远程仓库。
    - git pull origin develop:develop
    - git merge release
    - git push origin develop:develop
5. 删除本地的release分支。推送到远程。
    - git branch -d release     //删除本地分支
    - git push origin -d release    //删除远程分支
    
![GitFlow流程](https://img-blog.csdnimg.cn/img_convert/72a603649f9f159e9a409f86fedd1c5c.png)
    
### 5.hotfix流程
**概述:线上Bug紧急修复流程**
**参与角色:Git管理员、开发人员、测试人员**  
**涉及环境:git管理员的本地仓库、开发人员本地仓库、测试人员本地仓库看以及远程仓库。**   
**涉及分支:master分支、hotfix分支、develop分支**
    
1. 申请hotfix 测试申请hotfix,Git管理员基于最新的master tag迁出新的本地hotfix分支并推送到远程
    - git pull origin main:main   # 将线上远程的主分支的代码拉取到本地，此时相应的tag也拉取到了
    - git checkout -b hot  //git tag可以查看本地标签， git ls-remote --tags origin查看远程tag
    - git checkout -b hotfix_20210701 v0.0.1 # tag和branch有相似地方，都可以git checkout branch/tag,但是tag不可修改只是一个固定标记，想要修改得迁出新的分支。
    - git push origin hotfix_20210701:hotfix_20210701

2. 修改bug， 开发人员迁出hotfix分支到本地，用于修改bug并提交到远程
    - git pull origin hotfix_20210701:hotfix_20210701
    - 修改bug
    - git push origin hotfix_20210701:hotfix_20210701
    
3. 测试，测试人员迁出hotfix分支到本地测试，用于验证bug
    - git pull origin hotfix_20210701:hotfix_20210701
    - 打包测试,验证bug

3. 测试通过，Git管理员合并hotfix分支到main，然后发布main并打上tag，最后将hotfix分支合并到develop。
    - git pull origin hotfix_20210701:hotfix_20210701  //将远程hotfix分支更新的代码迁出到本地
    - git checkout main
    - git merge hotfix_20210701  //合并到main分支
    - git push origin main:main // 推送到远程
    - git tag -a v0.0.2  -m '修改线上BUG****'  //为本次的hotfix带标签
    - git push origin v0.0.2   // tag推送到远程
    - git checkout develop  
    - git merge hotfix_20210701 //合并hotfix分支到本地develop
    - git push origin develop   //develop推送到远程
    - git branch -d hotfix_20210701 //删除本地hotfix分支
    - git push origin -d hotfix_20210701   //推送到远程服务器  
    
![GitFlow流程](https://img-blog.csdnimg.cn/img_convert/678a40929149303cbcaf89235d3263cf.png)

## [Git在自己的服务器上创建远程私人仓库](http://dazuili.cn/blog/32)
[linux服务器配置用户公钥登录](http://dazuili.cn/blog/31)   

### 1. 创建管理私人仓库的linux用户
    - useradd git  // 默认会在配置的$HOME环境目录下创建一个目录git
    - passwd git   // 给普通用户git设置密码，gitclone时候会使用的到
    - su git
    
### 2. 利用已经创建好的linux用户git，在服务器创建git仓库
    - mkdir ~/git/gitRepo   //git用户目录下创建存储git私人仓库的目录
    - cd ~/git/gitRepo
    - git init --bare gitTest.git   //创建了名为gitTest的仓库
    - git clone git@域名:/***/git/gitRepo/gitTest.git  //用户可以通过这种结构clone代码(此时是需要linux的git用户密码的)
     
### 3. 实现免密码公钥配对方式进行git clone
    - cd ~/git/.ssh    // 在/git目录下 创建/.ssh目录，用来存放配对连接权限的公钥
    - touch authorized_keys  //.ssh/目录下创建该文件， 这个路径信息要一致，因为/etc/ssh/sshd_config的配置中这么写的
    - chmod 600 authorized_keys  // 修改权限后，将要连接的用户的公钥写入authorized_keys中
    - chmod 700 ../ssh
    - su root
    - vim /etc/ssh/sshd_config  //配置sshd_config文件
        - AllowUsers root git  //在该文件中加入该行，使linux git用户可以使用~/git/.ssh/authorized_keys登录，然后进行克隆。
    - service sshd restart     //重启服务使配置生效
    - 现在用户讲本地机器的公钥给远程仓库管理员，添加到authorized_keys文件中即可实现远程免密clone了，类似GitHub的配置公钥。




