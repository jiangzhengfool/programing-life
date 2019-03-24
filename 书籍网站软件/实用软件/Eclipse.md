## 编码
文件编码 修改成 UTF-8


## 字体大小
- Consolas 11

## 格式化
- 设置常用文件格式化默认长度修改为120,tab键


## 代码自动提示
-  Auto Activation 把"."修改为"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"， 这样就会 输入每个字母都提示。同样的方法，可以修改Javascript和HTML页面的代码提示：

## 设置JDK 本地JavaDOC API的路径 以及 源码
   
全选 JRE system libraries 下的所有jar 包， 点击右侧的 Source Attachment，选择External Location, 选择JDK安装目录下的src.zip文件，点击OK即可！

## 加速启动速度
- 在eclipse启动的时候，它总是会搜索让其运行的jre，往往就是这个搜索过程让eclipse启动变慢了。（没设置时，等2-3s出现进度条，设置后直接出现进度条）只要在eclipse.ini中加入-vm C:\Program Files\Java\jdk1.8.0_31\jre\bin的参数就可以了
- 取消所有启动时要激活的插件（在用时激活也一样）和其它的相关的在启动时执行的操作(start and shutdown)
- 减少jvm内存回收引起的eclipse卡的问题
- 关闭自动构建。在启用时，每保存一下，eclipse就会自动为我们构建整个项目，这样对于大的项目来说，每次保存时都会造成很卡。其实自动构建完全没有必要，只要保证在运行前构建一次就ok了，eclipse也会在运行前自动为我们构建，所以关闭是最明智的选择。
- 关闭拼写检查设置
- 关闭SaveAction
- [优化代码提示](http://dl.iteye.com/upload/attachment/0080/7765/a6a2ba1c-b9b8-38f0-beb9-141c02023751.png)

## maven环境

## tomcat环境

## 快捷键
Eclipse快捷键大全(转载)
* Alt+← 前一个编辑的页面
* Alt+→ 下一个编辑的页面(当然是针对上面那条来说了)
* Alt+Enter 显示当前选择资源(工程,or 文件 or文件)的属性
* Ctrl+Q 定位到最后编辑的地方
* Ctrl+L 定位在某行 (对于程序超过100的人就有福音了)
* Ctrl+M 最大化当前的Edit或View (再按则反之)
* Ctrl+W 关闭当前Editer
* Ctrl+K 参照选中的Word快速定位到下一个
* Ctrl+E 快速显示当前Editer的下拉列表(如果当前页面没有显示的用黑体表示)
* Ctrl+/(小键盘) 折叠当前类中的所有代码
* Ctrl+×(小键盘) 展开当前类中的所有代码
* Ctrl+Space 代码助手完成一些代码的插入(但一般和输入法有冲突,可以修改输入法的热键,也可以暂用Alt+/来代替)
* Ctrl+Shift+E 显示管理当前打开的所有的View的管理器(可以选择关闭,激活等操作)
* Ctrl+J 正向增量查找(按下Ctrl+J后,你所输入的每个字母编辑器都提供快速匹配定位到某个单词,如果没有,则在stutes line中显示没有找到了,查一个单词时,特别实用,这个功能Idea两年前就有了)
* Ctrl+Shift+J 反向增量查找(和上条相同,只不过是从后往前查)
* Ctrl+Shift+F4 关闭所有打开的Editer
* Ctrl+Shift+P 定位到对于的匹配符(譬如{}) (从前面定位后面时,光标要在匹配符里面,后面到前面,则反之)

下面的快捷键是重构里面常用的,本人就自己喜欢且常用的整理一下(注:一般重构的快捷键都是Alt+Shift开头的了)
* Alt+Shift+C 修改函数结构(比较实用,有N个函数调用了这个方法,修改一次搞定)
* Alt+Shift+F 把Class中的local变量变为field变量 (比较实用的功能)
* Alt+Shift+I 合并变量(可能这样说有点不妥Inline)
* Alt+Shift+V 移动函数和变量(不怎么常用)
* Alt+Shift+Z 重构的后悔药(Undo)

编辑
* 作用域 功能 快捷键
* 文本编辑器 查找上一个 Ctrl+Shift+K
* 文本编辑器 查找下一个 Ctrl+K
* 全局 恢复上一个选择 Alt+Shift+↓
* 全局 快速修正 Ctrl1+1
* 全局 内容辅助 Alt+/
* 全局 全部选中 Ctrl+A
* 全局 删除 Delete
* 全局 上下文信息 Alt+？
* Alt+Shift+?
* Ctrl+Shift+Space
* Java编辑器 显示工具提示描述 F2
* Java编辑器 选择封装元素 Alt+Shift+↑
* Java编辑器 选择上一个元素 Alt+Shift+←
* Java编辑器 选择下一个元素 Alt+Shift+→
* 文本编辑器 增量查找 Ctrl+J
* 文本编辑器 增量逆向查找 Ctrl+Shift+J
* 全局 重做 Ctrl+Y


查看
* 作用域 功能 快捷键
* 全局 放大 Ctrl+=
* 全局 缩小 Ctrl+-


* 窗口
* 作用域 功能 快捷键
* 全局 激活编辑器 F12
* 全局 切换编辑器 Ctrl+Shift+W
* 全局 上一个编辑器 Ctrl+Shift+F6
* 全局 上一个视图 Ctrl+Shift+F7
* 全局 上一个透视图 Ctrl+Shift+F8
* 全局 下一个编辑器 Ctrl+F6
* 全局 下一个视图 Ctrl+F7
* 全局 下一个透视图 Ctrl+F8
* 文本编辑器 显示标尺上下文菜单 Ctrl+W
* 全局 显示视图菜单 Ctrl+F10
* 全局 显示系统菜单 Alt+-


导航
* 作用域 功能 快捷键
* Java编辑器 打开结构 Ctrl+F3
* 全局 打开类型层次结构 F4
* 全局 打开声明 F3
* 全局 打开外部javadoc Shift+F2
* 全局 打开资源 Ctrl+Shift+R
* 全局 后退历史记录 Alt+←
* 全局 前进历史记录 Alt+→
* 全局 上一个 Ctrl+,
* 全局 下一个 Ctrl+.
* Java编辑器 显示大纲 Ctrl+O
* 全局 在层次结构中打开类型 Ctrl+Shift+H
* 全局 转至匹配的括号 Ctrl+Shift+P
* 全局 转至上一个编辑位置 Ctrl+Q
* Java编辑器 转至上一个成员 Ctrl+Shift+↑
* Java编辑器 转至下一个成员 Ctrl+Shift+↓
* 文本编辑器 转至行 Ctrl+L

搜索
* 作用域 功能 快捷键
* 全局 出现在文件中 Ctrl+Shift+U
* 全局 打开搜索对话框 Ctrl+H
* 全局 工作区中的声明 Ctrl+G
* 全局 工作区中的引用 Ctrl+Shift+G


文本编辑
* 作用域 功能 快捷键
* 文本编辑器 改写切换 Insert
* 文本编辑器 上滚行 Ctrl+↑
* 文本编辑器 下滚行 Ctrl+↓


* 文件
* 作用域 功能 快捷键
* 全局 打印 Ctrl+P
* 全局 关闭 Ctrl+F4
* 全局 全部保存 Ctrl+Shift+S
* 全局 全部关闭 Ctrl+Shift+F4
* 全局 属性 Alt+Enter
* 全局 新建 Ctrl+N


* 项目
* 作用域 功能 快捷键
* 全局 全部构建 Ctrl+B


* 源代码
* 作用域 功能 快捷键
* Java编辑器 取消注释 Ctrl+\
* Java编辑器 注释 Ctrl+/
* Java编辑器 添加导入 Ctrl+Shift+M
* Java编辑器 组织导入 Ctrl+Shift+O
* Java编辑器 使用try/catch块来包围 未设置，太常用了，所以在这里列出,建议自己设置。
* 也可以使用Ctrl+1自动修正。


* 运行
* 作用域 功能 快捷键
* 全局 单步返回 F7
* 全局 单步跳过 F6
* 全局 单步跳入 F5
* 全局 单步跳入选择 Ctrl+F5
* 全局 调试上次启动 F11
* 全局 继续 F8
* 全局 使用过滤器单步执行 Shift+F5
* 全局 添加/去除断点 Ctrl+Shift+B
* 全局 显示 Ctrl+D
* 全局 运行上次启动 Ctrl+F11
* 全局 运行至行 Ctrl+R
* 全局 执行 Ctrl+U


* 重构
* 作用域 功能 快捷键
* 全局 撤销重构 Alt+Shift+Z
* 全局 抽取方法 Alt+Shift+M
* 全局 抽取局部变量 Alt+Shift+L
* 全局 内联 Alt+Shift+I
* 全局 移动 Alt+Shift+V
* 全局 重命名 Alt+Shift+R
* 全局 重做 Alt+Shift+Y
