#### Git控制器

1. 学习资料地址：liaoxuefeng.com/wiki/896043488029600/
2. Git是什么？
    git是目前世界上最先进的分布式版本控制系统。
3. git和SVN的区别？
    SVN：集中式服务器
    git：分布式服务器
    集中式和分布式控制系统有什么区别呢？
       · 集中式版本控制系统，版本库是集中存放在中央服务器的，干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新版本，然后开始干活，干完活之后，再把自己的活推送给中央服务器。它最大的缺点就是必须联网了才能工作，网速快慢对它上传时的速度有影响。
       · 分布式版本版本控制系统没有“中央服务器”，每个人电脑上都是一个完本的版本库，这样你工作时就不需要联网了，因为版本库就在你自己的电脑上。那么如果多个人它如何协作呢？比如你在自己电脑上修改了文件A，你的同事也在他电脑上修改了文件A，这时，你们只需要把各自的修改推送给对方，这样就可以互相看到对方的修改了。
         总结：和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人的电脑里都是一个完整的版本库，某一个人的电脑坏点了也不要紧，只要从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器如果出现了问题，所有人都没法工作了。
       在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

4. 初始化身份
    因为每一台电脑都是一个仓库，你要跟别人的电脑交互需要有一个身份。
    - 因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址
        git config --global user.name "regina"
        git config --global user.email "1802125750@qq.com"

5. 创建版本库（仓库）
    初始化仓库      git init     
    把文件添加到版本库      git add filename
    提交到仓库    git commit -m '注释（必须写）'
    - 这样就添加到仓库里面了，这样仓库就开始管理你添加进去的文件了，它对你对文件做的操作就有记录了

## 6.时光穿梭机
    作用：可以查看仓库的状态。
*    - git status命令可以让我们时刻掌握仓库当前的状态。   
    什么都没有表示已经添加并且提交到仓库了， 绿色表示修改了文件之后添加了但没有提交， 红色表示修改之后既没有添加也没有提交
    - git diff命令能查看具体改变内容。   当git add把文件添加到仓库之后，就不能查看上一次改变的具体内容了。
    - git log命令能查看历史版本
      git log --pretty=oneline  命令能将查看的历史版本信息放在一行，不那么乱
    - git reset --hard HEAD^ 命令能回到历史版本 后面的^，一个^表示回退一个版本，两个^^表示回退两个版本
      git reset --hard 版本号  回到指定的历史版本   eg:git reset --hard c9186be68358a37c996a9f1009c32b6a827a502d 
                        版本号也可以不用复制完全，复制一部分代表它是唯一的就可以

    7. 工作区和暂存区
    - 工作区就是本地仓库，即文件存放的区域。
    工作区有一个隐藏目录.git ，这个不顺眼工作区，而是git版本库。
    - git的版本库里面有很多东西，其中有stage（或者叫index）的暂存区，还有git为我们自动创建的第一个分支master，以及指向master的指针叫HEAD。
    * git add 就是把工作区修改的文件添加到暂存区；
      git commit就是把暂存区的所有内容提交到当前分支master，即提交到版本库

    8. 删除文件
    当文件被git管理之后，就不要直接右键删除了，要用命令  rm 文件名  删除某个文件
    - 工作区删除：
        rm 文件名  删除工作区的某个文件，这个命令只是在本地删除了，版本库中并没有删除，所以还需要版本库删除
    - 版本库删除：
        git rm 文件名
        git commit -m '删除文件'  即告知版本库删除，这时才真正的删除了。

    - 当仅仅执行了rm 文件名，只是删除了工作区里的某个文件，那么还可以把删除掉的这个文件给找回来。
        - git checkout -- 文件名  找回刚刚删除掉的工作区的那个文件   

## 9.Git分支管理
*   分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

* - （现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。）

其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。

*   - 创建与合并分支：
    - 创建一个分支并切换过去:  git checkout -b 分支名称     （相当于复制了整个主分支master 过来）
    - 查看分支：git branch 
    - 创建分支：git branch  分支名称
    - 切换分支：git checkout 分支名称
    - 合并某分支到当前分支：git merge 某分支
    - 删除分支： git brabch -d 分支名称

*  10. git解决冲突
    例如有主分支master和子分支dev，当两个分支都对某个文件作了修改，并且都添加提交到仓库时，这时会造成分支冲突。
    Git用<<<<<<<，=======，>>>>>>>标记出冲突的不同分支的内容。
    同时VSCode提供了
