# git-commands
- [Commit message的建议格式](#commit-message-----)
- [git checkout](#git-checkout)
    + [切换分支/commit](#-----commit)
    + [创建新分支](#-----)
    + [创建local branch来track remote-tracking branch](#--local-branch-track-remote-tracking-branch)
      - [标准方法](#----)
      - [简化方法](#----)
      - [简化方法的简化](#-------)
- [git push](#git-push)
      - [如果远程仓库没有这条本地分支](#--------------)
      - [如果本地的历史改变了，只能强制push](#---------------push)
      - [删除远程分支](#------)
- [git reset --mode #commit/branch](#git-reset---mode--commit-branch)
- [修改历史](#----)
      - [将当前修改融合进上一个commit](#-----------commit)
      - [对多个commit进行修改](#---commit----)
- [git branch](#git-branch)
      - [查看所有本地分支](#--------)
      - [查看所有本地分支及其与与remote-tracking branch的关联情况](#------------remote-tracking-branch-----)
      - [为已存在的local branch设置remote branch](#-----local-branch--remote-branch)
- [子模块](#---)
  * [添加子模块](#-----)
  * [clone含子模块的仓库](#clone-------)
  * [对子模块进行操作](#--------)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


---
# Commit message的建议格式
Commit message由subject和body组成，标题与主体用空行隔开
1. subject
   - subject一般不超过50个字符（惜字如金、衡量commit的内容是否过多）
   - subject首字母大写
   - subject结尾不需要句号
2. body
   - body is optional
   - body每一行长度控制在72字符（主动换行）
# git checkout
### 切换分支/commit
```
git checkout branch/#commit
```
1. Checking out to another branch.
2. Checking out a specific commit will put the repo in a "detached HEAD" state.

### 创建新分支
```
git checkout -b branchname
```
基于当前commit创建并切换到新的branch
等价于
```
git branch branchname
git checkout branchname
```
### 创建local branch来track remote-tracking branch
有三种方法
#### 标准方法
```
git checkout -b <branch> <remote>/<branch>
```
该方法可以使tracking branch与remote tracking branch的名字不同
#### 简化方法
```
git checkout --track origin/serverfix
```
   上面方法的简化版，但是无法单独指定名字
#### 简化方法的简化
```
git checkout severfix
```
使用该方法创建tracking branch的条件：
1. The branchname specified doesn't exist
2. The branchname speciified exactly matches a name on only one remote

(注意没有`-b`，否则会创建一个普通branch，未与remote branch关联)


# git push
push命令用于与远程仓库交互
#### 如果远程仓库没有这条本地分支
```
git push -u origin branch-name 
```
等价于
```
git push --set-upstream origin branch-name
```
#### 如果本地的历史改变了，只能强制push
```
git push -f
```
#### 删除远程分支
```
git push origin -d branch-name
```
# git reset --mode #commit/branch
![image](https://user-images.githubusercontent.com/69393426/192718082-0641dc3f-43d7-451f-98d1-8c4a3c094d13.png)
At a surface level, `git reset` is similar in behavior to `git checkout`. Where
- `git checkout` solely operates on the HEAD ref pointer
- `git reset` will move the HEAD ref pointer and the current branch ref pointer.

`git reset`的默认参数是`mixed`和`HEAD`，因此直接执行`git reset`可以清空index tree。


# 修改历史
#### 将当前修改融合进上一个commit
```
git commit --amend -m ""
```
#### 对多个commit进行修改
```
git rebase -i HEAD~#
```
# git branch
#### 查看所有本地分支
```
git branch
```
#### 查看所有本地分支及其与与remote-tracking branch的关联情况
```
git branch -vv
```
#### 为已存在的local branch设置remote branch
```
git branch -u origin/serverfix
```
或
```
git branch --set-upstream-to origin/serverfix
```

# 子模块
## 添加子模块
给仓库内的path路径 添加submodule
```
git submodule add URL [path]
```
By default, submodules will add the subproject into a directory named the same as the repository. You can optionally add a different path at the end of the command if you want it to go elsewhere.
添加子模块后，执行git status会显示有两个新文件
- .gitmodules
   - a configuration file that stores the mapping between the project’s URL and the local subdirectory you’ve pulled it into
   - the repository’s branch to track
- submodule-name folder
  - the project folder entry
although is a subdirectory in your working directory, Git sees it as a submodule and doesn’t track its contents when you’re not in that directory. Instead, Git sees it as a particular commit from that repository.
## clone含子模块的仓库
By default you get the directories that contain submodules, but none of the files within them yet.
clone时同时初始化子模块：
```
git clone --recurse-submodules URL [path]
```
clone后初始化子模块：
```
git submodule update --init --recursive
```
## 对子模块进行操作
对于已经初始化过了的子模块（不是一个空文件夹），进入子模块文件夹后可以直接执行git命令。
也可以在主项目里对子模块进行操作：
对某一个子模块fetch + merge
```
git submodule update --remote
```
update所有子模块
```
git submodule update --init --recursive
```
