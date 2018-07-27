remote repositories:
the repositories you want to receive or send  new commites from and to.
push 
pull 

```
git remote 看有的remote
git remote add name_of_remote(origin) url
git remote
git remote -v (verbose)
git push orinia (name to push to ) master(branch you want to do ) in remote also called master
git pull origin master(nbranch you want to do)
git clone 自动加一个remote origin of the link you clone.
fork 在github上直接搞事情。
可以加collabrater 来一起搞事情。
```

```python
git diff what changes you have made.
```


#### origin/master
* 在本地的github中我们有一个叫做origin/master的branch来记录github上的remote的github
* 当我们push最近的commit的时候，这个origin/master也更新
* git fetch 我们把github上的更新加到origin/master上，并不改变本地的master
* git merge 然后可以搞两个
* git pull == git fetch + git merge
```python
git fetch orign
git merge master origin/master
... conflict ==> resolve.
git add
git commit
git pull origin master
```

### pull requests

```
git branck vege
git branch different-oil
git checkout different-oil
git add cake-recipe.txt
git commit
git push origin different-oil
```
* pull request in github: merger code to master. == merge request, ask you to pull her code into merge master 
* zai github 上add commit.


Make a pull request
* Create a different-oil branch
* Make a commit different-oil changing the oil
* Push different oil to your fork
* Create a pull request from different-oil into master


Conflict for pull request
* pull your computer git pull
* git checkout different-oil
* git merge master different-oil
* reslove the conflict
* git push different-oil
