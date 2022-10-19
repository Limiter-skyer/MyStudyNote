# 把文件添加到版本库
对一个已经存在的文件，比如readme.md，添加到git的版本库中。
第一步：用`git add`命令添加到缓存区。

```
$ git add readme.md
```

没有任何反馈，unix的哲学是：”没有消息就是好消息“
此时已经添加完成

第二步：用`git commit`命令，告诉git把缓存区的文件添加到仓库

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

再对文件进行修改后，依旧是用这两步进行提交。

`-m`命令后的字符串，是此次修改的说明。比如甲方要我改个颜色，我改完提交，可以在-m中写：”把红色改成蓝色“，这样在日后回滚的时候，就知道此次修改具体改了什么。

