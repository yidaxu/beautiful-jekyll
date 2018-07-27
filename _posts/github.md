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
 
 
 #### Branches: 分支
 * 不同分支做不同的事情。
 * labels 就是 branches
  * master 是主branches.
  * check out branches转移到那个branch
  * commit 指更新在当前branch
 * 多用branch, 比如 master, development, evaluation. 可以在多任务中互相转化。
  ``` python
  git branch  # 返回所有的branch 有星号的说明是当前branch
  git branch easy-mode # 后面跟着名字，name就建立这个branch
  git checkout easy-mode # 进入某一个branch
  git add 
  git commit

  ```
 #### Reachability
 * branch记住那些的commit在他的管辖范围之内
 * commit记住它的parent.
 
 #### Detached Head 
 * git checkout commit
 * head means current commit
 * git checkout -b new_branch_name 建立新的branch然后checkout到里面
 
 #### Mreging files
 * git branch
 * git merge 
    * merge
 * git show 和之前的parent的不同之处
 * git branch -d coins 删除 branch
 * git merge merge coins into master
 * delete branch delete label.
 
