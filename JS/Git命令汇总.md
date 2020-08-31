### 使用
##### git fetch 和 git pull的区别
* ```git pull```：相当于是从远程获取最新版本并merge到本地
* ```git fetch```：相当于是从远程获取最新版本到本地，不会自动merge

##### 添加upstream
```git remote add upstream http://github/remote/test.git```

##### 删除某个分支
```git branch -D 分支名```

##### 示例
```
git fetch upstream
git merge upstream/feature-typescript
git push origin feature-typescript
```
##### 从远端切换红包分支(不带master代码)：
1. 暂存本地更改：```git add .```
2. 提交本地更改：```git commit```
3. 拉取远端代码：```git fetch upstream```
4. 合并代码：```git merge upstream/release-red-packet```
5. 提交代码：```git push origin release-red-packet```

##### 从远端切换拼团活动分支(不带master代码)：
```git checkout -b feature upstream/feature```
1. 暂存本地更改：```git add .```
2. 提交本地更改：```git commit```
3. 拉取远端代码：```git fetch upstream```
4. 合并代码：```git merge upstream/release-red-packet```
5. 提交代码：```git push origin release-red-packet```

##### 提交代码
1. ```git add . ```&ensp;&ensp;添加所有文件
2. ```git commit -m "本功能全部完成"```

##### 撤回commit
```git reset --soft HEAD^```

仅仅是撤回commit操作，写的代码仍然保留。

HEAD^的意思是上一个版本，也可以写成HEAD~1

如果你进行了2次commit，想都撤回，可以使用HEAD~2

至于这几个参数：
> --mixed

意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作

这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

> --soft  

不删除工作空间改动代码，撤销commit，不撤销git add .

> --hard

删除工作空间改动代码，撤销commit，撤销git add .

注意完成这个操作后，就恢复到了上一次的commit状态。


##### commit注释写错了，改注释：
```git commit --amend```

此时会进入默认vim编辑器，修改注释完毕后保存就好了。

### 设置并查看config配置
```git config --global user.name "中文姓名" ```
```git config --global user.email "email@email.com"```

```cat ~/.gitconfig```

### 状态：Change, Staged, Committed
* Change(Unstaged)：你改动了一个，没有调用任何git命令前，就是这种状态。
* Staged：调用git add或者git commit -a之后，进入Staged状态，表示申明要变动了。
* Committed：Commit，生成新的版本commit号，进入此状态。

### git 命令

#### git init：
初始化一个目录，其实就是加了一个.git的隐藏目录

#### git clone：
远程复制一个完整的repository到本地，比如git clone git://github.com/schacon/simplegit.git，就是从git://github.com/schacon/simplegit.git这个地址clone到本地当前目录。

#### git add：
把一个文件从change->staged状态。

git add test.txt。注意，不仅仅是添加新文件，修改现有文件也要git add来修改状态，否则git不会考虑将之commit。（当然可以git commit -a省力点）

#### git status：
刚添加完，还没commit，这时候就能用git status -s看看当前修改和仓库里面差别多少，可以看到有多少文件被新增了，多少被修改了，多少被删除了。加个-s用简洁模式查看。一般在git commit之前看一把。

#### git diff：
不加参数比较当前修改的文件和上次commit在仓库里面的区别。

git diff develop，查看当前版本和develop分支的差异。如果想比较某个目录下的文件，可以用git diff -- ./libs，这个只比较libs目录下的文件。

#### git commit：
git commit -m 'message here'。提交到仓库，必须要一个message。

如果嫌每次都是先git add，再git commit，很麻烦的话，直接git commit -am 'message'，带上-a后全部一把进去。

#### git log：
查看提交记录。只想看某个分支的话，git log develop。带上--oneline用简洁模式查看。

#### git reset：
撤销某次提交。最普通用法，git reset HEAD -- file，将某个文件从staged状态->unstaged状态，这文件也不能被commit了。

git reset --hard HEAD~1，回退到当前HEAD之前的一个版本。

#### git branch：
不带任何参数，就是看当前目录有多少分支，默认init后一般会有一个master。git branch develop，创建一个develop分支。

git branch -d develop，删除develop分支。-D参数则表示不管有没有merge过，都强制删除。

#### git checkout：
快速切换分支

比如git checkout develop，马上切换到develop分支。这个地方我觉得git很牛逼，不用换目录，立马换一套context。

#### git tag：
git tag -a v1.0，将最后一次commit（HEAD）标记为永久的v1.0版本。如果要给以前某次commit打tag，也可以加上提交的版本号就行（版本号可以通过git log --online看到）

#### git remote：
列出所有的远程仓库。从别处clone来的，默认都会有一个别名"origin"的仓库。带上-v可以看到具体URL。git remote add/rw，添加/删除远程仓库地址。其实这些操作都是在本地，并没有实际牵涉到远程。另外github里面folk过来的，默认叫"upstream"。

#### git fetch：
从远程下载分支。

git fetch upstream A:B，将远程仓库upstream下的A分支下载到本地，本地叫B分支。如果不带A:B参数，则下载以后，可能会叫upstream/A（如果远程是A分支的话），远程分支要通过git branch -r查看。

一般的做法是先git fetch upstream master:tmp（将远程的master先下载到本地的tmp分支，然后git diff tmp看看本地master和tmp的区别，没问题的话再git merge tmp。这样比直接git pull upstream来的安全。

#### git pull：
同fetch，只是下载以后，直接进行merge。

比如git pull upstream master，就直接将upstream下载下来，与本地的master合并。

#### git push：
git push origin A:B，将本地的A分支push到远程仓库origin下，并叫做B。如果省略:B，那么一般本地和远程的分支同名。特殊情况：删除远程分支可用通过push一个本地空分支来做到。

git push origin :B，push一个空分支到origin下的B，即删除了远程分支B
