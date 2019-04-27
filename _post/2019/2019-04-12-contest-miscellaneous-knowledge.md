---
layout: post    
title: 算法竞赛杂项知识    
category: contest   
tags: [contest]
---

*首先需要认识一个键盘上的特殊键， 一般在左Ctrl和左Alt之间， 图标为Windows徽标， 这个按键在Windows系统叫win键， 在Linux系统叫Super键。*    

### 一、 Ubuntu
*以Ubuntu18.04为例， 大部分操作为Linux通用， 不通用操作会注明Ubuntu特有， 或是Ubuntu18.04特有。*

### 1、 终端 
#### 1.1、 打开方式
1) Ctrl+Alt+T快捷键， 打开之后工作目录为home。
2) 桌面右键， 单击Open Terminal， 打开之后工作目录为home     
2) 文件夹中右键， 单击Open in Terminal， 打开之后工作目录为当前文件夹目录。       
#### 1.2、 使用
![img](imgs/u1.png)

### 2、 添加环境变量
+ `export PATH=${PATH}:${your_path}`       
Linux下将路径添加到${PATH}变量中， 则该路径下的可执行程序和脚本可以直接在终端下执行， 不需要指定路径。 ${PATH}中各路径以`':'`分隔， 因此可以使用以上语句向${PATH}中添加一个路径。     
+ `export JAVA_HOME=${your_jdk_home}`     
添加环境变量同样使用export。     
#### 2.1、 添加临时环境变量   
在终端下执行以上export指令， 则该环境变量只在当前终端下起作用。    
#### 2.2、 添加用户环境变量
将export指令加到`~/.bashrc`文件中， 则该环境变量在当前用户起作用。    
#### 2.3、 添加系统环境变量
将export指令加到`/etc/profile`文件中， 则该环境变量应用于系统下所有用户。     

### 4、 常用指令
#### 4.1、 ls & cd
+ `cd relative_path`    
`cd`指令后接相对当前目录的路径， 则将当前工作目录转换成目标路径。     
+ `cd /absolute_path`    
若`cd`后接路径以`/`开头， 则为绝对路径。     
+ `cd ~/home_relative_path`     
若`cd`后接路径以`~/`开头， 则为相对home目录的路径。     
+ `ls`    
列出当前目录下非隐藏文件及文件夹。     
+ `ls path`    
列出`path`目录下非隐藏文件及文件夹， `path`可能为相对当前路径， 绝对路径， 相对home路径， 规则同`cd`。     
#### 4.2、 mkdir & touch & echo   
+ `mkdir contest`     
在当前目录下创建一个名为`contest`的文件夹。      
+ `mkdir contest && cd contest`      
在当前目录下创建一个名为`contest`的文件夹并将工作目录切换到`contest`文件夹。      
+ `touch file`    
创建文件的正常姿势， 此指令可以创建一个空文件， `echo > file`事实上会在文件中写入一个换行符。   
+ `echo > file`      
将空串写入当前目录下的`file`文件， 若`file`文件不存在， 则新建一个`file`文件。此指令是终端下创建文件的非常规姿势。             
+ `echo string > file`
在文件中写入string， 并在最后添加一个换行符， 若string中存在空格， 请使用引号， 如果`file`不存在， 则新建一个文件。     
+ `echo string >> file`     
在文件结尾追加string， 其余同`echo string > file`。  

