# 撤销操作
## commit --amend
有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。这是我们可以运行带有`--amend`选项的`commit`命令尝试重新提交：
```
// 修改最近一次提交的提交信息
$ git commit --amend -m "XXX"
// 向最近一次提交添加漏掉的几个文件
$ git add forgotten_file
$ git commit --amend
```

## 取消暂存的文件
如果想取消暂存区域的某个文件，可使用以下命令来取消暂存：
```
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md
```
> 在调用`reset`命令时，如果加上`--hard`选项，如`$ git reset --hard HEAD~~`，会导致分支向前移动，导致文件丢失。

## 使用rebase -i修改提交
如果要修改第一次提交，需使用以下命令：
```
$ git rebase -i --root
```
如果要修改最近n次提交，需使用以下命令：
```
$ git rebase -i HEAD~n
```

## 撤销对文件的修改
如果你对某个文件做了修改，但还没保存至暂存区域，这时，你想撤销修改，将它还原成提交时的样子，该怎么做呢？可使用以下命令进行执行：
```
$ git checkout -- CONTRIBUTING.md
```
> 注：你需要知道 `git checkout -- [file]` 是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。 除非你确实清楚不想要那个文件了，否则不要使用这个命令。

# 远程仓库的使用
远程仓库是指托管在因特网或其他网络中的你的项目的版本库。 管理远程仓库包括了解如何添加远程仓库、移除无效的远程仓库、管理不同的远程分支并定义它们是否被跟踪等等。

## 查看远程仓库
通过运行`git remote`命令，查看已经配置的远程仓库服务器，它会列出指定的每一个远程服务器的简写。
```
$ git remote
origin
```
> 注：远程仓库服务器的默认名称为 `origin`。

可以通过指定选项 `-v`，显示需要读写远程仓库使用的Git保存的简写与其对应的URL。
```
$ git remote -v
origin  https://github.com/GongchuangSu/GitInAction.git (fetch)
origin  https://github.com/GongchuangSu/GitInAction.git (push)
```

## 添加远程仓库
通过运行 `git remote add <shortname> <url>` 指令，添加一个新的远程Git仓库，同时可以指定一个能够轻松引用的简写。
```
$ git remote add gs https://github.com/GrantSu/GitInAction
$ git remote -v
gs      https://github.com/GrantSu/GitInAction (fetch)
gs      https://github.com/GrantSu/GitInAction (push)
origin  https://github.com/GongchuangSu/GitInAction.git (fetch)
origin  https://github.com/GongchuangSu/GitInAction.git (push)
```

## 远程仓库的移除与重命名
通过运行 `git remote rename` 命令，可以修改一个远程仓库的简写名。如将`gs`重命名为`grant`，可以进行如下操作：
```
$ git remote rename gs grant
$ git remote
grant
origin
```
通过运行 `git remote rm` 命令，可以移除某个远程仓库：
```
$ git remote rm grant
$ git remote
origin
```

## 从远程仓库中抓取与拉取
通过运行 `git fetch [remote-name]` 访问远程仓库，从中拉取所有你还没有的数据。执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
```
$ git fetch gs
From https://github.com/GrantSu/GitInAction
 * [new branch]      master     -> gs/master
```

## 推送到远程仓库
通过运行 `git push [remote-name] [branch-name]` 命令，将要分享的项目推送到上游。如想要将 `master` 分支推送到 `origin` 服务器时，可使用以下指令：
```
$ git push origin master
```
> 注：只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。

## 查看远程仓库
通过运行 `git remote show [remote-name]` 命令，查看某一个远程仓库的更多信息。
```
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/GongchuangSu/GitInAction.git
  Push  URL: https://github.com/GongchuangSu/GitInAction.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

# 参考资料
https://git-scm.com/book/zh