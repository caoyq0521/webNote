# Git常用命令

## Git初始化仓库

````js
echo '# project name' >> README.md

git init

git add README.md

git commit -m "first commit"

git remote add origin https://github.com/username/project-name.git

git push -u origin master
````

## 常用命令

````js
git branch  // 查看本地分支

git branch <name>  // 新建分支

git branch -a  // 查看所有分支,包括远程分支

git checkout master  // 切换分支

git log  // 查看提交日志

git log file.txt  // 查看文件的历史版本信息

git branch -D branch_name  // 删除本地分支

git push origin --delete branch_name  // Git 1.7以后删除远程分支

git push -f origin master  // 强制用本地的代码去覆盖掉远程仓库的代码

git diff 分支1 分支2 (可以是远程分支)  // 对比两个分支

git merge branchname  // 把branchname分支合并到当前分支里面 合并另外一个分支到当前分支

git branch branch_name && git push origin branch_name  // 新建分支并推送到远程

git fetch  // 从远程分支获取最新版本到本地分支，不会自动merge

git reset --soft commit_id // 回到之前某个版本

git reset --hard HEAD^  // 回退到上个版本

git reset --hard HEAD~3  // 回退到前3次提交之前，以此类推，回退到n次提交之前

git push origin HEAD --force  // 强推到远程

git pull  // 拉取远程的最新代码合并到本地当前分支

git rm --cached  // 清空git缓存的文件，主要针对有些时候无法add文件，重新忽略某些文件等场景

git config --system --list  // 查看系统config

git config --global  --list // 查看当前用户（global）配置

git config --local  --list // 查看当前仓库配置信息

git config --system --unset credential.helper // 重置用户名和密码
````

* Git更新自己Fork的代码项目和原作者的项目进度一致

````js
git remote add sri https://github.com/kraih/mojo

git fetch sri

git merge sri/master
````

* git 1.7 clone私有项目需要使用ssh，1.8之后可以直接输入用户名密码

````js
/* clone 私有项目 */
git clone https://username@github.com/compangynaem/projectname
````

附：[Git完整版教程](https://blog.csdn.net/tangbin330/article/details/9128765)