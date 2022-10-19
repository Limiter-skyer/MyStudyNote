# 安装
SB教程。步骤根本不对，浪费我时间。
MySQL的软件本体，分msi的安装版和rar的解压缩版，为了彰显程序员的身份，自然要用复杂难用的解压缩版了。
1. 解压缩至合适的目录下，指无中文，无空格，贴近根目录。比如我安装在：`C:\ProgramFiles\mysql-5.7.24-winx64` ，这段目录/字符，在后文称为放置目录。
2. 配置环境变量MySQL软件包下有一个bin文件夹，这个文件夹下有很多可操作的exe文件，把这个路径加到环境变量中。
	1. 此电脑右击--->属性--->高级系统设置--->环境变量
	2. 系统变量--->新建--->变量名：MYSQL_HOME--->变量路径：`C:\ProgramFiles\mysql-5.7.24-winx64` 
	3. 用户变量--->选中Path--->编辑--->新建--->`%MYSQL_HOME%\bin`
	4. 用管理员权限打开cmd，输入`mysql`，返回`不是内部或外部命令`字眼代表失败。
3. 新建配置文件，在 `C:\ProgramFiles\mysql-5.7.24-winx64` 放置目录下新建文本文件，文件名改为 `my.ini` ，文件内容。
4. 初始化，在cmd中输入 `mysqld --initialize-insecure` 。没有返回值，执行后会在该目录下生成一个data文件夹，有这个文件夹就算成功。如果返回失败，可能是管理员权限不够，之前说过要在管理员权限下运行。这个命令只能执行一次，一般不会失败，成功后第二次执行会报错。
5. 安装。输入 `mysqld -install` 安装。再输入一次，如果已安装，会返回，`The service already exists!The current server installed: "************" MySQL` 。注意！！！！！！星号代表的是安装路径，如果这个路径和你的放置目录不一样，那就要走第六步，一样则不需要。
6. 修改注册表。`计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\` 中的ImagePath的值修改为你的放置目录。
7. cmd输入 `net start mysql` 启动服务
8. cmd输入 `mysqladmin -u root password 1234` 来创建管理员账户，root是用户名, 1234是密码。
9. 登录MySQL：`mysql -uroot -p1234`
10. 退出MySQL：`exit`

# MySQL数据模型
Excel表式结构
数据库是由多张能互相链接的二维表组成的，储存在硬盘而非内存里。
MySQL是由文件夹实现的。
数据库（文件夹）--->表（.frm文件）--->数据（.MYD文件）