# git常用指令


### 如何在本地创建一个本地仓库并推送到github

```shell
 git init #通过git init命令把这个目录变成Git可以管理的仓库
git add *** #用命令git add告诉Git，把文件添加到仓库,可以多次add不同的文件
   （git add .）#添加所有文件
git commit -m "***" #用命令git commit告诉Git，把文件提交到仓库,简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。commit可以一次提交很多文件.
4. 登陆GitHub，创建一个新的仓库，（选择建立readme文件）
5. git remote add origin git@github.com:shilongshen/learngit.git #将本地库与远程库关联,其中learngit为仓库名
6. git pull --rebase origin master#将github中的README.md文件合并到本地代码目录中
7. git push -u origin master#把本地仓库的内容推送到GitHub仓库
```



### 将修改提交到仓库

```shell
git add  #(将文件提交到缓冲区)
#几种git add 命令：
git add -A #保存所有的修改
git add . #保存新的添加和修改，但是不包括删除
git add -u #保存修改和删除，但是不包括新建文件。

2.git commit -m '***'   #（将文件提交到本地仓库）

3.git push origin master #从现在起，只要本地作了提交，就可以通过命令把本地master分支的最新修改推送至GitHub
```



### 创建分支

```shell
git branch 分支名  #创建新的分支
git checkout 分支名 #切换分支
git branch -a #查看所有分支
git push origin 分支名  #将该分支提交到远程仓库
```



### 常见错误

1.error: failed to push some refs to 'git@github.com:shilongshen/test2.git'
解决方法：git pull --rebase origin master--将github中的README.md文件合并到本地代码目录中

2.fatal: Updating an unborn branch with changes added to the index.
解决方法：git commit -m "***"

3.error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.





![](https://gitee.com/shilongshen/xiaoxingimagebad/raw/master/img/20210501105332.jpeg)
