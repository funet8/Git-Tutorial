# Git入门和使用规范

此文档根据xmind生成的

## GIT基础知识

### [强烈建议所有开发者看Git官方文档](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)

- https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6
- 开发既熟悉又陌生的工具

### 1.什么是git

- 世界上最先进的分布式版本控制系统（没有之一）
- 特点

	- 1.直接记录快照，而非差异比较

		- Git 不按照以上方式对待或保存数据。反之，Git 更像是把数据看作是对小型文件系统的一系列快照。 在 Git 中，每当你提交更新或保存项目状态时，它基本上就会对当时的全部文件创建一个快照并保存这个快照的索引。 为了效率，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个 快照流

	- 2.近乎所有操作都是本地执行

		- 在 Git 中的绝大多数操作都只需要访问本地文件和资源，一般不需要来自网络上其它计算机的信息

	- 3.Git 保证完整性（哈希SHA-1校验）

		- Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希）。
		- Git 中使用这种哈希值的情况很多，你将经常看到这种哈希值。 实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。

	- 4.Git 一般只添加数据

		- 你执行的 Git 操作，几乎只往 Git 数据库中 添加 数据。 你很难使用 Git 从数据库中删除数据，也就是说 Git 几乎不会执行任何可能导致文件不可恢复的操作。
		- 但是一旦你提交快照到 Git 中， 就难以再丢失数据，特别是如果你定期的推送数据库到其它仓库的话。

- 流行原因

	- 1. 虽然git命令多，但可以用其他带界面的客户端代替命令，比如source tree用起来就很方便，直接点点鼠标就可以了。

2. git操作分支方便，便于采用工作流的方式多人合作开发，开发新功能的时候，每个人都在各自分支上开发，开发完毕后再合并到主分支，有助于减少代码冲突。向主分支合并时可以通过限制权限，强制review过的代码才被合并，可以确保主分支更稳定。

3. git是分布式的，本地有完整的代码仓库，所以有时定位问题时想查看历史版本，可以瞬间切换过去（不需要等待网络下载）。同样的原因，在断网的情况下，使用git仍然能继续提交代码，不影响工作。

作者：alpha
链接：https://www.zhihu.com/question/23376868/answer/277725676
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### 2.git有什么用

- 版本控制

	- 你在编写一个文档时，
如第一次写了一个文档： 1，2，3，4，5
然后你觉得写错了，改成了1，2，3，4，6
过一段时间后你又觉得有点不对劲，又改成了2，1，3，4，4
经过n次修改后最后成了4，2，5，3，7
这时候你突然发现一开始写的1，2，3，4，5才是对的，这时候因为修改次数过多，你已经无法使用撤销操作返回了，就又要重新写了。版本控制就是在这时候使用的，在你每次写完的时候，可以使用版本控制工具记录下版本。当你后面你想撤销的时候，可以使用版本号快捷的返回原版本（也叫版本回退）

- 分支管理

	- 当你写完一个版本后入1，2，3，4，5
这时候你觉得可能的发展会有两个1，2，3，4，5，6或者1，2，3，4
你暂时不确定你想要哪个，想两个都要实现下去，这时候就可以使用分支管理，创建一个新的分支2，
主支上是1，2，3，4，5，6。分支上是1，2，3，4
然后实现到最后发现原来分支2才是对的，主支上是错的，这时候就可以合并分支2，相当于舍弃了主支，分支2变成了主支。反之如果分支2是错的，则要舍弃分支2。

- 文件冲突

	- 当两个分支对同一个文件进行修改时，提交合并时就会造成冲突
简单点说，假设从主支上分出了两个分支如a，b
a的代码此时和b的代码一致，修改a中的代码，然后提交到主支，没问题。
然后修改b的代码，再提交到主支中，这时候就会出现问题，a的代码和b的代码系统不知道选择哪个作为新的代码。因为a，b分支是从之前的主支分出来的，属于同一级别上的，所以无法说b后提交就会覆盖a。
这时候只能手动修改ab冲突的部分，然后重新提交冲突的文件

### 3.Git的诞生发展

- 到了 2005 年，开发 BitKeeper 的商业公司同 Linux 内核开源社区的合作关系结束，他们收回了 Linux 内核社区免费使用 BitKeeper 的权力。这就迫使 Linux 开源社区(特别是 Linux 的缔造者 Linus Torvalds)基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。
- 大神Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！一个月之内，Linux系统的源码已经由Git管理了！牛是怎么定义的呢？大家可以体会一下。
- Git迅速成为最流行的分布式版本控制系统，尤其是2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub，包括jQuery，PHP，Ruby等等。

