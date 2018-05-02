# InjectCollection
A collection of injection via vc++ in ring3.

1.By the way of creating new thread in the target process to execute the kernel32 export function -- "LoadLibrary" to realize our aim of injection!
   
        Three functions I find can be used ：CreateRemoteThread、NtCreateThreadEx、RtlCreateUserThread

2.By the way of suspending one thread of our target process, and then change thread context of eip or rip to our shellcode, last resume thread. so target process will stop to execute our shellcode, our aim will also be achieved!
        
        some functions are needed, such as SuspendThread, GetThreadContext, SetThreadContext, ResumeThread

3.By the way of queueing apc in the thread apc queue, for this method request the thread should be alertable, so I queue this apc in all thread of our target process by force, but it seems to be not steady.

        main function been used is QueueUserApc
        
4.By the way of setting registry value to set global hook, almost all process being created will load our dll!

        in the HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Windows directory,
        set the value AppInit_DLLs to be our dll full path, 
        and set the value LoadAppInit_DLLs to be 0x1
        
These 4 methods above could use the dll named "NormalDll" I write for test.

5.By the way of Hooking the window message, once our target process triggered the hooked message, then it will execute export function in our dll!

        mainly used the SetWindowHookEx which is MS's API

This method should use the dll named "WindowHookDll" I write for test.

6.By the way of writing dll in the memory space of target process, and then create a thread in target thread to execute an export function in the dll we just wrote in target process. This export funcion mainly realize "LoadLibrary" by itself, so it requset the knowledge of PE structure!

﻿========================================================================
    动态链接库：LoadRemoteDll 项目概述
========================================================================

应用程序向导已为您创建了此 LoadRemoteDll DLL。

本文件概要介绍组成 LoadRemoteDll 应用程序的每个文件的内容。


LoadRemoteDll.vcxproj
    这是使用应用程序向导生成的 VC++ 项目的主项目文件，其中包含生成该文件的 Visual C++ 的版本信息，以及有关使用应用程序向导选择的平台、配置和项目功能的信息。

LoadRemoteDll.vcxproj.filters
    这是使用“应用程序向导”生成的 VC++ 项目筛选器文件。它包含有关项目文件与筛选器之间的关联信息。在 IDE 中，通过这种关联，在特定节点下以分组形式显示具有相似扩展名的文件。例如，“.cpp”文件与“源文件”筛选器关联。

LoadRemoteDll.cpp
    这是主 DLL 源文件。

/////////////////////////////////////////////////////////////////////////////
其他标准文件:

StdAfx.h, StdAfx.cpp
    这些文件用于生成名为 LoadRemoteDll.pch 的预编译头 (PCH) 文件和名为 StdAfx.obj 的预编译类型文件。

/////////////////////////////////////////////////////////////////////////////
其他注释:

应用程序向导使用“TODO:”注释来指示应添加或自定义的源代码部分。

/////////////////////////////////////////////////////////////////////////////
