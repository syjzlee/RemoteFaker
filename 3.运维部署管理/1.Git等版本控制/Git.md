# Git
## 1. 第一次创建本地git仓库然后关联推送到远程仓库  
1. 如果是新建的本地项目，需要初始化仓库
    - git init  把项目目录变成git可以管理的仓库，ls -all可以看到隐藏的.git目录
    - git add . 
    - git commit - m '说明'
2. github官网/个人团队的云服务器 创建远程仓库 
    - github官网
        - 利用可视化界面按钮直接创建就行
    - 如果是个人或团队的云服务器
        - git init --bare gittest.git   //创建了名为gittest的仓库   
         
    本质github上新建的远程仓库也仅仅是一个远程服务器上新建的git仓库，如果是自己的项目团队且有自己的服务器或者云服务器完全可以自己搭建远程git仓库；可以照着我这篇[文章](http://dazuili.cn/blog/32)实践下，
包括创建专门管理远程仓库的linux用户，创建团队的私人仓库，授予成员ssh公钥配对进行clone的权限等。
3. 新建的/已存在的本地仓库与远程仓库进行关联
    - git remote add origin git@github.com:syjzlee/RemoteFaker.git
    - 如果是私人仓库: git clone gitUser@域名:/***/gitUser/gitRepo/gittest.git  // gitUser是创建私人仓库的linux用户 
4. push到远程仓库
    - git branch -M main    //这是因为本地初始化init的分支名称默认是master，而远程github官网新建的仓库默认分支名称是main. -M强制重命名当前分支。
    - git push -u origin main  //-u 表示之后的git push操作默认推送到master分支，即git push==git push origin master。
    - git branch -a //查看本地以及远程git的所有分支。

    
## 2. 版本迭代，新开项目新建git分支进行代码开发(比如dev分支、test分支等)。