#### 4.3、 vim & gedit
Linux下一个终端编辑器， 高度可定制， 常用于查看各种文本或是在竞赛中写代码。     
+ `vim filename.ext`      
使用上述指令即可打开文件， 若文件不存在， 则创建一个新的文件。  
[Vim 使用教程](http://www.runoob.com/linux/linux-vim.html)    
+ `gedit filename.ext`     
使用`gedit`打开`filename.ext`, `gedit`是一个文本查看器， 功能请类比Windows下的记事本， 不过有语法高亮等功能。 使用gedit打开文件便于复制粘贴等操作。           

#### 4.4、 cat
+ `cat filename.ext`      
将文件内容输出到控制台。       
#### 4.5、 head & tail
+ `head -n 4 filename.ext`    
将文件前4行(4可以替换为任意其它数字)输出到控制台， 主要用于大文件的查看。 若无`-n`参数， 则输出前10行。     
+ `tail -n 4 filename.ext`     
 将文件后4行(4可以替换为任意其它数字)输出到控制台， 主要用于大文件的查看。 若无`-n`参数， 则输出后10行。     
 #### 4.6、 mv & cp
 `mv`即剪切+粘贴功能， `cp`即复制+粘贴功能。    
 + `mv source_file target_file`      
 将文件`source_file`移动到`target_file`的位置， <font color="red">若文件已存在， 会被覆写。</font>      
+ `mv source_file target_dir`          
将文件`source_file`移动到`target_dir`文件夹下， <font color="red">如果`target_dir`下存在与`source_file`同名文件， 会被覆写。</font>
+ `mv source_dir target_dir`       
将文件夹`source_dir`移动到`target_dir`的位置， 若`target_dir`不存在， 则创建一个名为`target_dir`的文件夹， 若`target_dir`已存在， 则尝试在`target_dir`下创建与`source_dir`同名文件夹， 若该同名文件夹已存在， 则`mv`执行失败。      
+ `cp source_file target_file`    
规则同`mv`。    
+ `cp source_file target_dir`      
规则同`mv`。    
+ `cp -r source_dir target_dir`    
除了加入`-r`参数外， 规则同`mv`。    

使用`mv`可以实现`rename`功能， 即`mv old_name new_name`。     

### 5、 重定向
#### 5.1、 终端重定向
*在终端下可以将标准输入输出重定向到文件， 便于向程序输入， 以及分析输出。*      
+ `<` 输入重定向， 用法： `a.out < in`， a.out程序中的`scanf`和`cin`等从标准输入读数据函数将读取`in`文件中的内容。     
+ `>` 输出重定向， 用法： `a.out > ot`， a.out程序中的`printf`和`cout`等向标准输出写数据函数将数据写入`ot`文件。      
+ `>>` 输出重定向， 用法同`>`, 与`>`不同的是， `>>`表示追加到文件末尾。        

#### 5.2、 程序内重定向
*在程序内可以通过freopen将输入重定向文件。*
```cpp
freopen("in", "r", stdin);      // 将输入重定向到 in 文件
freopen("ot", "w", stdout);     // 将输出重定向到 ot 文件
```

### 6、 对拍脚本
*为尽量提高提交准确率， 或是在在本地查找让自己程序出错的样例， 可以使用自己的程序跟标准程序对拍。*     
1) 首先， 需要写一个数据生成器， 生成随机测试数据， 假定为 `generator.cpp`。     
2) 其次， 需要提供一个标准程序， 可以是提交通过的程序， 或者， 在比赛环境下， 没有提交通过的程序， 可以自己使用暴力算法写一个超时但是结果正确的程序用来提供正确输出。     
3) 使用数据生成器生成随机输入数据， 然后分别使用自己的程序和标准程序运行得出输出数据， 比对两个输出数据， 若完全相同， 重复此过程， 否则， 记录此次输入数据， 分析出错原因。     

以上即是数据对拍需要做的工作， 由此可以写出对拍脚本。     
```sh
generator > 1.in
std_code.out < 1.in > 1.out
test_code.out < 1.in > 11.out
diff 1.out 11.out
```
以上是一个极简单的对拍脚本。      

### 二、 <S>Windows</s>(windows用户不需要知道这些)
*以win10为例， 终端使用cmd， 不使用powershell， 虽然大多操作通用， 不通用操作会注明cmd特有， 部分会给出powershell实现同样功能方式。*       
*此一项待补充。*

### 1、 终端 
1) win+R快捷键打开运行， 输入cmd， 如果使用powershell， 则输入powershell。      
2) 右键开始菜单。
### 2、 添加环境变量

### 4、 常用指令

### 5、 重定向

### 6、 对拍脚本

### 三、 C++

### 1、 终端编译&运行
`g++ source.cpp`     
将`source.cpp`编译成可执行文件，  文件名为`a.out`。     
`g++ source.cpp -o target.out`      
将`source.cpp`编译成目标可执行文件`target.out`。     
`g++ source.cpp -o target.out -g`      
将`source.cpp`编译成目标可执行文件`target.out`, 并且可以使用`gdb`调试。 一般不建议使用`gdb`调试。    
`./a.out`     
执行`a.out`可执行文件。因为直接使用文件名， 系统不会检索当前目录， 所以必须使用`./`指定当前目录。               

