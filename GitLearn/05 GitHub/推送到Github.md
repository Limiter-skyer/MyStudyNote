把当前分支推送到github
```
git push
```
很简单的一个命令

现在我新建了一个分支，完成了一系列的开发，要把这个分支推送到github。
github里是没有这个分支的，所以不能直接git push
```
git push -u origin login
```
这个命令会在github创建一个分支，然后推送。
