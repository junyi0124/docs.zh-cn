---
title: CLR ETW 提供程序
description: 查看有关两种公共语言运行时的详细信息 (CLR) Windows (ETW) 提供程序的事件跟踪： runtimne 提供程序和断开提供程序。
ms.date: 03/30/2017
helpviewer_keywords:
- ETW, CLR providers
- CLR ETW providers
ms.assetid: 0beafad4-b2c8-47f4-b342-83411d57a51f
ms.openlocfilehash: f537a2e0557f1b0434d1f303d74f9cd48f157edc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283869"
---
# <a name="clr-etw-providers"></a>CLR ETW 提供程序

公共语言运行时 (CLR) 具有两个提供程序：运行时提供程序和断开提供程序。  
  
 运行时提供程序根据启用的关键字（事件的类别）引发事件。 例如，可以通过启用 `LoaderKeyword` 关键字收集加载程序事件。  
  
 Windows (ETW) 事件的事件跟踪记录到一个文件中，该文件具有 .etl 扩展名，以后可根据需要以逗号分隔的值 ( .csv) 文件进行后期处理。 有关如何将 .etl 文件转换为 .csv 文件的信息，请参阅[控制 .NET Framework 日志记录](controlling-logging.md)。  
  
## <a name="the-runtime-provider"></a>运行时提供程序  

 运行时提供程序是主 CLR ETW 提供程序。  
  
 CLR 运行时提供程序 GUID 是 e13c0d23-ccbc-4e12-931b-d9cc2eee27e4。  
  
 有关如何使用常用工具记录并查看 CLR ETW 事件的示例，请参阅[控制 .NET Framework 日志记录](controlling-logging.md)。  
  
 除了使用 `LoaderKeyword` 之类的关键字以外，可能还需要启用用于记录会被过于频繁地引发的事件的关键字。 `StartEnumerationKeyword` 和 `EndEnumerationKeyword` 关键字启用这些事件，[CLR ETW 关键字和级别](clr-etw-keywords-and-levels.md)中对这些关键字进行了总结。  
  
## <a name="the-rundown-provider"></a>断开提供程序  

 对于某些特殊用途必须启用断开提供程序。 但对于大多数用户来说，运行时提供程序应该就足够了。  
  
 CLR 断开提供程序 GUID 是 A669021C-C450-4609-A035-5AF59AF4DF18。  
  
 通常情况下，ETW 日志记录在进程启动前启用，在进程退出后关闭。 但是，如果在执行进程的过程中打开 ETW 日志记录功能，则需要有关进程的其他信息。 例如，对于符号解析，必须记录在启用日志记录功能前已经加载的方法的方法事件。  
  
 `DCStart` 和 `DCEnd` 事件捕获进程在数据收集开始和停止时的状态。  (状态是指较高级别的信息，包括已实时 (JIT) 编译的方法和已加载的程序集。 ) 这两个事件可以提供有关进程中已发生的事件的信息;例如，JIT 编译了哪些方法，等等。  
  
 在使用断开提供程序时只引发名称中包含 `DC`、`DCStart`、`DCEnd` 或 `DCInit` 的事件。 此外，仅在使用断开提供程序时才引发这些事件。  
  
 除事件关键字筛选器以外，断开提供程序还支持 `StartRundownKeyword` 和 `EndRundownKeyword` 关键字，以便提供定向筛选。  
  
### <a name="start-rundown"></a>开始断开  

 使用 `StartRundownKeyword` 关键字启用断开提供程序的日志记录功能，会触发开始断开。 这将导致引发 `DCStart` 事件并捕获系统的状态。 在枚举开始前引发 `DCStartInit`。 在枚举结束时引发 `DCStartComplete` 事件，通知控制器数据收集已正常终止。  
  
### <a name="end-rundown"></a>结束断开  

 使用 `EndRundownKeyword` 关键字启用断开提供程序的日志记录功能，会触发结束断开。 结束断开将停止分析继续执行的进程。 `DCEnd` 事件捕获系统在分析停止时的状态。  
  
 在枚举开始前引发 `DCEndInit`。 在枚举结束时引发 `DCEndComplete` 事件，通知使用者数据收集已正常终止。 开始断开和结束断开主要用于托管符号解析。 开始断开可以为分析会话开始前已实时编译的方法提供地址范围信息。 结束断开可以为分析将关闭时已实时编译的方法提供地址范围信息。  
  
 分析会话停止时，不会自动发生结束断开。 相反，尝试执行托管符号解析的工具必须在即将停止分析之前，显式调用 CLR 断开提供程序会话，同时启用 `EndRundownKeyword` 关键字。  
  
 尽管开始断开或结束断开都能够提供有关托管符号解析的方法地址范围信息，但建议使用 `EndRundownKeyword` 关键字（提供 `DCEnd` 事件），而不使用 `StartRundownKeyword` 关键字（提供 `DCStart` 事件）。 使用 `StartRundownKeyword` 会导致在分析会话期间发生断开，这可能会干扰所分析的方案。  
  
## <a name="etw-data-collection-using-runtime-and-rundown-providers"></a>使用运行时提供程序和断开提供程序执行 ETW 数据收集  

 下面的示例演示如何以一种允许托管进程的符号解析并将影响减至最小的方式来使用 CLR 断开提供程序，而不考虑进程是在所分析的窗口内部还是外部开始或结束的。  
  
1. 使用 CLR 运行时提供程序启用 ETW 日志记录：  
  
    ```console
    xperf -start clr -on e13c0d23-ccbc-4e12-931b-d9cc2eee27e4:0x1CCBD:0x5 -f clr1.etl
    ```  
  
     日志将保存到 clr1.etl 文件中。  
  
2. 要在进程继续执行的过程中停止分析，请启动断开提供程序以捕获 `DCEnd` 事件：  
  
    ```console
    xperf -start clrRundown -on A669021C-C450-4609-A035-5AF59AF4DF18:0xB8:0x5 -f clr2.etl
    ```  
  
     这将使 `DCEnd` 事件的收集开始断开会话。 可能需要等待 30 至 60 秒钟，才能收集完所有事件。 日志将保存到 clr1.et2 文件中。  
  
3. 关闭所有 ETW 分析：  
  
    ```console
    xperf -stop clrRundown
    xperf -stop clr  
    ```  
  
4. 合并分析以便创建一个日志文件：  
  
    ```console
    xperf -merge clr1.etl clr2.etl merged.etl  
    ```  
  
     merged.etl 文件将包含来自运行时提供程序会话和断开提供程序会话的事件。  
  
 使用工具可以执行步骤 2 和步骤 3（开始断开会话然后终止分析），而不是在用户请求停止分析后立即关闭分析。 还可以执行步骤 4。  
  
## <a name="see-also"></a>另请参阅

- [公共语言运行时中的 ETW 事件](etw-events-in-the-common-language-runtime.md)
