# Git合并指定文件到另一个分支

## 合并某个分支上的单个commit

首先，用 `git log` 或 `sourcetree` 工具查看一下你想选择哪些 `commits` 进行合并，例如：

比如 `feature` 分支上的 `commit 82ecb31` 非常重要，它含有一个bug的修改，或其他人想访问的内容。无论什么原因，你现在只需要将 `82ecb31` 合并到`master`，而不合并 `feature`上的其他`commits`，所以我们用 **`git cherry-pick`** 命令来做：

````js
git checkout master  

git cherry-pick 82ecb31

/* 取消 */
git cherry-pick --abort
````

这样就好啦。现在82ecb31就被合并到master分支，并在master中添加了commit（作为一个新的commit）。cherry-pick 和merge比较类似，如果git不能合并代码改动（比如遇到合并冲突），git需要你自己来解决冲突并手动添加commit。

这里git cherry-pick每次合并过来会显示文件冲突(其实并没有冲突代码部分，只需手动解决既可)。

## 合并某个分支上的一系列commits

在一些特性情况下，合并单个 `commit` 并不够，你需要合并**一系列相连的commits**。这种情况下就不要选择`cherry-pick`了，**`rebase`** 更适合。

还以上例为例，假设你需要合并feature分支的commit76cada ~62ecb3 到master分支。

* 首先需要基于feature创建一个新的分支，并指明新分支的最后一个commit：

````js
git checkout feature 

git checkout -b newbranch 62ecb3
````
* 然后，rebase这个新分支的commit到master（--ontomaster）。76cada^ 指明你想从哪个特定的commit开始。

````js
git rebase --onto master 76cada^ 
````

* 得到的结果就是feature分支的commit 76cada ~62ecb3 都被合并到了master分支。

另外如果只想将feature分支的某个文件f.txt合并到master分支上。

````js
git checkout feature

git checkout --patch master f.txt
````

第一个命令： 切换到feature分支；
第二个命令：合并master分支上f文件到feature分支上，将master分支上 f 文件追加补丁到feature分支上 f文件。你可以接受或者拒绝补丁内容。

如果只是简单的将feature分支的文件f.txt copy到master分支上；

````js
git checkout master

git checkout feature f.txt
````