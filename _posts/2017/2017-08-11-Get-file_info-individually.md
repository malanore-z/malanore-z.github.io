---
layout: post
title: Get file_info individually by C
category: c/C++
tags: [c/C++]
---

*文件操作是一类非常常用的操作， 也是一类非常容易出错的操作。 因为不同的操作系统， 不同的文件系统……诸多要考虑的因素， 并且路径是以字符串的形式存在， 而这里出现， 编译器无法捕捉。 恰好， 连续两次有朋友让我帮忙写一个类似的程序， 就研究了一下怎么逐个地获取文件夹下文件的信息。 这里记下了问题的C语言实现*   

## <font color="blue">Windows平台</font>
### 一些介绍关于`_findnext`
要使用`_findnext`函数， 就必须在代码中包含`io.h`头文件。 在MSDN上这样介绍这个函数
> Find the next name, if any, that matches the filespec argument in a previous call to _findfirst, and then alter the fileinfo structure contents accordingly.
{% highlight cpp %}
int _findnext(  
   intptr_t handle,  
   struct _finddata_t *fileinfo   
);  
{% endhighlight %}
Parameters  	
`handle`  	
Search handle returned by a previous call to `_findfirst`.  	
`fileinfo`  	
File information buffer.  	  	

当然， MSDN上也介绍了这个函数的返回值， 有已经存在而且质量明显很好的资源不拿来用不是程序员的行事风格， 因此， 这里继续引用MSDN上的描述
>If successful, returns 0. Otherwise, returns –1 and sets errno to a value indicating the nature >of the failure. Possible error codes are shown in the following table.  	
><font color="purple">EINVAL</font>  	
>Invalid parameter: fileinfo was NULL. Or, the operating system returned an unexpected error.  
><font color="purple">ENOENT</font>  
>No more matching files could be found.  
><font color="purple">ENOMEM</font>  
>Not enough memory or the file name's length exceeded MAX_PATH.  
>If an invalid parameter is passed in, these functions invoke the invalid parameter handler, as >described in Parameter Validation.  

对于`_findnext`函数， 在使用其之前， 需要先使用 `_findfirst` 函数获取需要查找的文件句柄， 句法为
{% highlight cpp %}
long handle = _findfirst(path, &fileInfo);
{% endhighlight %}
`path` 为需要查找的文件路径， `fileInfo` 为结构体 `_finddata_t` 变量。