### 4.集中式和分布式

- 本地版本控制系统

	- 拷贝副本

		- 许多人习惯用复制整个项目目录的方式来保存不同的版本，或许还会改名加上备份时间以示区别。

- 集中化的版本控制系统

	- 代表

		- CVS、Subversion

			- 如图

	- 特点

		- 有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。 多年以来，这已成为版本控制系统的标准做法。

	- 优点

		- 开发者知道其他人的进度
		- 管理员分配开发者权限容易

	- 缺点

		- 单点故障
		- 网络限制

- 分布式版本控制系统

	- 代表

		- Git、Mercurial、Bazaar 

			- 如图

	- 特点

		- 客户端并不只提取最新版本的文件快照， 而是把代码仓库完整地镜像下来，包括完整的历史记录。
		- 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

	- 优点

		- 安全性高

			- 集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了

		- 分支管理
		- 快速

			- 要浏览项目的历史，Git 不需外连到服务器去获取历史，然后再显示出来——它只需直接从本地数据库中读取。 你能立即看到项目历史。
			- 如果你想查看当前版本与一个月前的版本之间引入的修改， Git 会查找到一个月前的文件做一次本地的差异计算，而不是由远程服务器处理或从远程服务器拉回旧版本文件再来本地处理。

		- 简单易用

	- linux开源社区

		- 目标

			- 1.速度
2.简单的设计
3.对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
4.完全分布式
5.有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

### 5.git的三种状态

- 已提交（committed）

	- 已提交表示数据已经安全地保存在本地数据库中。

- 已暂存（staged）

	- 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

- 已修改（modified）

	- 已修改表示修改了文件，但还没保存到数据库中

### 7.工作目录、暂存区域以及 Git 仓库

- 工作目录

	- 工作区是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

- 暂存区

	- 暂存区是一个文件，保存了下次将要提交的文件列表信息，一般在 Git 仓库目录中。 按照 Git 的术语叫做“索引”，不过一般说法还是叫“暂存区”。

- Git 仓库

	- Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，复制的就是这里的数据。

- 如图

	- 

- **GIT工作流程**

	- 1.在工作区中修改文件
	- 2.将你想要下次提交的更改选择性地暂存，这样只会将更改的部分添加到暂存区
	- 3.提交更新，找到暂存区的文件，将快照永久性存储到 Git 目录。

## GIT起步

### [1.安装 Git](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)

- 在 Linux 上安装
- 在 macOS 上安装
- 在 Windows 上安装

### [2.初次运行 Git 前的配置](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)

### [3.获取帮助](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E8%8E%B7%E5%8F%96%E5%B8%AE%E5%8A%A9)

- git help <verb>

	- git add -h
	- git help config

### [4.获取 Git 仓库](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%8E%B7%E5%8F%96-Git-%E4%BB%93%E5%BA%93)

- 在已存在目录中初始化仓库
- 克隆现有的仓库

### [5.记录每次更新到仓库](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93)

- 文件两种状态

	- 已跟踪

		- 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后， 它们的状态可能是未修改，已修改或已放入暂存区。简而言之，已跟踪的文件就是 Git 已经知道的文件。

	- 未跟踪

		- 工作目录中除已跟踪文件外的其它所有文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态，因为 Git 刚刚检出了它们， 而你尚未编辑过它们。

- 检查当前文件状态

	- $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  nothing to commit, working directory clean
	- 这说明你现在的工作目录相当干净。换句话说，所有已跟踪文件在上次提交后都未被更改过。
	- $ echo 'My Project' > README
    $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Untracked files:
    (use "git add <file>..." to include in what will be committed)

    README

nothing added to commit but untracked files present (use "git add" to track)
	- 在状态报告中可以看到新建的 README 文件出现在 Untracked files 下面。 未跟踪的文件意味着 Git 在之前的快照（提交）中没有这些文件；Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文件”。 

- 跟踪新文件

	- git add README

- 暂存已修改的文件

	- 现在我们来修改一个已被跟踪的文件。 如果你修改了一个名为 CONTRIBUTING.md 的已被跟踪的文件，然后运行 git status 命令，会看到下面内容：
	- $ git status
    On branch master
    Your branch is up-to-date with 'origin/master'.
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

    new file:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md

- 状态简览

	- git status

		- 有些繁琐

	- git status -s

		- 紧凑的输出
		- $ git status -s
 M README         修改过的文件前面有 M 标记。
 MM Rakefile        输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态
 A  lib/git.rb          新添加到暂存区中的文件前面有 A 标记
 M  lib/simplegit.rb
 ?? LICENSE.txt   新添加的未跟踪文件前面有 ?? 标记

