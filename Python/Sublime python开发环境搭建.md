# Sublime python开发环境搭建

## ubuntu18.04（安装sublime Text3）

1. 通过Ctrl + Alt + T打开终端或从桌面应用程序启动器搜索“终端”。当它打开时，运行命令来安装密钥：

   wget -qO - http://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add - 

2. sudo apt-get install apt-transport-https

3. 然后通过命令添加apt存储库：

   echo“deb http://download.sublimetext.com/ apt / stable /”| sudo tee /etc/apt/sources.list.d/sublime-text.list

4. sudo apt-get update

5. sudo apt-get install sublime-text

## 配置python开发环境

1. 安装Python 3

   去官网下载Python 3，网址：https://www.python.org/downloads/release/python-363/

   双击安装，勾选添加到环境变量。

   有时候会添加不成功，需要自己手动添加到PATH。

2. 安装Sublime Text 3

   去官网下载Sublime Text 3，双击安装。网址：https://www.sublimetext.com/3

   备注：官网下载的是试用版，可以在网上搜索注册码。ps, 如果用试用版，遇到注册提醒，一定要记得备份代码，否则可能会丢。

3. 配置Sublime Text 3 插件安装（以REPL插件为例）
   - 安装 Package Control插件：装了Package Control，可以方便我们管理所有插件。安装方法详见： https://packagecontrol.io/installation
   - 安装SublimeREPL插件：SublimeREPL支持各种语言解释器，方便我们在编辑器上编写完代码进行调试。具体安装步骤如下：
     - Step1.Cmd+Shift+P调出快捷命令窗口，输入install，选择Package Control:Install Package；
     - Step2.输入sublimerepl，点击选中，然后它就会在后台安装；
     - Step3.安装完之后，查看 Tools->SublimeREPL。若有这个菜单，则说明安装成功。

   - python3运行设置：

     - 首先Packages:Browse Package 找到 SublimeREPL的文件夹，再进入config文件夹，可以看到许多语言的配置文件，Python也在里面

     - 在这里新建一个Python3的文件夹，在里面新建Default.sublime-commands和Menu.sublime-menu两个文件（模仿Python文件夹）我们Python3目前只要能打开shell运行，和运行这个脚本，两个功能，因此就只要包含Python3和 Python3 – Run current file两项就好了。Default.sublime-commands配置如下：
       ```json
       [{
           "caption": "SublimeREPL: Python3",
           "command": "run_existing_window_command",
           "args":
           {
               "id": "repl_python3",
               "file": "config/Python3/Main.sublime-menu"
           }
       },
        {
            "caption": "SublimeREPL: Python3 - RUN current file",
            "command": "run_existing_window_command",
            "args": {
                "id": "repl_python3_run",
                "file": "config/Python3/Main.sublime-menu"
            }
        }
       ]
       ```

       Menu.sublime-menu配置如下：

       ```json
       [{
           "id": "tools",
           "children": [{
               "caption": "SublimeREPL",
       		"mnemonic": "R",
               "id": "SublimeREPL",
       		"children": [{
                   "caption": "Python3",
       			"id": "Python3",
       			"children": [{
                       "command": "repl_open",
                       "caption": "Python3",
                       "id": "repl_python3",
                       "mnemonic": "P",
                       "args": {
                           "type": "subprocess",
                           "encoding": "utf8",
                           "cmd": ["python3", "-i", "-u"],
                           "cwd": "$file_path",
                           "syntax": "Packages/Python/Python.tmLanguage",
                           "external_id": "python3",
                           "extend_env": {
                               "PYTHONIOENCODING": "utf-8"
                           }
                       }
                   },
                   {
                       "command": "repl_open",
                       "caption": "Python3 - RUN current file",
                       "id": "repl_python3_run",
                       "mnemonic": "R",
                       "args": {
                           "type": "subprocess",
                           "encoding": "utf8",
                           "cmd": ["python3", "-u", "$file_basename"],
                           "cwd": "$file_path",
                           "syntax": "Packages/Python/Python.tmLanguage",
                           "external_id": "python3",
                           "extend_env": {
                               "PYTHONIOENCODING": "utf-8"
                           }
                       }
                   }]
       		}]
       	}]
       }]
       ```
       注意Default.sublime-commands文件里的id 和Menu.sublime-menu文件里的id要一致，Menu.sublime-menu里的caption就是菜单栏的层级内容，cmd里的内容python3.5，就是你直接在ubuntu terminal里打的命令，而Default.sublime-commands文件里的file指向了Menu.sublime-menu文件，通过id和文件名，就可以找到相应命令的配置

     - 设置key binding:

       每次这样到菜单栏里去找，太慢，能不能像ctrl+B一样直接运行呢？

       可以的，只要设置快捷键就好了，在Preference->key Bindings-User里

       写上如下配置：
       ```json
       [
           {"keys":["f5"],
            "caption": "SublimeREPL: Python3 - RUN current file",
            "command": "run_existing_window_command", "args":
            {
                "id": "**repl_python3_run**",
                "file": "config/Python3/Main.sublime-menu"
            }
           }
       ]
       ```
       注意id还是要和Menu.sublime-menu文件里的id要一致，

