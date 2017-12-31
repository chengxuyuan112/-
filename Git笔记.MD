###　git常用命令整理

- 初始化本地库命令

        git init 


- 添加文件到库中

        git add a.xxx


- 提交文件到库中

        git commint -m"需要提交的说明理由"              


-查看库的文件状态

        git status

- 比较文件的

        git diff 1.txt

- 查看文件内容

        cat 1.txt
####  git的版本回退

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

 - 回退到上个版本

        $ git reset --hard HEAD^

 - 查看版本库的状态

        git log

 - 可以使用commitID来实现版本的切换

        git reset --hard commitid

 -查看记录每一次命令的commitid

    git  reflog    

 - 使用git像远程仓库github提交代码，这里注意第一次使用github的时候需要创建SSH Key来连接

 第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key

        $ ssh-keygen -t rsa -C "youremail@example.com"


 在github创建一个空的库 

         ①  $ git remote add origin git@github.com:michaelliao/learngit.git 使用这个命令与本地库关联起来

         ② $ git push -u origin master  往远程库推送


- 从远程库clone

           $ git clone git@xxxxx.git 远程库的名字