- **忽略文件**

	- .gitignore忽略

		- 不希望他们出现在为跟踪文件列表中

			- 日志文件
	临时文件
	缓存文件
	填充图片，大视频等
	配置文件等

	- .gitignore

		- $ cat .gitignore
	*.[oa]         #告诉 Git 忽略所有以 .o 或 .a 结尾的文件
	*~               # Git 忽略所有名字以波浪符（~）结尾的文件
	/errpage/*
	/log/*
		- *
	!public/
	!.gitignore

	- .gitignore规范

		- 所有空行或者以 # 开头的行都会被 Git 忽略。

可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。

匹配模式可以以（/）开头防止递归。

匹配模式可以以（/）结尾指定目录。

要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。
		- 例如

			- # 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf

	- [github中的gitignore](https://github.com/github/gitignore)
	
		- https://github.com/github/gitignore

- 查看已暂存和未暂存的修改

	- git diff

		- git diff 能通过文件补丁的格式更加具体地显示哪些行发生了改变

	- git diff --staged 

		- 这条命令将比对已暂存文件与最后一次提交的文件差异：

- 提交更新

	- git commit

- 跳过使用暂存区域
- 移除文件
- 移动文件

	- git mv file_from file_to

### [6.查看提交历史](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93)

- 查看提交历史

	- git log

		- 不传入任何参数的默认情况下，git log 会按时间先后顺序列出所有的提交，最近的更新排在最上面。 正如你所看到的，这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

	- git log -p -2

		- 其中一个比较有用的选项是 -p 或 --patch ，它会显示每次提交所引入的差异（按 补丁 的格式输出）。 你也可以限制显示的日志条目数量，例如使用 -2 选项来只显示最近的两次提交：

	- git log --stat

		- --stat 选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。

- 限制输出长度

	- git log --since=2.weeks
	- 用 --author 选项显示指定作者的提交
	- 用 --grep 选项搜索提交说明中的关键字
	- git log --pretty="%h - %s" --author='Junio C Hamano'  \
--since="2008-10-01"  --before="2008-11-01" --no-merges -- t/

### [7.撤消操作](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%92%A4%E6%B6%88%E6%93%8D%E4%BD%9C)

- 撤消操作

	- git commit --amend

- 取消暂存的文件
- 撤消对文件的修改

### [8.远程仓库的使用](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)

- 查看远程仓库

	- git remote
git remote -v

- 添加远程仓库

	- git remote add <shortname> <url> 添加一个新的远程 Git 仓库

- 从远程仓库中抓取与拉取

	- git fetch <remote>

		- 这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

- 推送到远程仓库

	- git push <remote> <branch>

- 查看某个远程仓库

	- git remote show <remote> 

		- 查看某一个远程仓库的更多信息

- 远程仓库的重命名与移除

	- git remote rename

		- $ git remote rename pb paul
$ git remote
origin
paul

### [9.打标签](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)

- Git 可以给仓库历史中的某一个提交打上标签，以示重要
- git tag

	- git tag -l "v1.8.5*"

- 创建标签

	- 轻量标签

		- 轻量标签很像一个不会改变的分支——它只是某个特定提交的引用。

	- 附注标签

		- 而附注标签是存储在 Git 数据库中的一个完整对象， 它们是可以被校验的，其中包含打标签者的名字、电子邮件地址、日期时间， 此外还有一个标签信息，并且可以使用 GNU Privacy Guard （GPG）签名并验证。 通常会建议创建附注标签，这样你可以拥有以上所有信息。但是如果你只是想用一个临时的标签， 或者因为某些原因不想要保存这些信息，那么也可以用轻量标签。

	- 创建附注标签

		- $ git tag -a v1.4 -m "my version 1.4"
	$ git tag
	v0.1
	v1.3
	v1.4
		- 通过使用 git show 命令可以看到标签信息和与之对应的提交信息：

- 后期打标签
- 共享标签
- 删除标签
- 检出标签

### 10.Git 别名

### [11.Git 分支](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)

- 分支简介
- 分支创建

	- 很简单，它只是为你创建了一个可以移动的新的指针。
	- git branch testing
	- Git 又是怎么知道当前在哪一个分支上呢

		- 也很简单，它有一个名为 HEAD 的特殊指针。 请注意它和许多其它版本控制系统（如 Subversion 或 CVS）里的 HEAD 概念完全不同。 在 Git 中，它是一个指针，指向当前所在的本地分支（译注：将 HEAD 想象为当前分支的别名）。
		- git branch 命令仅仅 创建 一个新分支，并不会自动切换到新分支中去

- 分支切换

	- $ git branch
* master
  testing
	- git checkout testing

- 新建分支并且切换过去

	- git checkout -b <newbranchname> 

### [**12.分支的新建与合并**](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

- 实际工作流

	- 让我们来看一个简单的分支新建与分支合并的例子，实际工作中你可能会用到类似的工作流。 你将经历如下步骤：

    1.开发某个网站。
    2.为实现某个新的用户需求，创建一个分支。
    3.在这个分支上开展工作。

正在此时，你突然接到一个电话说有个很严重的问题需要紧急修补。 你将按照如下方式来处理：

1.切换到你的线上分支（production branch）。
2.为这个紧急任务新建一个分支，并在其中修复它。
3.在测试通过之后，切换回线上分支，然后合并这个修补分支，最后将改动推送到线上分支。
4.切换回你最初工作的分支上，继续工作。

- 新建分支
- **分支的合并**

	- git merge

		- $ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)

- 遇到冲突时的分支合并

	- 如果你在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们。
	- $ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.

- [解决冲突](https://www.cnblogs.com/gavincoder/p/9071959.html)

	- git冲突的场景

		- 情景一：多个分支代码合并到一个分支时；
		情景二：多个分支向同一个远端分支推送代码时；
		- 相同分支的同一个文件不同人修改，推送代码时

### [13.分支管理](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%A1%E7%90%86)

- 分支管理

	- git branch
	- git branch -v

		- 如果需要查看每一个分支的最后一次提交

	- git branch --merged

		- 查看哪些分支已经合并到当前分支

	- git branch --no-merged

		- 查看所有包含未合并工作的分支

### [14.分支开发工作流](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E5%BC%80%E5%8F%91%E5%B7%A5%E4%BD%9C%E6%B5%81)

- 开发分支

	- 许多使用 Git 的开发者都喜欢使用这种方式来工作，比如只在 master 分支上保留完全稳定的代码——有可能仅仅是已经发布或即将发布的代码。 他们还有一些名为 develop 或者 next 的平行分支，被用来做后续开发或者测试稳定性——这些分支不必保持绝对稳定，但是一旦达到稳定状态，它们就可以被合并入 master 分支了。 这样，在确保这些已完成的主题分支（短期分支，比如之前的 iss53 分支）能够通过所有测试，并且不会引入更多 bug 之后，就可以合并入主干分支中，等待下一次的发布。

### [15.远程分支](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E8%BF%9C%E7%A8%8B%E5%88%86%E6%94%AF)

- 远程分支
- 推送
- 跟踪分支
- 拉取
- 删除远程分支

## 常用命令

### 克隆到本地-git clone

- git clone git@192.168.1.9:liuxingxing/jenkins_test.git
git clone https://gitee.com/funet8/spider-flow.git

### 提交数据

-   添加到暂停区
        提交当前目录下的所有文件
            git add .

        提交当前仓库所有文件
            git add *
        
        指定目录或文件
            git add dirname test.php hello.txt

    添加到当前分支
        git commit -m '注释'

    提交到远程仓库
        git push

### 撤销修改

- git checkout .   # 放弃所有修改
git checkout test.php   # 放弃test.php文件修改
git clean -fd   # 放弃新创建的目录或文件

如果已经添加到暂停区了怎么撤销？两步完成（git add test.php）
git reset HEAD test.php
git checkout test.php

### 拉取分支数据

- 默认master分支
git pull

指定分支
git pull origin master

git pull   是获取并且更新版本库以及工作区。

git fetch  只从远程仓库获取用于更新版本库，工作区不更新。
fetch只会拉取远程分支最新版本，不做merge操作
  git fetch origin test
  git checkout test

git merge  是将本地版本库更新至工作区。

### 版本回退

- 回退到上一个版本
        git reset --hard HEAD^
    指定版本号（如果电脑有重启，使用 git reflog）
        git log
            commit 4aa614980a3db998f3f6299f7c22e82f4e248e27
            Author: Gfx <Gfx@Gfx.com>
            Date:   Thu Aug 27 21:47:26 2015 +0800

                gitignore
        
            commit d496317fc6e0de1697bcebd1dcd0120eaac5b578
            Author: Gfx <Gfx@Gfx.com>
            Date:   Thu Aug 27 21:45:32 2015 +0800
        
                del temp
        
            commit 2663f5a91403065f83091087286d9bd7c2368afb
            Author: Gfx <Gfx@Gfx.com>
            Date:   Thu Aug 27 21:31:48 2015 +0800
        
                dev update
        
        比如我们回退到 d496317fc6e0de1697bcebd1dcd0120eaac5b578 版本号不用写全，git会自动取找，前几位就行
            git reset --hard d496317fc
                HEAD is now at d496317fc dev update

回退成功后提交到远程仓库
        git push origin master

### 文件夹/文件管理

- 文件删除
    git rm test.php

删除文件夹以及文件夹下面的文件
    git rm -r dirname

文件命名
    git mv test.php new_file_name.php
    
    提交到本地仓库
    git commit -m '文件命名'

提交到远程仓库
    git push 

### 分支管理

- 1.创建分支
git branch develop
例如：
$ git branch
* dev/20200815-修复数据库问题
  master
  online

2.切换分支
git checkout develop
当前分支前面标记一个*号
* develop
master

3.创建分支并且切换到新创建的分支
git checkout -b develop



4.查看本地分支
git branch

5.查看远程分支
git branch -a

6.重命名本地分支名称
git branch -m develop new_name

7.推送本地分支到远程
git push origin develop

8.删除本地分支
git branch -d develop

9.删除远程分支
git push origin --delete develop
git push origin :develop   [git v1.7.0之前] 

10.合并某分支到当前分支
git checkout master
git merge develop

### 拉取远程分支并创建本地分支

- git checkout -b 本地分支名x origin/远程分支名x
新建分支（并且切换到该分支）
git checkout -b test_dev2
提交文件到主分支or其他分支

git add .
git commit -m"woshinibaba"
git push origin master
或者
git push origin test_dev3 #提交到test_dev3分支

查看所有远程分支

git branch -r

### 标签管理

- 1.创建标签
    git tag v_0.0.1

2.创建历史版本标签
    查看历史提交的commit id
        git log --pretty=oneline --abbrev-commit
            97ccfa5 test
            4aa6149 gitignore
            d496317 del temp
            2663f5a dev update

    比如要对2663f5a打标签
        git tag v_0.0.1 2663f5a

3.查看所有标签
    git tag

4.提交一个标签到远程
    git push origin v_0.0.1

5.提交所有未提交到远程的标签
    git push origin --tags

6.删除本地标签
    git tag -d v_0.0.1

7.删除远程标签
    git push origin --delete tag v_0.0.1
    git push origin :refs/tags/v_0.0.1   [git v1.7.0之前]

8.获取远程标签
    git fetch origin tag v_0.0.1

### 暂时提交到缓存

- 提交修改的数据到缓存
    git stash

查看缓存数据列表
    git stash list

恢复数据并删除缓存数据
    git stash pop

恢复数据不删除缓存数据
    git stash apply

多次stash后可以使用序号恢复
    git stash apply stash@{0}

删除缓存数据
    git stash drop

## 使用规范

### 主要规范

- 1.开发不同功能需创建不同分支，如果涉及到多人开发，需提交到远程仓库一起在新的分支中开发。
2.合并分支前打tag标签，版本号明确(非必须)。
3.提交代码注释写详细
4.请勿将大文件（视频、图片等）等非必须文件提交到版本库
5.请勿将日志、临时文件提交到版本库
6.研发必须具备的解决版本冲突的能力
- [分支策略](https://www.liaoxuefeng.com/wiki/896043488029600/900005860592480)

	- 分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

	- 所以，团队合作的分支看起来就像这样：

### 操作文档

- 遵循《GIT版本库操作手册及管理规范》

### 开发流程图

- 基本

	- 

- 分支开发

	- 

### 小技巧

- 免密码提交方法

	- SSH-KEY密钥
	- 修改项目下.git/config Git保存账号密码
	- WIN10自动保存密码

- git、github、gitee、gitlab的区别

	- git 是一种版本控制系统，是一个命令，是一种工具。
	- github和gitee是一个基于git实现在线代码托管的仓库，向互联网开放，增值服务要收费。
	- gitlab 类似 github，一般用于在企业内搭建git私服，要自己搭环境。
实现一个自托管的Git项目仓库，可通过Web界面进行访问公开的或者私人项目
	- Bitbucket 个大厂 BAT自建的git库

- github上一些公开文件夹的含义

	- dist 编译后的文件，可以理解为压缩发布版

src 源码文件，适合动手能力强的小米

docs 文档

examples 示例文件

test 测试文件

.gitignore 告诉git哪些文件不要上传到Github 上

LICENSE.txt 授权协议

README.md 自述文件，整个项目的简介使用方法等

bower.json 包管理器配置文件

package.json npm包管理器配置文件

