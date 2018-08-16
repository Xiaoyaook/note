# Git常用命令

> 这篇博文的总结更全面些[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)

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

## 冲突解决

具体操作
1. `git add <FILE>`
2. `git commit ...`
3. `git pull --rebase` （将你的版本加在最后的节点上）
    如果有冲突，会提示你。冲突分为两种，一种是在不同地方的冲突，通常情况下，git是可以自己合并的。另外一种是因为可能是同一个文件的编辑，git没法自动合并，需要二选一，这时候打开冲突的文件，手动编辑文件到可用的版本。然后。。
4. `git add ...` `git rebase --continue`
重复3，4直到所有的都solve
5. git push

---
[何方舟](https://www.zhihu.com/question/21215715/answer/31023475)
