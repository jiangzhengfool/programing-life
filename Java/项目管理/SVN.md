# Subversion
### 类似的技术有哪些?对比之后的优缺点?
- 类似的有CSV
- 优缺点:
- 目录版本控制
- 原子提交
- 版本控制的元数据:文件和目录的附件"属性"
- 可选的网络层:WebDAV/DeltaV通讯
- 一致的数据处理
- 高效的分支和标签:硬链接
### 解决什么样的问题
- 项目版本控制
### 理解其原理?并掌握其结构图或实现结构?
	实现了一个"虚拟"文件系统
	原理:
		C/S模式  C语言开发
	http://upload-images.jianshu.io/upload_images/2069996-087f4faa8d9fc3f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240
### 如何使用?知识点结构图?重难点?
	基本功能:checkout update commit
	配置并启动服务端
	权限配置:单一版本库、多版本库
	info log
	解决冲突
	在Eclipse中使用
### 根据需要对此技术的掌握程度,深入理解此技术,如:阅读原码,重构,二次开发等
	配置并启动，权限配置
### 参考内容
- [[尚硅谷]_封捷_SVN课程讲义](D:\NutCloud\E_JAVA\项目管理\[尚硅谷]_封捷_SVN课程讲义.docx)
- [SVN命令使用详解](D:\NutCloud\E_JAVA\项目管理\SVN命令使用详解.txt)

# Tortoise SVN
### Subversion 的客户端程序
### 参考内容
- [[尚硅谷]_封捷_SVN课程讲义](D:\NutCloud\E_JAVA\项目管理\[尚硅谷]_封捷_SVN课程讲义.docx)