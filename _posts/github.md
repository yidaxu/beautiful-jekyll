Repositories
 * 在git init之后建立。
 * 只track本文件及其子文件夹。
 * 和普通文件及相同，除了有.git隐藏文件夹。
 
Git log 
 * 有多少commit
 
Git status
  * which branch you are in
  * untracked files.
  
Staging area
* 是working directory和repository的中间变量
* 利用git add 加文件进入staging area
* git status 中 changes to be committed 就是 staging area.

Commit change

* git commit -m "add something."

Git diff

* git diff: working directory  和 stage 有什么不同
* git diff --staged: stage 和 most commit 有什么不同
* git reset --hard 舍弃说有的的stage和working directory的修改。
 
