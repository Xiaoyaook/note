# Git常用命令

* **git init** : 初始化 git 仓库
* **git status** : 查看当前git 仓库的一些状态
* **git add** : 提交文件到「暂存区」
* **git commit** : 正式提交文件
* **git reset** : 版本回退
* **git log** : 查看所有产生的 commit 记录
* **git branch** : 查看下当前分支情况
* **git branch <分支名称>** : 新建分支
* **git branch -b <分支名称>** : 新建分支并切换到新建的分支
* **git checkout <分支名称>** : 切换分支
* **git merge <分支名称>** : 把<分支名称>合并到当前分支
* **git branch -d <分支名称>** : 删除分支
* **git branch -D <分支名称>** : 强制删除分支
* **git tag** : 查看历史 tag 记录
* **git diff** : 查看代码改动
* **git stash** : 把当前分支所有没有 commit 的代码先暂存起来

## 与远程库连接

* **git remote -v** : 查看远程库的信息
* **git push origin branch-name** : 把该分支上的所有本地提交推送到远程库(可不推送到主分支,即可创建一个新分支)
* **git checkout -b branch-name origin/branch-name** : 在本地创建和远程分支对应的分支
* **git branch --set-upstream branch-name origin/branch-name** : 建立本地分支和远程分支的关联
* **git pull** : 从远程抓取分支
* **git push origin :branch-name** : 在github远程端删除一个分支(分支名前的冒号代表删除),注意删除远程分支后，如果有对应的本地分支，本地分支并不会同步删除！

## Git配置
* **git config --global core.quotepath false** : 设置显示中文文件名
* **git config --global color.ui true** : 给 Git 输出着色
* **git config --global user.name "username"** : 设置用户名
* **git config --global user.email "email"** : 设置邮箱
* **git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative** : 日志美化 