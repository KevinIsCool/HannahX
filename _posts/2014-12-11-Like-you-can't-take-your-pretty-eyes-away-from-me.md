---
layout: post
title: Like you can't take your pretty eyes away from me
category: tech
tags:
- Windows
- Kernel
- Dll
excerpt: Harbin
---


Ntdll是一个特殊的系统支持库，主要用于子系统DLL。它包含以下两种类型的函数：
> - 系统服务分发存根（stub），它们会调用Windows执行体的系统服务。
> - 内部支持函数，供子系统、子系统DLL以及其他的原生映像文件使用。

第一组函数为Windows执行体系统服务提供接口，在用户模式下可以通过接口函数调用Windows执行体的系统服务。这样的函数超过了400个，比如：`NtCreateFile`、`NtSetEvent`等。这些函数中的大部分都可以通过Windows API来访问（除了少部分仅被用于操作系统内部）。    

对于每一个这样的函数，Ntdll包含了一个同名的入口点。函数内部的代码包含了与处理器体系架构相关的模式切换指令，通过该指令可以转换到内核模式下，从而调用系统服务分发器（System service dispatcher）。系统服务分发器在检查某些参数后，再调用真正的内核模式系统服务，其中包括`Ntoskrnl.exe`内部的实际代码。    

Ntdll也包含了很多支持函数，比如映像加载器（以`Ldr`开头的函数）、堆管理器、Windows子系统进程通信函数（以`Csr`开头的函数）。Ntdll也包含了一般性的运行库例程（以`Rtl`开头的函数）、对用户模式调试和Windows事件跟踪的支持函数（分别以`DbgUi`和`Etw`开头的函数），以及用户模式异步过程调用（APC，Asynchronous Procedure Call）分发器和异步分发器。最后，Ntdll还包含一个很小的C运行库（CRT）例程的子集，仅包括字符串和标准库中的一些例程，如：memcpy、strcpy、itoa等等。

> *From:*  **Windows Internals** Sixth Edition, Part 1 