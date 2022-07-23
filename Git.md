# Git

## 一、简介

1. 版本管理工具

2. svn是集中式（单点故障，容错能力较差），git是分布式的

3. 去中心化思想

4. 步骤：

   a. 从远程仓库中克隆Git资源作为本地仓库

   b. 从本地仓库中checkout代码然后进行代码修改

   c. 在提交前先将代码提交到暂存区

   d. 提交修改，提交到本地仓库，本地仓库中保存修改的哥哥历史版本

   e. 在修改完成后，需要和团队成员共享代码时，可以将代码push到远程仓库

5. 工作区

6. 版本库（.git）

   内有暂存区

## 二、基本配置

1. 系统配置（对所有用户都适用）

   存放在git的安装目录下：%Git%/etc/gitconfig；若使用git config 时用 --system选项，读写的就是这个文件

2. 用户配置（只适用于该用户）

   存放在用户目录下：~/gitconfig；若使用git config 时用 --global选项，读写的就是这个文件

3. 仓库配置（只对当前项目有效）

   当前仓库的配置文件（也就是工作目录中的.git/config文件）；若使用 git config 时用 --local 选项，读写的就是这个文件

## 工程区域

1. 版本库 （repository)

   在工作区中有一个隐藏目录.git，这个文件夹就是Git的版本库，里面存放了Git用来管理该工程的所有版本数据，也可以叫本地仓库。

2. 工作区 （working directory)

   日常工作的代码文件或者文档所在的文件夹。

3. 暂存区 （stage)

   一般存放在工程根目录 .git/index 文件中，所以也叫索引（index)

## 文件状态

1. 已提交 （committed)

   该文件已经被安全的保存在本地数据库中了。

2. 已修改 （modified）

   修改了某个文件，但还没有提交保存。

3. 已暂存 （staged）

   把已修改的文件放在下次提交时要保存的清单中。

## 常用命令

git branch 可以产看本地工程的所有git分支名称。 

git branch new_branch_name 和 git checkout -b 都可以用于新建分支（默认基于当前分支节点创建），后者会自动切换到新分支。

git branch -d branch_name 和 git branch -D branch_name 都可以用来删除本地分支，后者大写表示强制删除。

git checkout branch_name 用来切换分支，也叫 “检出”。

git pull origin remote_branch:local_branch 从远端服务器中获取某个分支的更新，再与本地指定的分支进行自动合并。（如果远程指定的分支和本地指定分支相同，则可直接执行 git pull origin remote_branch

git fetch origin remote_branch:local_branch 从远程服务器中获取某个分支的更新到本地仓库，并不会进行合并。

git merge branch_name 和 git rebase 从指定的分支（节点）合并到当前分支的操作。

git reset commit_id 撤销当前工作区中的某些 git add/commit 操作，将工作区内容回退到历史提交节点。

git checkout . 或者加文件名，用于回退本地所有修改而未提交的文件内容。