## sublime Text3（linux）中文输入解决

- 更新并将系统升级到最新版本:
   `sudo apt-get update && sudo apt-get upgrade` 

- 在本地项目中克隆此目录:
   `git clone https://github.com/lyfeyaj/sublime-text-imfix.git` 

- 将你的当前目录更改为`sublime-text-imfix`:
   `cd sublime-text-imfix` 

- 运行下面的脚本：
   `./sublime-imfix` 

- 完成，重新打开你的sublime窗口，切换中文输入法，就可以输入中文了。

## 汉化：

安装localization插件即可

## 项目开发面板配置：



## anaconda 插件简要配置

preferences(偏好)==>Package Settings(包设置)

选择default settting （修改python环境）代码如下：

```json
"python_interpreter": "/home/xxg/anaconda3/bin/python3",
```

选择users settting （修改插件提示）键入如下代码：

```json
{
	"python_interpreter": "python3",
	"suppress_word_completions":true,
	"suppress_explicit_completions":true,
	"complete_parameters":true,
	"anaconda_linting": false
}
```

其他功能快捷键设置实例：

- Goto Definitions 能够在你的整个工程中查找并且显示任意一个变量，函数或者类的定义。

- Find Usage 能够快速的查找某个变量，函数或者类在某个特定文件中的什么地方被使用了。

- Show Documentation： 能够显示一个函数或者类的说明性字符串(当然，是在定义了字符串的情况下)

- 打开选项：preferences -> package setting ->Anaconda ->Key Bulidings -default

  ```json
   {
      	"command": "anaconda_goto", "keys": ["ctrl+alt+g"], "context": [
              {"key": "selector", "operator": "equal", "operand": "source.python"}
          ]},
  ```



## 深度定制

参考资料

https://sublime-text.readthedocs.io/en/latest/reference/build_systems.html

## 插件介绍：

- BracketHighlighter 插件：高亮显示匹配的括号、引号和标签。

- LESS 插件：LESS语法高亮显。

- sublime-less2css 插件：将less文件编译成css文件。

- Emmet 插件：Emmet的前身是大名鼎鼎的Zen codin。前端开发必备，HTML、CSS代码快速编写神器。（使用方法：默认快捷键 Tab）

- JsFormat 插件：avaScript代码格式化。

- ColorHighlighter 插件：显示所选颜色值的颜色，并集成了ColorPicker

- Compact Expand CSS Command 插件：使CSS属性展开及收缩，格式化CSS代码。（使用方法：按 Ctrl+Alt+[ 收缩CSS代码为一行显示，按 Ctrl+Alt+] 展开CSS代码为多行显示）

- SublimeTmpl 插件：快速生成文件模板。

  使用方法：SublimeTmpl默认的快捷键如下，如果快捷键设置冲突可能无效。

  - Ctrl+Alt+h              新建 html 文件
  - Ctrl+Alt+j               新建 javascript 文件
  - Ctrl+Alt+c               新建 css 文件
  - Ctrl+Alt+p              新建 php 文件
  - Ctrl+Alt+r               新建 ruby 文件
  - Ctrl+Alt+Shift+p     新建 python 文件

