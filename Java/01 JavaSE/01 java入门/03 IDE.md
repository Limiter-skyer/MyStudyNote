# 集成开发工具
IDE: Integrated Development Environment 集成开发环境
把代码、编译、执行等多种功能综合到一起的开发工具
常见的开发工具有：Eclipse、MyEclipse，IntelliJ IDEA等

# IntelliJ IDEA
一般简称IDEA，业界公认最好的java开发工具。
[官网](https://www.jetbrains.com/idea/)

## IDEA项目结构
- Project
	- Module1
		- package1
			- class
			- class
		- package2
	- Module2

## IDEA项目创建顺序
1. 创建工程Project
2. 创建模块Module，文件新建，name.app
3. 创建包Package，右击src新建，com.域名.name
4. 创建类class，右击Package新建
5. 在类中编写代码，main
6. 完成编译运行，右击run Class main()

## IEDA快捷键
- main/psvm、sout：快速键入相关代码
- Ctrl+D：复制当前行
- Ctrl+Y/X：删除/剪切所在行
- Ctrl+Alt+L：格式化代码
- Alt+Shift+↑/↓：上下移动代码
- Ctrl+/，Ctrl+Shift+/：注释
- fori：快速输入for循环
- 选中代码，Ctrl+Shift+T：把选中代码嵌入分支或者循环
- 数组.fori：快速把数组嵌入for循环

## 其他操作
删除：右击>Delets：直接删除文件夹
右击>Remove Module：只从界面处删除，实际文件夹不删除

**重命名**：右击>Refactor>Rename
![[Pasted image 20220705094858.png]]

**重命名模块**：右击>Refactor>Rename>Rename module and directory
![[Pasted image 20220705094821.png]]


**导入模块**:File>New>Module from Existing Source…
![[Pasted image 20220705094746.png]]
**导入模块的时候选中iml文件点击导入**
这种方式叫关联导入
建议新建模块，直接把别的代码直接拷贝到工程文件夹里