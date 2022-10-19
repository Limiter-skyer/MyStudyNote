# 难看的分支树
```
$ git log --graph --pretty=oneline --abbrev-commit
*   ce4c237 (HEAD -> main) merge bug fix 101
|\
| * a1d65d5 debug: change word turn to turning
|/
*   71c2068 fix conflicts
|\
| * feb5e0a branch test
| * 2e4329c branch test
* | c190369 branch test
|/
* 91937af test
* 9bf8eac change words turning
*   347200b Merge branch 'front' into after
|\
| * 58304cf add some lyrics front original lyrics
* | 51ec59c add some lyrics after original lyrics
|/
* 6fdf713 branch test
* c4ac0a2 (origin/main) add a readme file
* 92fc1f9 add two music
* a20c013 remove optical.txt
* b09975f add dio's word
* 2831a51 create a new file named optical.txt
```
远程仓库中的版本在(origin/main)，本地经过诸多开发后，版本在最新的(HEAD -> main)

开发中肯定会出现很多分支，虽然这些分支在开发时是必要的，但是我已经开发完了，还留着这么一大堆分支干什么，是时候卸磨杀驴了。
其实驴早就杀了，我已经把这些分支都合并删除了，现在log里的只是一堆尸体。
但问题是，我现在要把开发完的代码推到GitHub上去，这些尸体就不用一起推过去了吧。

# rebase
```
$ git rebase
dropping 2e4329cfb86540534116f433d9a57a97d4c2c8a7 branch test -- patch contents already upstream
Successfully rebased and updated refs/heads/main.
```
一个命令，分支树就被干掉了
```
* 107bd07 (HEAD -> main) debug: change word turn to turning
* fc2e21f branch test
* c00682d branch test
* f19a439 test
* af554ab change words turning
* 118c0f0 add some lyrics front original lyrics
* 51ec59c add some lyrics after original lyrics
* 6fdf713 branch test
* c4ac0a2 (origin/main) add a readme file
* 92fc1f9 add two music
* a20c013 remove optical.txt
* b09975f add dio's word
* 2831a51 create a new file named optical.txt
* b758256 add a title
* dc5bda2 add some lyric
* 529c521 creat a music lyric

```