### `_finddata_t`结构体
{% highlight cpp %}
struct _finddata_t
{
//File attribute.
    unsigned    attrib;

//Time of file creation (–1L for FAT file systems).
//This time is stored in UTC format. To convert to the local time, use localtime_s.
    __time32_t  time_create;    // -1 for FAT file systems

//Time of the last file access (–1L for FAT file systems).
//This time is stored in UTC format. To convert to the local time, use localtime_s.
    __time32_t  time_access;    // -1 for FAT file systems

//Time of the last write to file.
//This time is stored in UTC format. To convert to the local time, use localtime_s.
    __time32_t  time_write;

//Length of the file in bytes.
    _fsize_t    size;

//Null-terminated name of matched file or directory, without the path.
    char        name[260];
};
{% endhighlight %}
**当然， 原始的`_finddata_t`结构体定义并不完全如此， 因为涉及到多种变体， `_finddata_t`实际上是一个宏。  
当然， 毕竟涉及到了文件信息， 还有着一些因为不同文件系统而不同的限制或是特例， 参照  
[Portal: MSDN detailed documentation](https://msdn.microsoft.com/en-us/library/kda16keh.aspx)。**  
`_finddata_t` 结构体成员 `attrib` 存储文件/文件夹的属性， 在 `io.h` 中定义了几个宏作为文件夹属性的值， 仍然引用MSDN上的说明
>You cannot specify target attributes (such as _A_RDONLY) to limit the find operation. These attributes are returned in the attrib field of the _finddata_t structure and can have the following values (defined in IO.h). Users should not rely on these being the only values possible for the attrib field.
<font color="purple">_A_ARCH</font>  
Archive. Set whenever the file is changed and cleared by the BACKUP command. Value: 0x20.
<font color="purple">_A_HIDDEN</font>  
Hidden file. Not generally seen with the DIR command, unless you use the /AH option. Returns information about normal files and files that have this attribute. Value: 0x02.
<font color="purple">_A_NORMAL</font>  
Normal. File has no other attributes set and can be read or written to without restriction. Value: 0x00.
<font color="purple">_A_RDONLY</font>  
Read-only. File cannot be opened for writing and a file that has the same name cannot be created. Value: 0x01.
<font color="purple">_A_SUBDIR</font>  
Subdirectory. Value: 0x10.
<font color="purple">_A_SYSTEM</font>  
System file. Not ordinarily seen with the DIR command, unless the /A or /A:S option is used. Value: 0x04.

对于查找文件来说， 只有 `attrib` 和 `name` 是有用的， `attrib` 句法如下
{% highlight cpp %}
if (fileInfo.attrib & _A_SUBDIR)
	//the attribute of file is directory
{% endhighlight %}
### 代码实现
`_findnext` 函数功能已经做了说明， 然后就是将之用于代码中， 我很快完成了这一步， 并认为它可以工作的很好
{% highlight cpp %}
void dirTrace(string currentPath) {
	//struct _finddata_t used to storage file info
	_finddata_t fileInfo;
	//file handle, if -1, no sundirectory or error
	long fileHandle = _findfirst((currentPath).c_str(), &fileInfo);

	//if the value of fileHandle is -1, there is no subdirectory, return
	if (fileHandle == -1)
		return;

	do {
		//if subdirectory, recursive search
		if (fileInfo.attrib & _A_SUBDIR) {
			if (strcmp(fileInfo.name, ".") && strcmp(fileInfo.name, ".."))
				dirTrace(currentPath + "\\" + string(fileInfo.name));
		}
		//if others(archive, hidden, system, normal, or readonly), deal with it
		else {
			//deal with the file
            //TODO:
		}
	} while (!_findnext(fileHandle, &fileInfo));//judge if exit

	//close file handle, it's important if you would not like program crash!!!
	_findclose(fileHandle);
{% endhighlight %}
毫无疑问， 编译时没有问题的， 很顺利， 除了我在我的Ubuntu上用gcc编译提示我没有 `io.h` 这一点小小的失误之外， 转到Windows下很好的通过了编译， 我在`int main()`函数中， 给`dirTrace`函数传入了一个根目录， 并用“打印文件名”功能替代了了‘TODO’， 我认为我的代码会递归打印出根目录下所有文件名， 实际结果却是什么都没有打印。  
在我仔细的看了相关的函数 `_findnext`, `_findfirst` 之后， 发现`_findfirst`返回的是传入文件的句柄， 而我需要的是返回传入文件夹下第一个文件的句柄， 所以我将代码改成了这样
{% highlight cpp %}
//......
long fileHandle = _findfirst((currentPath + "\\*").c_str(), &fileInfo);
//......
dirTrace(currentPath + "\\" + string(fileInfo.name));
//......
{% endhighlight %}
在给`_findfirst` 传入参数的时候， 并不是传入文件夹名， 而是通过通配符*传入文件夹下所有文件， 这样， 代码就可以很好的工作， 实现逐个查找文件夹下所有文件的功能。

### 完整代码
{% highlight cpp %}
#include<iostream>
#include<string>
#include<cstring>
#include<io.h>
using namespace std;

void dealWith(_finddata_t fileInfo) {
	//TODO
}

void dirTrace(string currentPath) {
	//struct _finddata_t used to storage file info
	_finddata_t fileInfo;
	//file handle, if -1, no sundirectory or error
	long fileHandle = _findfirst((currentPath + "\\*").c_str(), &fileInfo);

	//if the value of fileHandle is -1, there is no subdirectory, return
	if (fileHandle == -1)
		return;

	do {
		//if subdirectory, recursive search
		if (fileInfo.attrib & _A_SUBDIR) {
			if (strcmp(fileInfo.name, ".") && strcmp(fileInfo.name, ".."))
				dirTrace(currentPath + "\\" + string(fileInfo.name));
		}
		//if others(archive, hidden, system, normal, or readonly), deal with it
		else {
			dealWith(currentPath + fileInfo.name, fileInfo);
		}
	} while (!_findnext(fileHandle, &fileInfo));//judge if exit

	//close file handle, it's important if you would not like program crash!!!
	_findclose(fileHandle);
}

int main() {
	//root path, select your path start work
	string rootPath = "E:\\Snake";

	dirTrace(rootPath);

	return 0;
}
{% endhighlight %}

## <font color="red">Linux 平台</font>
