# 保存现场
现在有master分支，dev分支，你在dev分支上开发。
现在有一个master上有bug需要紧急修复，但是你在dev上的开发工作还没有完成，没有到可以提交的地步。
此时可以用`git stash`来暂时保存现场。
```
$ git stash
Saved working directory and index state WIP on dev: 8fdeb9b add readme message
```

此时用`git status`查看，工作区就干净了。我们可以用`git stash list`查看保存了哪些现场。恢复现场之后再说

# 修改Bug
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它。
bug在main分支上，我们先切换到main分支:`$ git switch main`，然后新建一个bug分支
```
$ git switch -c issue-101
```
修Bug，修完add，commit。
然后切换回main分支，合并分支
```
$ git merge --no-ff -m "merge bug fix 101" issue-101
Merge made by the 'ort' strategy.
 LemonTree.txt | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
```
删掉issue分支
OK，Bug修复完成，现在该回到dev干活了。

# 加载保存的现场
之前我们为了修Bug，而把干到一半，没有提交的工作，用`git stash`保存了，现在来读取一下。
先用`git stash list`查看保存的现场
```
$ git stash list
stash@{0}: WIP on dev: 8fdeb9b add readme message
```
再恢复现场，有两个办法：
一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；
另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：
```
$ git stash pop
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (729866cd81537afa1c0d22590b6a51de77d2a543)

```

再用`git stash list`查看，就看不到任何stash内容了：
你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：
```
$ git stash apply stash@{0}
```