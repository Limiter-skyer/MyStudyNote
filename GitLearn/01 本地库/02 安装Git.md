Git可以在: Linux, windows, macOS, 中使用，当然，主要还是在Linux。
由于我本人暂时还没有用过macOS和Linux，所以暂时只记录windows的安装过程。

# windows下安装Git
windows的安装非常简单，直接去[官网](https://git-scm.com/downloads) 下载，一步步默认安装就行了。
安装完成后，在开始菜单中找到`Git Bash`, 打开。
蹦出一个类似命令行窗口的东西，就说明安装成功了。

# 初始化
打开Git Bash，输入：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

初始化你的用户名和邮箱