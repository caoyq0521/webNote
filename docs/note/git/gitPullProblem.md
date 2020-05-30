# 解决git拉取问题

````js
/* 解决办法一:保留本地的更改,中止合并->重新合并->重新拉取 */
git merge --abort
git reset --merge
git pull

/* 解决办法二:舍弃本地代码,远端版本覆盖本地版本(慎重) */
git fetch --all
git reset --hard origin/master
git fetch
````