### 2、 IO speed
#### 2.1、 cin & cout
正常来说， `cin`和`cout`的速度略慢于`scanf`和`printf`， 但是差距一般可以忽略不计， 但是因为`cin`，`cout`要考虑兼容`scanf`，`printf`， 需要做一个同步操作， 导致实际速度明显慢于`scanf`，`printf`。 在竞赛中使用`cin`,`cout`, 习惯上关同步之后使用， 坏处是关同步之后不能使用C风格输入输出函数， 即<cstdio>头文件中的输入输出函数， 包括`scanf`,`printf`,`getchar`,`putchar`,`gets`,`puts`等。       
```cpp
ios::sync_with_stdio(false);    // 关同步
cin.tie(0);                     // 在每次读入时， 不刷新输出缓冲区
```
在主函数入口加入以上两句代码即可。     
#### 2.2、 scanf & printf
对于大多数题， 使用`scanf`和`printf`不会导致TLE， 但是坏处也很明显， C风格输入输出函数调用很繁琐， 而且不便于处理单个字符读入(`cin`忽略空白字符)。

#### 2.3、 fread
对于少部分毒瘤题来说， `scanf`也会导致超时， 这时候需要用到`fread`， 一次读入指定字节数据， 然后在代码中对其进行解析， 得到格式化的输入。      
```cpp
size_t fread( void *buffer, size_t size, size_t count, FILE *stream );
```
对于竞赛来说， 无特殊说明情况下， `stream`参数值为`stdin`。 以下是一个示例：     
```cpp
char buffer[100010];
int len;
int main() {
    int len = fread(buffer, sizeof char, 100010, stdin);
    for (int i = 0; i < len; ++i) {
        // your code here 
    }
    return 0;
}
```

### 3、 计时
对于有些迷之时间复杂度题目， 可以自己生成数据之后测试运行时间， 模板如下：    
```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;
int main() {
    clock_t start_time = clock();
    // your code write here
    clock_t end_time = clock();
    // 输出程序运行时间， 单位ms
    cout << 1000.0 * (end_time - start_time) / CLOCK_PER_SEC << endl;
    return 0;
}
```

### 4、 调试
经验表明， 单步调试是一个很蠢的做法， 它太慢了， 不如在代码中输出中间结果， 然后分析出错位置。      
一种行之有效的方法是， 在代码各个模块相接处分别添加输出语句， 观察输出结果是否符合预期， 则可以将错误定位到某一块， 或者说某几行代码。      
以如下代码为例：
```cpp
void read();
void pre_process();
void analyze();
void post_process();

int main() {
    read();
    pre_process();
    analyze();
    post_process();
    return 0;
}
```
可以考虑在代码中加入若干输出语句：    
```cpp
int main() {
    read();
    cout << "Read Succ" << endl;            // 此行用于判断程序是否执行到此
    assert(read data meets expectations);   // 此处判断输入是否符合预期， 一般采用将输入原样输出的方式测试
    pre_process();
    cout << "Pre Process Succ" << endl;     // 此行用于判断程序是否执行到此
    assert(pre process meets expectations); // 此处判断预处理结果是否符合预期， 可以输出几个关键变量查看结果
    analyze();
    // As above
    post_process();
    // As above
    return 0;
}
```
这样， 根据输出即可判断程序是否在某个模块异常退出， 以及各个模块是否得到预期结果。     

### 四、 ERROR
#### 4.1、 编译错误(CE, Compile Error)
常见报错关键字如下：   
+ ambiguous: 标识符重定义， 检查自己是否在同一个域将一个标识符定义了两次， 或是重定义了头文件中的标识符。    

#### 4.2、 运行时错误(RE, Runtime Error)
运行时错误是指在程序运行过程中发生了不可处理的错误， 会导致程序异常退出。    
常见运行时错误类型如下：    
+ 数据越界， 对于C++来说， 有时候可能数据越界很多才会报 RE 错误， 越界少的时候可能会修改相邻变量的值， 导致Wrong Answer。        
+ 除以0， 整数相除时， 如果除数为零， 会导致 RE 错误。
+ 在函数中开大数组， 导致栈溢出， 也会出现 RE 错误。