- Alignment 插件：使代码格式的自动对齐（使用方法：快捷键Ctrl+Alt+A）。
- AutoFileName 插件：动补全文件(目录)名。
- DocBlockr 插件：快速生成JavaScript (including ES6), PHP, ActionScript, Haxe, CoffeeScript, TypeScript, Java, Groovy, Objective C, C, C++ and Rust语言函数注释。（使用方法：在函数上面输入/** ,然后按 Tab 就会自动生成注释。）
- SublimeCodeIntel 插件：智能提示。
- HTML-CSS-JS Prettify 插件：HTML、CSS、JS格式化。
- SideBarEnhancements 插件：侧栏菜单扩充功能。
- View In Browser 插件：Sublime Text保存后网页自动同步更新。
- LiveReload 插件：调试网页实时自动更新（使用说明：快捷键 Ctr+Alt+V）。
- TortoiseSVN 插件：版本控制工具（win下需要安装有TortoiseSVN客户端支持）。
- Theme-Soda 插件：最受欢迎的 Sublime Text 主题之一。
- Theme-Flatland 插件：最受欢迎的 Sublime Text 主题之一。
- Theme-Nexus 插件：最受欢迎的 Sublime Text 主题之一。

sublime快捷键：

 1、通用

| 按键             | 功能                          |
| ---------------- | ----------------------------- |
| ↑↓← →            | 上下左右移动光标              |
| Alt              | 调出菜单                      |
| Ctrl + Shift + P | 调出命令板（Command Palette） |
| Ctrl + `         | 调出控制台                    |

 2、编辑

| 按键                 | 功能                             |
| -------------------- | -------------------------------- |
| Ctrl + Enter         | 在当前行下面新增一行然后跳至该行 |
| Ctrl + Shift + Enter | 在当前行上面增加一行并跳至该行   |
| Ctrl + ←/→           | 进行逐词移动                     |
| Ctrl + Shift + ←/→   | 进行逐词选择                     |
| Ctrl + ↑/↓           | 移动当前显示区域                 |
| Ctrl + Shift + ↑/↓   | 移动当前行                       |

 3、选择

| 按键                 | 功能                                                         |
| -------------------- | ------------------------------------------------------------ |
| Ctrl + D             | 选择当前光标所在的词并高亮该词所有出现的位置，再次 Ctrl +  D 选择该词出现的下一个位置，在多重选词的过程中，使用 Ctrl + K 进行跳过，使用 Ctrl  + U 进行回退，使用 Esc 退出多重编辑 |
| Ctrl + Shift + L     | 将当前选中区域打散                                           |
| Ctrl + J             | 把当前选中区域合并为一行                                     |
| Ctrl + M             | 在起始括号和结尾括号间切换                                   |
| Ctrl + Shift + M     | 快速选择括号间的内容                                         |
| Ctrl + Shift + J     | 快速选择同缩进的内容                                         |
| Ctrl + Shift + Space | 快速选择当前作用域（Scope）的内容                            |

 4、查找&替换

| 按键               | 功能                                 |
| ------------------ | ------------------------------------ |
| F3                 | 跳至当前关键字下一个位置             |
| Shift + F3         | 跳到当前关键字上一个位置             |
| Alt + F3           | 选中当前关键字出现的所有位置         |
| Ctrl + F/H         | 进行标准查找/替换，之后：            |
| Alt + C            | 切换大小写敏感（Case-sensitive）模式 |
| Alt + W            | 切换整字匹配（Whole matching）模式   |
| Alt + R            | 切换正则匹配（Regex matching）模式   |
| Ctrl + Shift + H   | 替换当前关键字                       |
| Ctrl + Alt + Enter | 替换所有关键字匹配                   |
| Ctrl + Shift + F   | 多文件搜索&替换                      |

 5、跳转

| 按键     | 功能                                                         |
| -------- | ------------------------------------------------------------ |
| Ctrl + P | 跳转到指定文件，输入文件名后可以：<br />@ 符号跳转    输入@symbol跳转到symbol符号所在的位置<br /># 关键字跳转    输入#keyword跳转到keyword所在的位置<br />: 行号跳转    输入:12跳转到文件的第12行 |
| Ctrl + R | 跳转到指定符号                                               |
| Ctrl + G | 跳转到指定行号                                               |

 6、窗口

| 按键             | 功能                                         |
| ---------------- | -------------------------------------------- |
| Ctrl + Shift + N | 创建一个新窗口                               |
| Ctrl + N         | 在当前窗口创建一个新标签                     |
| Ctrl + W         | 关闭当前标签，当窗口内没有标签时会关闭该窗口 |
| Ctrl + Shift + T | 恢复刚刚关闭的标签                           |

 7、屏幕

| 按键                        | 功能               |
| --------------------------- | ------------------ |
| F11                         | 切换至普通全屏     |
| Shift + F11                 | 切换至无干扰全屏   |
| Alt+Shift+1       Single    | 切换至独屏         |
| Alt+Shift+2       Columns:2 | 切换至纵向二栏分屏 |
| Alt+Shift+3       Columns:3 | 切换至纵向三栏分屏 |
| Alt+Shift+4       Columns:4 | 切换至纵向四栏分屏 |
| Alt+Shift+8       Rows:2    | 切换至横向二栏分屏 |
| Alt+Shift+9       Rows:3    | 切换至横向三栏分屏 |
| Alt+Shift+5       Grid      | 切换至四格式分屏   |

anaconda 插件配置

https://www.cnblogs.com/nx520zj/p/5787393.html