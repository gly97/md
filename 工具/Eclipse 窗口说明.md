# Eclipse 窗口说明

## Eclipse 工作台(Workbench)

首先，让我们来看一下 Eclipse 工作台用户界面，和它里面的各种组件。

工作台是多个窗口的集合。每个窗口包含菜单栏，工具栏，快捷方式栏，以及一个或者多个透视图。

![Eclipse 窗口说明](https://www.runoob.com/wp-content/uploads/2014/12/workbench_decomposed.gif)

视图完全存在于某个透视图中而且不能被共享，而任何打开的内容编辑器可以在透视图间共享。

如果两个或者多个透视图打开了同样的视图，他们共享这个视图的同一个实例，虽然在不同透视图之间视图的布局可能不同。

对于不同的工作台窗口中的透视图，编辑器和视图都不能共享。

一个透视图就好像是一本书里面的一页。它存在在一个窗口中，并且和其他透视图一起存在，和书中的一页一样，每次你只能看到一个透视图。

工作台的主菜单栏通常包括File，Edit，Navigate，Project，Window，Help这些顶层菜单。

其他的顶层菜单位于Edit和Project菜单之间，往往是和上下文相关，这个上下文包括当前活动的透视图，最前面的编辑器以及活动视图。

在File菜单中，你可以找到一个New子菜单，它包括Project，Folder，File的创建菜单项。

File 菜单也包含Import and Export菜单项，用来导入文件到Workbench中，以及导出它们。

在Edit菜单中，你可以找到象Cut，Copy，Paste，和Delete这些命令。这些命令称为全局命令，作用于活动部件。

也就是说，如果当Navigator活动时使用Delete命令, 实际操作是由Navigator完成的。

在Project菜单中，你可以找到和项目相关的命令，比如Open Project，Close Project和Rebuild Porject等。

在Run菜单中，你可以看到和运行，调试应用代码相关的命令，以及启动象Ant脚本这样的外部工具。

在Window菜单中，你可以找到Open Perspective子菜单，根据你开发任务的需要打开不同的透视图。

你也能看到透视图 布局管理菜单栏。Show View子菜单用来在当前的Workbench窗口中增加视图。

另外，你可以通过首选项菜单项来修改工作台的功能首选项配置。

如果你是插件开发者，你可以为平台提供新的视图，编辑器，向导，菜单和工具项。 这些东西都是用XML来定义的，插件一旦注册后，就可以和平台中已经存在的组件无缝地集成在一起。

------

## Eclipse 多窗口

Eclipse 可以同时开启多个窗口，在 菜单栏选择： Window -> New Window 来开启多窗口。

多个窗口的切换你可以使用 Alt + Tab 来回切。











# Eclipse 菜单

Eclipse 查看的菜单栏通常包含以下几个菜单：

- File 菜单
- Edit 菜单
- Navigate 菜单
- Search 菜单
- Project 菜单
- Run 菜单
- Window 菜单
- Help 菜单

![Eclipse 菜单](https://www.runoob.com/wp-content/uploads/2014/12/explore_menu.jpg)

------

## 菜单描述

| 菜单名   | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| File     | File 菜单运行你打开文件，关闭编辑器，保存编辑的内容，重命名文件。 此外还可以导入和导出工作区的内容及关闭 Eclipse。 |
| Edit     | Edit 菜单有复制和粘贴等功能。                                |
| Source   | 只有在打开 java 编辑器时 Source 菜单才可见。 Source 菜单关联了一些关于编辑 java 源码的操作。 |
| Navigate | Navigate 菜单包含了一些快速定位到资源的操作。                |
| Search   | Search 菜单可以设置在指定工作区对指定字符的搜索。            |
| Project  | Project 菜单关联了一些创建项目的操作。                       |
| Run      | Run 菜单包含了一些代码执行模式与调试模式的操作。             |
| Window   | Window 菜单允许你同时打开多个窗口及关闭视图。 Eclipse 的参数设置也在该菜单下。 |
| Help     | Help 菜单用于显示帮助窗口，包含了 Eclipse 描述信息，你也可以在该菜单下安装插件。 |

Eclipse 也可以自定义菜单，自定义菜单的详细介绍可以查看 [Eclipse 透视图](https://www.runoob.com/eclipse/eclipse-perspectives.html)。









# Eclipse 视图

## 关于视图

Eclipse视图允许用户以图表形式更直观的查看项目的元数据。 例如，项目导航视图中显示的文件夹和文件图形表示在另外一个编辑窗口中相关的项目和属性视图。

Eclipse 透视图(perspective) 可以显示任何的视图和编辑窗口。

所有的编辑器实例出现在一个编辑器区域内，可以通过文件夹视图查看。

一个工作台窗口可以显示任意数量的文件夹视图。每个文件夹视图可以显示一个或多个视图。

## 组织视图

下图显示了文件夹视图的四个视图。

![explore_view_1](https://www.runoob.com/wp-content/uploads/2014/12/explore_view_1.jpg)![explore_view_2](https://www.runoob.com/wp-content/uploads/2014/12/explore_view_2.jpg)

## 移除视图 views

视图从一个文件夹视图移动到另外一个文件夹视图只需要点击视图标题并推动视图工具区域到另外一个文件夹视图。

![explore_views_moving](https://www.runoob.com/wp-content/uploads/2014/12/explore_views_moving.jpg)

文件夹视图可以通过移动视图标题栏到编辑去外或移动标题栏到另外一个文件夹视图来动态创建。 下图中如果你拖动了绿色线框内的标题栏意味着一个新的文件夹视图将被创建。

![explore_views_creating_vf](https://www.runoob.com/wp-content/uploads/2014/12/explore_views_creating_vf.jpg)

## 操作视图

你可以在 Window 菜单中点击 "Show View" 选项打开其他视图。

![explore_views_new_view](https://www.runoob.com/wp-content/uploads/2014/12/explore_views_new_view.jpg)![explore_views_show_view](https://www.runoob.com/wp-content/uploads/2014/12/explore_views_show_view.jpg)

视图通过各个分类来组织。你可以通过搜索框快速查找视图。 然后打开视图并选择，点击 "OK" 按钮即可。

## 什么是透视图？

透视图是一个包含一系列视图和内容编辑器的可视容器。默认的透视图叫 java。

Eclipse 窗口可以打开多个透视图，但在同一时间只能有一个透视图处于激活状态。

用户可以在两个透视图之间切换。

## 操作透视图

通过"Window"菜单并选择"Open Perspective > Other"来打开透视图对话框。

![explore_perspective_open](https://www.runoob.com/wp-content/uploads/2014/12/explore_perspective_open.jpg)



该透视图列表也可以通过工具栏上的透视图按钮来打开 (![img](http://www.tutorialspoint.com/eclipse/images/explore_perspective_btn.jpg) ) 。

## 视图切换

大都数情况下 java 开发者会使用 Java 透视图和 Debug 透视图。你可以通过工具条上的透视图名称来自由切换。

![explore_perspective_toolbar](https://www.runoob.com/wp-content/uploads/2014/12/explore_perspective_toolbar.jpg)

工具条上右击透视图名及选择"Close"项即可关闭透视图。

![explore_perspective_close](https://www.runoob.com/wp-content/uploads/2014/12/explore_perspective_close.jpg)

我们可以通过自定义透视图窗口来设置我们想要的透视图。

- 点击菜单栏上的 "Windows" => "Customize Perspective" => 弹出窗口，可以在"Submenus"里面选择你要设置的内容。
- "New" => 设置你的新建菜单，可以把你平时需要新建的最常用的文件类型选中。
- "Show Views" 在你自定义的这个视图的布局，也就是切换到你自己的视图后，会出现哪些窗口。根据自己习惯进行设置。
- Open Perspective" => 切换视图菜单中，出现哪些可以选择的视图。
- 都设置好以后，保存你的自定义透视图。Windows => Save Perspective as，然后为你自定义的透视图取一个名字，保存。

![explore_perspective_customize_1](https://www.runoob.com/wp-content/uploads/2014/12/explore_perspective_customize_1.jpg) 

# Eclipse 工作空间(Workspace)

eclipse 工作空间包含以下资源：

- 项目
- 文件
- 文件夹

项目启动时一般可以设置工作空间，你可以将其设置为默认工作空间，下次启动后无需再配置：

![1362387429_6854](https://www.runoob.com/wp-content/uploads/2014/12/1362387429_6854.png)

插件可以通过资源插件提供的API来管理工作空间的资源。

## 管理工作空间(Workspace)

用户通过使用视图，编辑器和向导功能来创建和管理工作空间中的资源。其中，显示工作区的内容很多意见中的Project Explorer视图。显示项目工作空间内容的视图是Project Explorer视图。



![eclipse_workspace_pe](https://www.runoob.com/wp-content/uploads/2014/12/eclipse_workspace_pe.jpg)![eclipse_workspace_new_file](https://www.runoob.com/wp-content/uploads/2014/12/eclipse_workspace_new_file.jpg)

文件夹(Folder)创建向导(File > New > Folder) 。

![eclipse_workspace_new_folder](https://www.runoob.com/wp-content/uploads/2014/12/eclipse_workspace_new_folder.jpg)

在菜单栏上选择 "Window" => "preferences..." => "General"=>"Workspace"，设置说明如下图：

![1363013170_2649](https://www.runoob.com/wp-content/uploads/2014/12/1363013170_2649.png)

Eclipse切换工作空间可以选择菜单栏中选择 "File" => "switch workspace"：

![20140312232833156](https://www.runoob.com/wp-content/uploads/2014/12/20140312232833156.png)

# Eclipse 创建 Java 项目

------

## 打开新建 Java 项目向导

通过新建 Java 项目向导可以很容易的创建 Java 项目。打开向导的途径有：

- 通过点击 "File" 菜单然后选择 New > Java Project
- 在项目浏览器(Project Explorer)窗口中鼠标右击任一地方选择 New > Java Project
- 在工具条上点击新建按钮 (![new_button](https://www.runoob.com/wp-content/uploads/2014/12/new_button.jpg) ) 并选择 Java Project

## 使用新建 Java 项目向导

新建 Java 项目向导有两个页面。

第一个页面:

- 输入项目名称（Project Name 栏中）
- 选择 Java Runtime Environment (JRE) 或直接采用默认的
- 选择项目布局（Project Layout），项目布局决定了源代码和 class 文件是否放置在独立的文件夹中。 推荐的选项是为源代码和 class 文件创建独立的文件夹。

![new_java_project](https://www.runoob.com/wp-content/uploads/2014/12/new_java_project.jpg)

第二个页面 [Java 构建路径设置（Java Build Settings ）](https://www.runoob.com/eclipse/eclipse-java-build-path.html)，该页面我们可以配置项目的依赖关系及额外的 jar 包。

------

## 查看新建项目

Package Explorer 显示了新建的 Java 项目。项目图标中的 "J" 字母表示 Java 项目。 文件夹图标表示这是一个 java 资源文件夹。

![new_java_project_pe](https://www.runoob.com/wp-content/uploads/2014/12/new_java_project_pe.jpg)

# Eclipse Debug 调试

------

## Debug 调试 Java 程序

我们可以在 Package Explorer 视图调试 Java 程序，操作步骤如下：

- 鼠标右击包含 main 函数的 java 类
- 选择 Debug As > Java Application

该操作也可以通过快捷键来完成，快捷键组合为 Alt + Shift + D, J。



以上操作会创建一个新的 [Debug Configuration（调试配置）](https://www.runoob.com/eclipse/eclipse-debug-configuration.htm) ，并使用该配置来启动 Java 应用。

如果 Debug Configuration（调试配置）已经创建，你可以通过 Run 菜单选择 Debug Configurations 选取对应的类并点击 Debug 按钮来启动 Java 应用。

![debug_program_1](https://www.runoob.com/wp-content/uploads/2014/12/debug_program_1.jpg)

Run 菜单的 Debug 菜单项可以重新加载之前使用了调试模式的 java 应用。

![debug_program_menu](https://www.runoob.com/wp-content/uploads/2014/12/debug_program_menu.jpg)

重新加载之前使用了调试模式的 java 应用快捷键为 F11。

当使用调试模式开启java程序时，会提示用户切换到调试的透视图。调试透视图提供了其他的视图用于排查应用程序的故障。

java 编辑器可以设置断点调试。 在编辑器中右击标记栏并选择 Toggle Breakpoint 来设置断点调试。

![debug_program_2](https://www.runoob.com/wp-content/uploads/2014/12/debug_program_2.jpg)

断点可以在标记栏中看到。也可以在 Breakpoints View（断点视图）中看到。

当程序执行到断点标记的代码时 JVM 会挂起程序，这时你可以查看内存使用情况及控制程序执行。

程序挂起时，Debug(调试)视图可以检查调用堆栈。

![debug_program_3](https://www.runoob.com/wp-content/uploads/2014/12/debug_program_3.jpg)

variables(变量)视图可以查看变量的值。

![debug_program_4](https://www.runoob.com/wp-content/uploads/2014/12/debug_program_4.jpg)

Run 菜单中有继续执行(Resume)菜单项，跳过(Step Over)一行代码，进入函数(Step Into)等。

![debug_program_5](https://www.runoob.com/wp-content/uploads/2014/12/debug_program_5.jpg)

以上图片中显示了 Resume, Step Into 和 Step Over 等关联的快捷键操作。

# Eclipse 首选项(Preferences)

## 设置首选项

该对话框可通过框架管理但是其他插件可以设置其他页面来管理首选项的配置。

我们可以通过 Window 菜单选择 Preferences 菜单项来开启该对话框。

![preferences_1](https://www.runoob.com/wp-content/uploads/2014/12/preferences_1.jpg)

首选项页面有多个分类组成。你可以在左侧菜单中展开各个节点来查看首选项的配置。

左上角的输入框可以快速查找首选项页面。 你只需在输入框中输入要查找的首选项页面的字母即可快速找到对应的首选项页面。 例如：输入 font 即可查找到 Font(字体) 首选项页面。

![preferences_2](https://www.runoob.com/wp-content/uploads/2014/12/preferences_2.jpg)

在你完成首选项页面的配置后点击 OK 按钮就可以保存配置，点击 Cancel 按钮用于放弃修改。

# Eclipse 查找

## 工作空间中查找

Eclipse 查找对话框中可以允许用户在指定工作空间上使用单词或字母模式来查找文件。 或者你可以在指定项目或在 package explorer 视图上选择好指定文件夹来查找。

可通过以下方式来调用查找框：

- 在 Search 菜单上选择 Search 或 File 或 Java
- 按下快捷键： Ctrl + H

![search_1](https://www.runoob.com/wp-content/uploads/2014/12/search_1.jpg)![search_2](https://www.runoob.com/wp-content/uploads/2014/12/search_2.jpg)

例如我们查找 Person 类型使用的情况，可以通过 Java 查找页面：

- 在查找框中输入 Person
- 在 search for 的单选按钮中选择 Type
- 在 limit to（限于）单选按钮中选择 References
- 点击 Search

Search 视图中显示结果如下：

![search_3](https://www.runoob.com/wp-content/uploads/2014/12/search_3.jpg)

# Eclipse 快捷键

## 关于快捷键

Eclipse 的很多操作都提供了快捷键功能，我们可以通过键盘就能很好的控制 Eclipse 各个功能：

- 使用快捷键关联菜单或菜单项
- 使用快捷键关联对话窗口或视图或编辑器
- 使用快捷键关联工具条上的功能按钮

![shortcuts_menu](https://www.runoob.com/wp-content/uploads/2014/12/shortcuts_menu.jpg)![shortcuts_keys](https://www.runoob.com/wp-content/uploads/2014/12/shortcuts_keys.jpg)

------

## 设置快捷键

Eclipse 系统提供的快捷键有时比较难记住，甚至根本没有提供快捷键时，就需要自己手动设置快捷键。

我们可以通过点击window->preferences->general->keys（或直接搜索keys），进入快捷键管理界面：

![shortcut1](https://www.runoob.com/wp-content/uploads/2014/12/shortcut1.jpg)

设置完快捷键后，还需要设置在什么时候可以使用该快捷键，eclipse提供各种场景供选择，一般选择In Windows(即在eclipse窗口激活状态)即可。

![shortcut3](https://www.runoob.com/wp-content/uploads/2014/12/shortcut3.jpg)

------

## Eclipse 常用快捷键

| 快捷键                                        | 描述                                                         |
| :-------------------------------------------- | :----------------------------------------------------------- |
| 编辑                                          |                                                              |
| Ctrl+1                                        | 快速修复（最经典的快捷键,就不用多说了，可以解决很多问题，比如import类、try catch包围等） |
| Ctrl+Shift+F                                  | 格式化当前代码                                               |
| Ctrl+Shift+M                                  | 添加类的import导入                                           |
| Ctrl+Shift+O                                  | 组织类的import导入（既有Ctrl+Shift+M的作用，又可以帮你去除没用的导入，很有用） |
| Ctrl+Y                                        | 重做（与撤销Ctrl+Z相反）                                     |
| Alt+/                                         | 内容辅助（帮你省了多少次键盘敲打，太常用了）                 |
| Ctrl+D                                        | 删除当前行或者多行                                           |
| Alt+↓                                         | 当前行和下面一行交互位置（特别实用,可以省去先剪切,再粘贴了） |
| Alt+↑                                         | 当前行和上面一行交互位置（同上）                             |
| Ctrl+Alt+↓                                    | 复制当前行到下一行（复制增加）                               |
| Ctrl+Alt+↑                                    | 复制当前行到上一行（复制增加）                               |
| Shift+Enter                                   | 在当前行的下一行插入空行（这时鼠标可以在当前行的任一位置,不一定是最后） |
| Ctrl+/                                        | 注释当前行,再按则取消注释                                    |
| 选择                                          |                                                              |
| Alt+Shift+↑                                   | 选择封装元素                                                 |
| Alt+Shift+←                                   | 选择上一个元素                                               |
| Alt+Shift+→                                   | 选择下一个元素                                               |
| Shift+←                                       | 从光标处开始往左选择字符                                     |
| Shift+→                                       | 从光标处开始往右选择字符                                     |
| Ctrl+Shift+←                                  | 选中光标左边的单词                                           |
| Ctrl+Shift+→                                  | 选中光标右边的单词                                           |
| 移动                                          |                                                              |
| Ctrl+←                                        | 光标移到左边单词的开头，相当于vim的b                         |
| Ctrl+→                                        | 光标移到右边单词的末尾，相当于vim的e                         |
| 搜索                                          |                                                              |
| Ctrl+K                                        | 参照选中的Word快速定位到下一个（如果没有选中word，则搜索上一次使用搜索的word） |
| Ctrl+Shift+K                                  | 参照选中的Word快速定位到上一个                               |
| Ctrl+J                                        | 正向增量查找（按下Ctrl+J后,你所输入的每个字母编辑器都提供快速匹配定位到某个单词,如果没有,则在状态栏中显示没有找到了,查一个单词时,特别实用,要退出这个模式，按escape建） |
| Ctrl+Shift+J                                  | 反向增量查找（和上条相同,只不过是从后往前查）                |
| Ctrl+Shift+U                                  | 列出所有包含字符串的行                                       |
| Ctrl+H                                        | 打开搜索对话框                                               |
| Ctrl+G                                        | 工作区中的声明                                               |
| Ctrl+Shift+G                                  | 工作区中的引用                                               |
| 导航                                          |                                                              |
| Ctrl+Shift+T                                  | 搜索类（包括工程和关联的第三jar包）                          |
| Ctrl+Shift+R                                  | 搜索工程中的文件                                             |
| Ctrl+E                                        | 快速显示当前Editer的下拉列表（如果当前页面没有显示的用黑体表示） |
| F4                                            | 打开类型层次结构                                             |
| F3                                            | 跳转到声明处                                                 |
| Alt+←                                         | 前一个编辑的页面                                             |
| Alt+→                                         | 下一个编辑的页面（当然是针对上面那条来说了）                 |
| Ctrl+PageUp/PageDown                          | 在编辑器中，切换已经打开的文件                               |
| 调试                                          |                                                              |
| F5                                            | 单步跳入                                                     |
| F6                                            | 单步跳过                                                     |
| F7                                            | 单步返回                                                     |
| F8                                            | 继续                                                         |
| Ctrl+Shift+D                                  | 显示变量的值                                                 |
| Ctrl+Shift+B                                  | 在当前行设置或者去掉断点                                     |
| Ctrl+R                                        | 运行至行(超好用，可以节省好多的断点)                         |
| 重构（一般重构的快捷键都是Alt+Shift开头的了） |                                                              |
| Alt+Shift+R                                   | 重命名方法名、属性或者变量名 （是我自己最爱用的一个了,尤其是变量和类的Rename,比手工方法能节省很多劳动力） |
| Alt+Shift+M                                   | 把一段函数内的代码抽取成方法 （这是重构里面最常用的方法之一了,尤其是对一大堆泥团代码有用） |
| Alt+Shift+C                                   | 修改函数结构（比较实用,有N个函数调用了这个方法,修改一次搞定） |
| Alt+Shift+L                                   | 抽取本地变量（ 可以直接把一些魔法数字和字符串抽取成一个变量,尤其是多处调用的时候） |
| Alt+Shift+F                                   | 把Class中的local变量变为field变量 （比较实用的功能）         |
| Alt+Shift+I                                   | 合并变量（可能这样说有点不妥Inline）                         |
| Alt+Shift+V                                   | 移动函数和变量（不怎么常用）                                 |
| Alt+Shift+Z                                   | 重构的后悔药（Undo）                                         |
| 其他                                          |                                                              |
| Alt+Enter                                     | 显示当前选择资源的属性，windows下的查看文件的属性就是这个快捷键，通常用来查看文件在windows中的实际路径 |
| Ctrl+↑                                        | 文本编辑器 上滚行                                            |
| Ctrl+↓                                        | 文本编辑器 下滚行                                            |
| Ctrl+M                                        | 最大化当前的Edit或View （再按则反之）                        |
| Ctrl+O                                        | 快速显示 OutLine（不开Outline窗口的同学，这个快捷键是必不可少的） |
| Ctrl+T                                        | 快速显示当前类的继承结构                                     |
| Ctrl+W                                        | 关闭当前Editer（windows下关闭打开的对话框也是这个，还有qq、旺旺、浏览器等都是） |
| Ctrl+L                                        | 文本编辑器 转至行                                            |
| F2                                            | 显示工具提示描述                                             |