---
layout: post    
title: Configure VScode(Mainly for C++)   
tags: [configure]
---

*VSCode 安装配置基础教程(Mainly for .cpp and .java)*

### Download & Install
[Download Link](https://code.visualstudio.com/)   

安装结束之后打开vscode.
![image](../../../assets/images/welcome.png)    
vscode界面最上方是菜单项， 最左侧是导航栏， 从上至下依次为Explorer， Search, Source Control, Debug, Extensions。   
选择 Open Folder， 打开一个文件夹作为工作根目录。 vscode添加新目录比较麻烦， 因而建议建立一个工作区， 在工作区下添加自己需要用到的文件夹。    
在初始界面不要选择 Open Folder， 如果已经打开了一个目录， File->Close Folder关闭当前文件夹， 然后选择File->Add Folder to WorkSpace, vscode会自动建立一个未命名工作区， 然后便可以向工作区中添加文件夹了。工作区的配置需要保存， 否则关闭之后就需要重建， 选择File->Save WorkSpace As, 建议保存在主要工作目录中， 之后就可以通过File->Open WorkSpace快速打开工作区。    
![image](../../../assets/images/init.png)     

### Extensions   
vscode本身只是一个比较好用的编辑器， 如果想配置成一个顺手的IDE， 需要通过添加各种各样的插件来完成这项工作。
![image](../../../assets/images/extensions.png)  
这是几个提供了基础功能的插件。  
C/C++: 提供了一个C/C++语言IDE的基本功能， 包括自动补全， 代码格式化， 类/函数导航， 声明/定义跳转还有最重要的debug。   
java: 几个java相关的插件是一系列的插件， 安装Java Extension Pack会一并安装， 提供了Java语言IDE的基本功能， 包括同C/C++相同的功能， 以及对Maven， Gradle， 和JUnit Test的支持。   
Coder Runner： 可以一键运行源文件， 并且可以自由配置编译运行参数。 也可以选择代码段运行。 不过没找到怎么输入， 似乎只能输出不接受输入。并且代码段要求必须是完整的， 比如C语言要包括所需的头文件， 这个插件更适合脚本语言。
AIO Compiler for C，C++&Java： 使用集成终端编译运行， 不方便的是编译和执行是两次操作， 好处是直接执行终端指令， 方便修改， 并且跟终端自行执行没有任何区别。
vscode-fileheader： 可以一键添加文件头信息， 包括创建时间， 作者， 最后一次修改时间等。  
Vim: 这个不多做介绍， 会使用Vim插件的也不会看这种教程……

### Debug for C++
新建一个cpp文件， 写一段简短的C++程序， 选择debug， 会提示选择一个debug configuration， 选择C++(GDB/LLDB), 然后vscode自动打开一个launch.json文件
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            // 指定用来执行debug的可执行文件名
            // fileDirname 为当前active file的目录， fileBasenameNoExtension 为当前active file的文件名，
            // 不包括扩展名
            // e.g. 若当前active file为test.cpp， 则debug的可执行文件名为当前目录下的test.out
            // 若在Windows环境下， 将.out改为.exe
            "program": "${fileDirname}/${fileBasenameNoExtension}.out",
            "args": [],
            // 指定是否停在程序入口， 即是否在程序入口自动加一个断点
            "stopAtEntry": true,
            "cwd": "${workspaceRoot}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            // 指定debug之前要执行什么任务， 此处compile为编译任务
            "preLaunchTask": "compile"
        }
    ]
}
```
将以上代码复制粘贴进launch.json文件， 然后继续完成compile task。
Ctrl+Shift+p -> Tasks: Configure Task -> Create tasks.json file from template -> Others, vscode会自动打开一个tasks.json文件。
```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "echoCommand": true,
    "isBackground": true,
    "tasks": [
        {
            // 任务名
            "label": "compile",
            "type": "shell",
            // 要指定的shell指令名
            "command": "g++",
            // 参数
            "args": [
                // 要编译的文件
                // fileDirname 为当前active file的目录， fileBasenameNoExtension 为当前active file的文件名，
                // 不包括扩展名
                "${fileDirname}/${fileBasenameNoExtension}.cpp",
                // 指定输出的可执行文件名
                // 若为Windows环境， 将.out改成.exe
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}.out",
                // 在编译的时候， 产生调试信息， 没有加这个参数， .out文件不能直接用于debug
                "-g",
                // 静态链接
                "-static-libgcc",
                // 编译后显示所有警告
                "-Wall",
                // 使用c++11规范
                "-std=c++11"
            ],
            "group": "build",
            "presentation": {
                "reveal": "always",
                "focus": true,
                "echo": true
            },
            "problemMatcher": "$msCompile"
        }
    ]
}
```
将以上代码复制粘贴进tasks.json， 此时， 对C++文件的debug已经配置完成， 可以尝试使用debug功能了。   

### Settings
单机界面左下角的齿轮标志， 选择settings， 打开User Settings， 在打开的json文件中修改vscode设置。
![image](../../../assets/images/settings.png)
上图是我的一些设置， 左侧为默认设置， 右侧为个人设置， 个人设置会覆盖默认设置中的相应项。   
```json
{
    "workbench.colorTheme": "Visual Studio Light",
    "editor.wordWrap": "on",
    "editor.formatOnPaste": true,
    "editor.acceptSuggestionOnEnter": "smart",
    "editor.mouseWheelZoom": true,
    "workbench.startupEditor": "none",
    "terminal.integrated.cursorStyle": "underline",
    "editor.wrappingIndent": "none",
    "editor.multiCursorModifier": "ctrlCmd",
    "java.errors.incompleteClasspath.severity": "ignore",
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.copyOnSelection": true,
    "files.autoSave": "afterDelay",
    "fileheader.Author": "SnWhisper",
    "fileheader.LastModifiedBy": "SnWhisper",
    "C_Cpp.clang_format_fallbackStyle": "{BasedOnStyle: Google ,IndentWidth: 4}",
    "editor.formatOnType": true
}
```
json文件中不能有注释， 所以将一些说明写在下面。(上面的lunch.json和tasks.json能有注释是因为vscode允许……)   
```json
{
    // 主题
    "workbench.colorTheme": "Visual Studio Light",
    // 当某一行很长是， 是否换行显示， 不换行的话行尾只能通过移动水平滚动条看到。 此处的换行在文件中实际还是一行， 只是在编辑器中显示多行。
    "editor.wordWrap": "on",
    "editor.formatOnPaste": true,
    "editor.acceptSuggestionOnEnter": "smart",
    // 是否可以通过Ctrl+鼠标滚轮改变字号
    "editor.mouseWheelZoom": true,
    // 是否在启动vscode之后显示欢迎界面， 或者新建一个未命名文件
    "workbench.startupEditor": "none",
    // 集成终端光标样式
    "terminal.integrated.cursorStyle": "underline",
    // 某一行太长导致自动换行之后是否缩进
    "editor.wrappingIndent": "none",
    // 同时选中多处快捷键， 即按住此快捷键鼠标每点击一次会留下一个光标， 之后的操作如退格， 输入字符等， 会在多个光标处进行相同的操作， 可选快捷键有ctrl和alt
    "editor.multiCursorModifier": "ctrlCmd",
    "java.errors.incompleteClasspath.severity": "ignore",
    // 集成终端光标是否闪烁
    "terminal.integrated.cursorBlinking": true,
    // 如果为true， 集成终端中被选中的内容将会被自动复制到剪贴板
    "terminal.integrated.copyOnSelection": true,
    // 文件自动保存
    "files.autoSave": "afterDelay",
    // vscode-fileheader插件的设置， 指定作者名， 和最后一次修改人的名字
    "fileheader.Author": "SnWhisper",
    "fileheader.LastModifiedBy": "SnWhisper",
    // C++格式化代码的规范，
    // “Visual Studio” 花括号换行
    // ”Google“ 花括号不换行， if的执行语句如果只有一句， 和判断语句同一行
    // “Chromium” 花括号不换行， if的执行语句一定换行
    "C_Cpp.clang_format_fallbackStyle": "{BasedOnStyle: Google ,IndentWidth: 4}",
    "editor.formatOnType": true
}
```

### Keyboard Shortcuts
这里记录几个常用的快捷键， 如果安装Vim插件， 可以忽略， 不过建议检查一下这几个键有没有和Vim冲突     

 功能  | 快捷键   
 ---- | ----    
Format Document | Ctrl+Alt+i(Ubuntu), Ctrl+Shift+f(Windows or MaxOS)     
Debug | F5   
Delete Line | Ctrl+Shift+k  
Open Preview to the Side<br/>(for markdown) | Ctrl+k, v(先按Ctrl+k， 然后再按v, 不要三个键同时按)
Show All Commands | Ctrl+Shift+p
toggle block comment<br/>切换块注释 | Ctrl+Shift+A(Ubuntu), Shift+Alt+A(Windows)
Outdent Line 减少缩进 | Ctrl+[
Indet Line 缩进 | Ctrl+]
Go to Definition | F12
Go to Declaration | Ctrl+F12
Peek Definition | Alt+F12(Ubuntu下默认Ctrl+Alt+F12， 和系统快捷键冲突， 需要更改)

这是一些很常用的快捷键， 还有其它一些诸如Ctrl+A，Z, C，V等几乎通用的快捷键不再列出。   

### Snippets
vscode提供了配置用户代码段的功能， 入口在Ctrl+Shift+p列出所有命令中搜索snippets， 选择Configure User Snippets， 选择C++， 将以下代码复制入其中。
```json
{
	"main": {
		"prefix": "main",
		"body": [
			"#include <iostream>",
			"#include <cstdio>",
			"#include <string>",
			"#include <cstring>",
			"using namespace std;",
			"",
			"int main(int argc, char const *argv[]) {",
			"\tios::sync_with_stdio(false);",
			"\tcin.tie(0);",
			"\n\t$0\n",
			"\treturn 0;",
			"}"
		],
		"description": "main function"
	}
}
```
然后选择java， 将以下代码复制入其中。
```json
"main": {
		"prefix": "main",
		"body": [
			"import java.io.*;",
			"",
			"public class $TM_FILENAME_BASE {",
			"",
			"\tpublic static void main(String[] args) {",
			"\t\t$0\n\t}\n",
			"}"
		],
		"description": "main function"
	}
```

### Integrated Terminal
vscode的集成终端会根据系统自动选择使用bash(Ubuntu)或者powershell(Windows)。 在使用上跟独立终端体验几乎一样。 不过vscode的集成终端输出有时会出错， 尤其输出的内容较乱时。

### Others
#### Compiler
vscode不提供任何语言的内置编译器， 需要自己下载并配置环境变量。
待补充……
#### Vim
待补充……
#### GDB
待补充……
#### Source Control
待补充……
