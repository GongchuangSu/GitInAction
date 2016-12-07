# 获取Git仓库
有两种取得Git项目仓库的方法：
1. 在现有的目录中初始化仓库
2. 克隆现有的仓库

## 在现有的目录中初始化仓库
进入该目录并执行以下命令
```
$ git init
```
该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。

## 克隆现有的仓库
克隆仓库的命令格式为`git clone [url]`
```
$ git clone https://github.com/GongchuangSu/HelloWeb
```
这会在当前目录下创建一个名为 “HelloWeb” 的目录，并在这个目录下初始化一个 .git 文件夹，从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。
> 注：初次克隆某个仓库的时候，工作目录中的所有文件都属于**已跟踪文件**，并处于未修改状态。

# 记录每次更新到仓库
工作目录下的每一个文件都不外乎这两种状态：**已跟踪**或**未跟踪**。已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于**未修改**，**已修改**或**已放入暂存区**。工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。

编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。我们逐步将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复。所以使用 Git 时文件的生命周期如下：

![](http://i.imgur.com/zMse8ig.png)

## 检查当前文件状态
要查看哪些文件处于什么状态，可以用 `git status` 命令。如果在克隆仓库后立即使用此命令，会看到类似这样的输出：
```
$ git status
On branch master
nothing to commit, working directory clean
```
> 注：如果当前工作目录有处于未跟踪的文件，上述命令会将其列出来。

## 跟踪新文件
使用命令 `git add` 开始跟踪一个文件。 例如，要跟踪 `README` 文件，运行：
```
$ git add README
```
此时再运行 `git status` 命令，会看到 `README` 文件已被跟踪，并处于暂存状态：
```
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md
```
只要在 `Changes to be committed` 这行下面的，就说明是**已暂存状态**。 如果此时提交，那么该文件此时此刻的版本将被留存在历史记录中。

## 暂存已修改文件
现在我们来修改一个已被跟踪的文件。如果你修改了一个名为`README.md`的已被跟踪的文件，然后运行`git status`命令，将会看到以下内容：
```
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md
```
文件`README.md`出现在`Changes not staged for commit`这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。 要暂存这次更新，需要运行 `git add` 命令。
如果没有暂存这次更新，而是直接准备好提交，值得注意的是，`README.md`文件同时出现在暂存区和非暂存区。那么提交的到底是哪个版本呢？实际上，Git只不过暂存了你运行`git add` 命令时的版本，如果你现在提交，`README.md`的版本是你最后一次运行`git add` 命令时的那个版本，而不是你运行`git commit`时，在工作目录中的当前版本。
> 注：`git add` 命令是一个多功能命令。一、可以用来跟踪新文件；二、把已跟踪的文件放到暂存区；三、用于合并时把有冲突的文件标记为已解决状态

现在我们运行 `git add` 将`README.md`放到暂存区，然后再看看 `git status` 的输出：
```
$ git add README.md
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.md
```
在 `Changes to be committed` 这行下面的，说明该文件是**已暂存状态**。

## 状态简览
你使用 `git status -s` 命令或 `git status --short` 命令，你将得到一种更为紧凑的格式输出。 运行 `git status -s` ，状态报告输出如下：
```
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```
说明：
- `A`：表示该文件是新添加到暂存区中的文件
- `??`：表示该文件是新添加的未跟踪文件
- `M`：分两种情况：靠左边的`M`和靠右边的`M`
 - 靠左边的`M`：表示该文件被修改了并放入了暂存区
 - 靠右边的`M`：表示该文件被修改了但还没有放入暂存区

> 注：如果出现两个`M`，那表示该文件在工作区被修改并提交到暂存区后又在工作区被修改了，所以在暂存区和工作区都有该文件被修改了的记录。

## 忽略文件
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。
```
$ cat .gitignore
*.[oa]
*~
```

## 查看已暂存和未暂存的修改
要查看未暂存文件的修改，可直接不加参数的输入`git diff`
```
$ git diff
```
要查看已暂存文件的修改，可用以下命令
```
git diff --cached
```
或
```
$ git diff --staged
```

## 提交更新
现在的暂存区域已经准备妥当可以提交了。 在此之前，请一定要确认还有什么修改过的或新建的文件还没有 `git add` 过，否则提交的时候不会记录这些还没暂存起来的变化。 这些修改过的文件只保留在本地磁盘。 所以，每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit`：
```
$ git commit 
```
另外，你也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行
```
$ git commit -m "First commit"
```

## 跳过使用暂存区域
尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 git commit 加上 -a 选项，**Git 就会自动把所有已经跟踪过的文件暂存起来一并提交**，从而跳过 git add 步骤：
```
$ git commit -a -m 'added new benchmarks'
```

## 移除文件
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。
如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。

# 三种状态和三个工作区域
Git有三种状态，文件可能处于其中之一：
1. 已修改（modified）：表示修改了文件，但还没保存到数据库中
2. 已暂存（staged）：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中
3. 已提交（committed）：表示数据已经安全的保存在本地数据库中

Git项目有三个工作区域：
1. Git仓库：Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。
2. 工作目录：对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
3. 暂存区域：它是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作“索引”，不过一般说法还是叫暂存区域。
![](http://i.imgur.com/hdYR0t1.png)

基本的 Git 工作流程如下：
1. 在工作目录中修改文件
2. 暂存文件，将文件的快照放入暂存区域
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录

# 参考资料
https://git-scm.com/book/zh