# git命令行操作指南（git指令及使用场景详解及git stash、git branch、git分支关联等）

现在版本控制使用git的挺多，之前常用SVN，偶尔使用Git也是使用可视化工具操作（sourcTree，IDE自带的Git功能），之前不求甚解，所以对git的了解相当浅薄，后来遇到问题只能一次一次去查资料，后来用的多了就觉得麻烦，所以整理一下git的相关命令行操作，以备后用！因为分值操作相关较多且稍复杂，统一放在文章后面

##### git最常用的基本命令

1.查看当前git版本（判断是否安装过git）



```undefined
git --version
```

![img](https://upload-images.jianshu.io/upload_images/6175320-31e60859b3e51885.png?imageMogr2/auto-orient/strip|imageView2/2/w/874/format/webp)

image.png

2.git下载代码



```php
git clone http://gitlab.tech.xxx.com/xxx/backend-view.git
```

![img](https://upload-images.jianshu.io/upload_images/6175320-cce9b194ea96ad85.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



项目地址在gitlab项目地址复制粘贴即可（全局配置权限后以后即可不用输入，不再赘述）

3.修改编辑项目常用



```csharp
// 查看当前仓库文件状态（常在提交文件之前查看，会显示新增文件删除文件，已修改文件等状态）
git status

// 添加文件
git add .    // 添加所有已修改文件
git add fileName   // 添加指定文件名的文件（可在git status返回中复制）

// 提交修改说明
git commit -m "修改的内容"    // 记录当前提交的主题 以便区分每次提交的内容

// 拉取代码
git pull    // 拉取代码  push之前pull一次代码  （尤其多人开发一定注意push之前先pull）
git pull origin <远程分支名>     // 将远程指定分支 拉取到 本地当前分支上
git pull origin <远程分支名>:<本地分支名>     // 将远程指定分支 拉取到 本地指定分支上
git pull origin    // 将与本地当前分支同名的远程分支 拉取到 本地当前分支上(需先关联远程分支)

// 推送代码
git push    // 推送代码到远程仓库
git push origin <本地分支名>    // 将本地当前分支 推送到 与本地当前分支同名的远程分支上（注意：pull是远程在前本地在后，push相反）
git push origin <本地分支名>:<远程分支名>     // 将本地当前分支 推送到 远程指定分支上（注意：pull是远程在前本地在后，push相反）
git push origin     // 将本地当前分支 推送到 与本地当前分支同名的远程分支上(需先关联远程分支)

git push --set-upstream origin      // <本地分支名>将本地分支与远程同名分支相关联
```

4.分支相关



```cpp
git branch      // 查看本地分支（名称前面加* 号的是当前的分支）
git branch -a      // 查看分支，远程分支会用红色表示出来（如果你开了颜色支持的话）
git branch -vv      // 查看本地分支和远程分支对应关系
git remote      // 列出所有远程主机
git remote update origin --prune      // 更新远程主机origin（gitlab有新分支，本地查看分支无法查看到的时候使用）
git branch -r       // 列出远程分支
git branch -vv       // 查看本地分支和远程分支对应关系
git checkout -b dev origin/dev      // 新建本地分支dev与远程dev分支相关联
```

4.1 新建分支相关



```cpp
git checkout -b newBranch
git push origin newBranch:newBranch    // 把新建的本地分支push到远程服务器，远程分支与本地分支同名（当然可以随意起名）：
```

4.2 删除分支相关
4.2.1 删除本地分支



```cpp
git branch -D newBranch    // 删除本地 newBranch 分支
git checkout newBranch    // 如果需要重新拉取远程的newBranch分支 执行
```

4.2.2 删除远程分支



```cpp
git push origin :delBranchName
git push origin --delete delBranchName
```



```csharp
// 切换分支,分支跟踪， 本地分支和远程分支的关系
git branch branchName     // 创建分支
git checkout branchName     // 切换分支
git branch -d branchName     // 删除本地分支

git branch -r -d origin/branch-name  
git push origin :branch-name     // 删除远程分支

// 如果远程新建了一个分支，本地没有该分支,git checkout --track origin/ branchName ，这时本地会新建一个分支名叫  branchName，会自动跟踪远程的同名分支 branchName。
git checkout --track origin/branchName

// 如果本地新建了一个分支 branchName，但是在远程没有。这时候 push 和 pull 指令就无法确定该跟踪谁，一般来说我们都会使其跟踪远程同名分支，所以可以利用 git push --set-upstream origin branchName ，这样就可以自动在远程创建一个 branchName 分支，然后本地分支会 track 该分支。后面再对该分支使用 push 和 pull 就自动同步。
git push --set-upstream origin branchName

// 合并分支（多人开发中，经常一人一个分支，各自在自己分支开发，开发完成以后合并到某一个指定分支，没有问题后最后合并到master主分支，我们的流程是各自在自己的develop开发，开发完成以后合并到lastest分支，没有问题后提交合并申请到master分支，由leader审批是否统一合并到master，因为很多新人不太清楚代码的具体用途，所以讲的稍微详细点，明白命令的实现目的能更好的掌握使用，后面会有具体的操作流程）
1.本地代码依次
git status
git add
git commit -m ""
git pull
git push （develop-author分支，即自己的开发分支）
以后（把本地代码推送到远程对应分支）
2.git checkout lastest （切换到lastest分支）
3.git pull origin lastest  （先把远程lastest分支修改内容拉取，多人开发，需要把远程lastest上的代码pull下来）
4.git  merge develop-author   （合并自己的分支到lastest）
```

![img](https://upload-images.jianshu.io/upload_images/6175320-bcadc62aabae8a54.png?imageMogr2/auto-orient/strip|imageView2/2/w/832/format/webp)

image.png



![img](https://upload-images.jianshu.io/upload_images/6175320-b62c34774822f7bd.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png



![img](https://upload-images.jianshu.io/upload_images/6175320-47c2f37b600ac359.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png



![img](https://upload-images.jianshu.io/upload_images/6175320-3a00156d71db5f27.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png

5 git stash相关(临时保存当前工作区修改)
其实这个我并不经常用，所以理解可能不足，粗略讲下自己的想法， git stash 常用于 多人开发项目，例如：正式环境代码在master，自己开发在dev，当你正在dev开发或修改时，正式环境有个紧急问题需要解决，但是你dev分支的修改只进行了一半，不方便提交时可以利用git stash 将工作区修改过的内容临时保存起来，切换回master修改完紧急内容会回来可以再取出来临时保存的修改继续操作(还是不太理解的话，再举个不是很恰当的例子，比如你有一些文件在特定环境下必须通过U盘拷到另一个电脑上，只是举个例子，别说说用qq蓝牙邮件什么的传输，但是U盘储存空间不足以保存需要传输的内容大小，也不能分批次传输，这种情况相信大家都能想到先把U盘已有的内容剪切到电脑上，腾出空间储存需要拷贝的资料，等拷贝到目标电脑以后再把U盘原有文件恢复到U盘即可，这个过程就相当于 git stash最基本的用途)。说了这么多，看重点吧



```dart
（1）git stash save "save message"  : 执行存储时，添加备注，方便查找，只有git stash 也要可以的，但查找时不方便识别。

（2）git stash list  ：查看stash了哪些存储

（3）git stash show ：显示做了哪些改动，默认show第一个存储,如果要显示其他存贮，后面加stash@{$num}，比如第二个 git stash show stash@{1}

（4）git stash show -p : 显示第一个存储的改动，如果想显示其他存存储，命令：git stash show  stash@{$num}  -p ，比如第二个：git stash show  stash@{1}  -p

（5）git stash apply :应用某个存储,但不会把存储从存储列表中删除，默认使用第一个存储,即stash@{0}，如果要使用其他个，git stash apply stash@{$num} ， 比如第二个：git stash apply stash@{1} 

（6）git stash pop ：命令恢复之前缓存的工作目录，将缓存堆栈中的对应stash删除，并将对应修改应用到当前的工作目录下,默认为第一个stash,即stash@{0}，如果要应用并删除其他stash，命令：git stash pop stash@{$num} ，比如应用并删除第二个：git stash pop stash@{1}

（7）git stash drop stash@{$num} ：丢弃stash@{$num}存储，从列表中删除这个存储

（8）git stash clear ：删除所有缓存的stash

有时也可通过这种方法实现避免或解决冲突，当你修改的内容是最新的，但是你需要pull下来的代码是需要被替换的，你pull的时候还是会冲突，可以先把你的修改stash临时保存，pull完代码以后在恢复stash的保存，即可替换pull下来的需要被替换的代码，当然不保存直接对比解决冲突也是可以的，看个人喜好了。其他更好的用途相信在遇到更多问题的时候会慢慢发掘出来
```

暂时想到的常用的基本就这些了，关于多人开发冲突解决的有时候自己提交的时候 pull或push失败 可以查看git status 是否多出了一些绿色的文件名称，检查文件没有 ====的冲突提示以及修复冲突以后重新提交即可



```csharp
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

如果提交的过程中发现进入了VIM编辑器 出现#开头的代码 无法继续操作的时候，可以尝试 切换到大写 输入ZZ 即可解决
还有一种情况需要：q 然后输入A 具体问题遇到可以搜索匹配一下





# git远程操作: git push, git fetch, git pull

## git push

##### 1. 推送本地分支到远程分支



```xml
git push <远程主机名> <本地分支名>：<远程分支名>
```

##### 2. 如果省略远程分支名，则表示将本地分支推送至与之存在“追踪关系”的远程分支（通常两者同名），如果该远程分支不存在，则会被新建：



```undefined
git push origin master
```

上述命令表示：将本地master分支推送到origin主机的master分支。如果后者不存在，则会被新建。

##### 3. 如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。



```css
git push origin :master
```

等价于：



```cpp
git push origin --delete master （删除远程分支）
```

##### 4. 如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。



```undefined
git push origin
```

##### 5. 如果当前分支只有一个追踪分支，那么主机名都可以省略：



```undefined
git push
```



## git fetch

##### 1. 将远程主机所有更新全部取回到本地



```xml
git fetch <远程主机名>
```

##### 2. 将远程主机特定分支更新取回到本地



```xml
git fetch <远程主机名> <远程分支名>
```

注：所取回的更新，在本地主机上要用“远程主机名/分支名”的形式读取。如origin主机的master，就要用origin/master读取。

###### (1) 可以在此origin/master的基础上创建新的分支：



```undefined
git checkout -b newBranch origin/master
```

###### (2) 也可以将此origin/master分支合并到本地分支：



```undefined
git merge origin/master
```



## git pull

###### 1. 取回远程主机某个分支的更新，再与本地指定分支合并



```xml
git pull <远程主机名> <远程分支名>：<本地分支名>
```

eg：取回origin主机的next分支与本地master分支合并：



```ruby
git pull origin next：master
```

##### 2. 如果远程分支是与当前分支合并，则冒号后面的部分可以省略：



```ruby
git pull origin next
```

等价于：



```ruby
git fetch origin next

git merge origin/next
```

注：

  在某些场合，Git会自动在本地分支与远程分支之间建立一种追踪关系（tracking）。比如，在git clone的时候，所有本地分支默认与远程主机的同名分支建立追踪关系。也就是说，本地的master分支自动“追踪”origin/master分支。

Git也允许手动建立追踪关系：

eg：指定本地分支master追踪远程origin/next分支：



```bash
git branch --set-upstream-to origin/next master
```

注：查看分支追踪关系：



```undefined
git branch -vv
```

##### 3. 如果当前分支与远程分支存在追踪关系，git pull就可以省略远程分机名



```undefined
git pull origin
```

上面命令表示，本地的当前分支自动与对应的origin主机“追踪分支”（remote-tracking branch）进行合并。

##### 4. 如果当前分支只有一个追踪分支，连远程主机名都可以省略



```undefined
git pull
```

注：

  如果远程主机删除了某个分支，默认情况下，git pull不会在拉取远程分支的时候删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致git pull不知不觉删除了本地分支。

但可以通过加参数 -p 就会在本地删除远程已经删除的分支：



```undefined
git pull -p
```

```
git remote rename origin old-origin
git remote add origin http://gitlab.mlk8s.com/root/myblog.git
git push -u origin --all
git push -u origin --tags
```



```
git config --global user.name "Administrator"
git config --global user.email "admin@example.com"

Create a new repository
git clone http://gitlab.mlk8s.com/root/tools.git
cd tools
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

Push an existing folder
cd existing_folder
git init
git remote add origin http://gitlab.mlk8s.com/root/tools.git
git add .
git commit -m "Initial commit"
git push -u origin master

Push an existing Git repository
cd existing_repo
git remote rename origin old-origin
git remote add origin http://gitlab.mlk8s.com/root/tools.git
git push -u origin --all
git push -u origin --tags
```

```shell
yum install git

git config --global user.name 'ml'
git config --global user.email '75824214@qq.com'

git config --list 查看
git config --list --local 查看 local 配置
comit的时候会带上这个信息

git config --local 只对本地
git config --global 
git config --system 对系统所有登录的用户有效

git  config --local       ##只对某个仓库有效,切换到另外一个仓库失效
git  config --global    ##当前用户的所有仓库有效,工作当中最常用

local的在本地仓库 .git/config
global的在个人home目录下的 .gitconfig 
git add -u：将文件的修改、文件的删除，添加到暂存区。
git add .：将文件的修改，文件的新建，添加到暂存区。
git add -A：将文件的修改，文件的删除，文件的新建，添加到暂存区。

git add -A相对于git add -u命令的优点 ： 可以提交所有被删除、被替换、被修改和新增的文件到数据暂存区，而git add -u 只能操作跟踪过的文件
git add -A 等同于git add -all

git mv files Files 文件重新命名
git reset --hard 清除暂存

git checkout -b test commitid   从哪个commit新建一个分支
git checkout -v 查看当前有本地分支
•	git log --all 查看所有分支的历史
•	git log --all --graph 查看图形化的 log 地址
•	git log --oneline 查看单行的简洁历史。
•	git log --oneline -n4 查看最近的四条简洁历史。
•	git log --oneline --all -n4 --graph 查看所有分支最近 4 条单行的图形化历史。
•	git help --web log 跳转到git log 的帮助文档网页
```

#### .git目录 

```

HEAD 引用指向
[root@k8s-master git_learning]# cat .git/HEAD 
ref: refs/heads/master


// 09|探秘.git目录
//cat命令主要用来查看文件内容，创建文件，文件合并，追加文件内容等功能。
cat HEAD                             查看HEAD文件的内容   
git cat-file 命令                    显示版本库对象的内容、类型及大小信息。
git cat-file -t b44dd71d62a5a8ed3    显示版本库对象的类型
git cat-file -s b44dd71d62a5a8ed3    显示版本库对象的大小
git cat-file -p b44dd71d62a5a8ed3    显示版本库对象的内容

HEAD：指向当前的工作路径
config：存放本地仓库（local）相关的配置信息。
refs/heads:存放分支
refs/tags:存放tag，又叫里程牌 （当这次commit是具有里程碑意义的 比如项目1.0的时候   就可以打tag）
objects：存放对象  .git/objects/ 文件夹中的子文件夹都是以哈希值的前两位字符命名  每个object由40位字符组成，前两位字符用来当文件夹，后38位做文件
```

#### commit tree blob三个对象之间的关系

```groovy
HEAD指向 refs/heads/detached_test

[root@k8s-master git_learning]# cat .git/HEAD 
ref: refs/heads/detached_test

[root@k8s-master git_learning]# cat .git/refs/heads/detached_test
0810ef6b759fd185fc50b8a6692779a3c092c76d

[root@k8s-master git_learning]# git cat-file -t 0810ef6b759
commit

[root@k8s-master git_learning]# git cat-file -p 0810ef6b759
tree 4bd1f073c2d26662b0262eb187032045241735da
parent 9d91c68a53221b7aa8bc99953c8b3bdab1140dbf
author ml-local <ml@qq.com> 1660301659 -0400
committer ml-local <ml@qq.com> 1660301659 -0400

change index.html
[root@k8s-master git_learning]# 

[root@k8s-master git_learning]# git cat-file -p 4bd1f073c2
100644 blob 9a306fad523c9c9cc1e5e2cef6d2f759eee672d3	Readme.md
040000 tree 57399af029434924e74f44d821ee157a0fea69af	images
100644 blob a60899ad7bd95819a8625f143f0b9b3fbdbf02b5	index.html
040000 tree 800a69d8a0cff78cdb26d0742facf7b057cdeece	js
040000 tree b123cc2949289924c69de3012326d616e4e40143	styles

HEAD-->refs文件--->commit-->tree-->blob

tree文件夹
blob文件

```

删除分支：

```
git branch -d 
git branch -D 强删

```

修改commit message

```
git commit --amend
```

修改老旧的commit

```
git rebase -i 要修改的commit的父commit
修改为r
:wq
修改commit
限于本地分支
```

压缩连续commit为一个

```
git rebase -i 前一个commit
pick
s
s
:wq
添加commit说明
:wq
```

压缩间隔的commit为一个

```
git rebase -i 前一个commit
pick
s
s
：wq
添加 
：wq
把要合并的commit 放到一起
```

比较暂存区HEAR所含文件的差异

```
git diff --cached
```

工作区和暂存区的差异

```
git diff  
git diff -- readme.md 只对某个文件
```

暂存区恢复成和HEAD的一样

```
暂存区的变更都不想要了
git reset HEAD   所有的
git reset HEAD -- file  单独
git reset --hard HEAD 工作区和暂存区都恢复成和HEAD一样
```

把工作区恢复成和暂存区一样

```
git diff 比较下
git checkout -- filename

恢复暂存区用： git reset
恢复工作区： git checkout

```

看看两个commit的文件差异

```
git diff temp master -- index.html

```

删除文件

```
git rm filename
```

暂存工作

```
git stash  放到临时目录 
git stash list  查看
git stash apply 从临时目录复制到工作区（）临时保留
git stash pop   从临时目录复制到工作区（）临时不保留
```

git  ignore

```
.gitignore

.doc   doc文件 和doc文件夹都是纳入管控
.doc/   只有doc文件夹不管控
```

备份

```groovy
/path/to/repo.git  哑协议
file:///path/to/repo.git  智能协议
http://git-server.com:port/path/to/repo.git  http
https://git-server.com:port/path/to/repo.git https
user@git-server.com:path/to/repo.git ssh

--bare  不要工作区

git clone --bare /filepath/.git
git clone --bare file:///filepath/.git 


git clone --bare  /opt/myrepo/git_learning/.git  zhineng.git
git clone --bare  file:///opt/myrepo/git_learning/.git  zhineng.git


推送到远端
git remote -v
git remote add zhineng file:///opt/myrepo/clone-test/zhineng.git
git config --global push.default current
git push zhineng 



```

```
github 高级搜索
in:readme
stars:>1000
filename:gitlab

blog easily start in:readme
```

```
github 建博客
```

