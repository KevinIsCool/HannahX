---
layout: post
title: Windows Architecture
category: tech
tags:
- Windows
excerpt: Harbin
---

![Windows Architecture]({{ site.url }}/images/201412/Architecture.gif)    

> *From* **Windows Internals** Sixth Edition Part 1

如上图所示，Windows内核可分为三层：  
   
* 硬件抽象层（Hardware Abstraction Layer, HAL）：这一层与所有的硬件打交道，将所有与硬件相关联的代码逻辑隔离在一个专门的模块中，从而使上层实现硬件平台无关性。    
* 内核层（微内核-Micro-kernel）：该层位于HAL层之上，包含了基本的操作系统原语和功能：如，线程和进程、线程调度、中断和异常的处理、同步对象和各种同步机制。    
* 执行体（Executive）层：位于内核层之上，该层的目的是提供一些可供上层应用程序或内核驱动程序直接调用的功能和语义。Windows内核的执行体包含了一个对象管理器，用于一致地管理执行体中的对象。执行体层和内核层位于同一个二进制模块中，即内核基本模块，其名称为：ntoskrnl.exe。    

执行体层和内核层的分工是，内核层实现操作系统的基本机制，而所有的策略决定则留给执行体。执行体中的对象大多封装了一个或多个内核层对象，并且通过某种方法（如对象句柄）暴露给应用程序。这种设计体现了机制和策略分离的思想。    

Windows内核为应用程序提供了一组系统服务，供应用程序使用内核中的功能。应用程序通常不直接调用这些系统服务，而是通过一组系统DLL，最终通过ntdll.dll切换到内核模式下的执行体API函数中，以调用内核中的系统服务。Ntdll.dll是连接用户模式代码和内核模式服务的桥梁。对于内核提供的每一个系统服务，该DLL都提供了一个相应的存根函数，这些存根函数的名称都是以`Nt`作为前缀，例如：NtCreateProcess、NtOpenFile等。另外，ntdll.dll还提供了许多系统的支持函数，如：映像加载器函数（以`Ldr`为前缀）、Windows子系统进程通信函数（以`Csr`为前缀）、调试函数（以`Dbg`为前缀）、系统事件函数（以`Etw`为前缀），以及一般的运行支持函数（以`Rtl`为前缀）和字符串支持函数。

>在32为系统中，用户模式代码只能访问2GB以下的虚拟地址内存空间，而内核模式代码可以访问当前进程整个4GB虚拟地址空间范围。2GB以下称为进程地址空间，2GB以上称为系统地址空间。**实际上，在这两者之间有一段特殊的64KB地址空间，位于0x7fff0000~0x7fffffff，这两种模式下都不可以访问。**      

>这里涉及Windows系统中进程虚拟地址空间的划分问题，在Windows系统中，是采用固定地址来划分这4GB的虚拟地址的。      
  
>* **32位Windows 2000(x86和Alpha处理器)：**
>> 0x00000000-0x0000FFFF: NULL指针分配的分区；    
>> 0x00010000-0x7FFEFFFF<将近2G>: 用户模式；     
>> 0x7FFF0000-0x7FFFFFFF: 64KB禁止进入分区；     
>> 0x80000000-0xFFFFFFFF<共2G>: 内核模式；
         
>* **32位Windows 2000(x86w/3GB用户方式)**
>> 0x00000000-0x0000FFFF: NULL指针分配的分区；    
>> 0x00010000-0xBFFEFFFF: 用户模式；    
>> 0xBFFF0000-0xBFFFFFFF: 64KB禁止进入分区；        
>> 0xC0000000-0xFFFFFFFF: 内核模式；    

>*1.NULL指针分区是NULL指针的地址范围。 对这个区域的读写企图都将引发访问违规。      
2.用户分区是进程的私有领域，Win2000中，程序的可执行代码和其它用户模块均加载在这里，内存映射文件也会加载在这里。     
3.禁止访问分区只有在win2000中有。这个分区是用户分区和内核分区之间的一个隔离带，目的是为了防止用户程序违规访问内核分区。     
4.内核方式分区对用户的程序来说是禁止访问的，操作系统的代码在此。内核对象也驻留在此。*    
