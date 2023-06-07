---
title: 调试枚举
ms.date: 03/30/2017
helpviewer_keywords:
- debugging enumerations [.NET Framework]
- unmanaged enumerations [.NET Framework], debugging
- enumerations [.NET Framework debugging]
ms.assetid: 3af9f584-f1b4-4154-aeaa-8fce7c9f8b50
ms.openlocfilehash: bdd4b60d068677ae2a0874b589294ba220f0d854
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722994"
---
# <a name="debugging-enumerations"></a>调试枚举

本节介绍了调试 API 使用的非托管枚举。  
  
## <a name="in-this-section"></a>本节内容  

 [CLR_DEBUGGING_PROCESS_FLAGS 枚举](clr-debugging-process-flags-enumeration.md)  
 提供 [ICLRDebugging：： OpenVirtualProcess](iclrdebugging-openvirtualprocess-method.md) 方法使用的值。  
  
 [CLRDataEnumMemoryFlags 枚举](clrdataenummemoryflags-enumeration.md)  
 指示对 [ICLRDataEnumMemoryRegions：： EnumMemoryRegions](iclrdataenummemoryregions-enummemoryregions-method.md) 方法的调用应包括哪些内存区域。  
  
 [COR_PUB_ENUMPROCESS 枚举](cor-pub-enumprocess-enumeration.md)  
 标识要枚举的进程的类型。  
  
 [CorDebugBlockingReason 枚举](cordebugblockingreason-enumeration.md)  
 指定线程可能在给定对象上受到阻塞的原因。  
  
 CorDebugChainReason  
 指示启动调用链的一个或多个原因。  
  
 [“Cor调试代码调用类型”枚举](cordebugcodeinvokekind-enumeration.md)  
 描述导出函数如何调用托管代码。  
  
 [“Cor调试代码调用目的”枚举](cordebugcodeinvokepurpose-enumeration.md)  
 描述为何导出的函数会调用托管代码。  
  
 CorDebugCreateProcessFlags  
 提供可在对 [ICorDebug：： CreateProcess](icordebug-createprocess-method.md) 方法的调用中使用的其他调试选项。  
  
 [“Cor调试调试事件类型”枚举](cordebugdebugeventkind-enumeration.md)  
 指示 [DecodeEvent](icordebugprocess6-decodeevent-method.md) 方法对其信息进行解码的事件类型。  
  
 [“Cor调试解码事件标志窗口”枚举](cordebugdecodeeventflagswindows-enumeration.md)  
 提供关于 Windows 平台上的调试事件的其他信息。  
  
 CorDebugExceptionCallbackType  
 指示从 [ICorDebugManagedCallback2：： Exception](icordebugmanagedcallback2-exception-method.md) 事件进行的回调类型。  
  
 [CorDebugExceptionFlags 枚举](cordebugexceptionflags-enumeration.md)  
 提供有关异常的附加信息。  
  
 CorDebugExceptionUnwindCallbackType  
 指示在展开阶段正由回调发送信号的事件。  
  
 [CorDebugGCType 枚举](cordebuggctype-enumeration.md)  
 指示垃圾回收器是在工作站还是服务器上运行。  
  
 [CorDebugGenerationTypes 枚举](cordebuggenerationtypes-enumeration.md)  
 指定托管堆上内存区域的生成。  
  
 CorDebugHandleType  
 指示句柄类型。  
  
 [CorDebugIlToNativeMappingTypes 枚举](cordebugiltonativemappingtypes-enumeration.md)  
 指示本机指令的某一特定范围是否与特殊的代码区域相符。  
  
 CorDebugIntercept  
 指示可单步执行的代码的类型。  
  
 [CorDebugInterfaceVersion 枚举](cordebuginterfaceversion-enumeration.md)  
 指定 .NET Framework 的版本，或在其中引入了接口的 .NET Framework 的版本。  
  
 CorDebugInternalFrameType  
 标识堆栈帧的类型。  
  
 [CorDebugJITCompilerFlags 枚举](cordebugjitcompilerflags-enumeration.md)  
 包含影响托管的实时 (JIT) 编译器的行为的值。  
  
 [CorDebugJITCompilerFlagsDeprecated 枚举](cordebugjitcompilerflagsdeprecated-enumeration.md)  
 已过时。 改为使用 `CORDEBUG_JIT_DEFAULT` [CorDebugJITCompilerFlags](cordebugjitcompilerflags-enumeration.md) 枚举的成员。  
  
 CorDebugMappingResult  
 提供如何获取指令指针 (IP) 的值的详细信息。  
  
 [CorDebugMDAFlags 枚举](cordebugmdaflags-enumeration.md)  
 指定在其上激发托管调试助手 (MDA) 的线程的状态。  
  
 [CorDebugNGenPolicy 枚举](cordebugngenpolicy-enumeration.md)  
 提供用于确定调试器是否从本机映像缓存中加载本机 (NGen) 映像的值。  
  
 [CorDebugPlatform 枚举](cordebugplatform-enumeration.md)  
 提供 [ICorDebugDataTarget：： GetPlatform](icordebugdatatarget-getplatform-method.md) 方法使用的目标平台值。  
  
 [“Cor调试记录格式”枚举](cordebugrecordformat-enumeration.md)  
 描述包含本机异常调试事件相关信息的字节数组的数据格式。  
  
 CorDebugRegister  
 指定与给定处理器体系结构关联的寄存器。  
  
 [CorDebugSetContextFlag 枚举](cordebugsetcontextflag-enumeration.md)  
 指示上下文是来自堆栈上的活动（或叶）帧，还是已通过从另一个帧展开来进行计算。  
  
 [“Cor调试状态已更改”枚举](cordebugstatechange-enumeration.md)  
 描述基于进程变化必须删除的缓存数据数量。  
  
 CorDebugStepReason  
 指示一个单步执行的结果。  
  
 CorDebugThreadState  
 指定用于调试的线程的状态。  
  
 \>CorDebugUnmappedStop  
 指定未映射代码的类型，这些代码可以中断分档器代码执行。  
  
 CorDebugUserState  
 指示线程的用户状态。  
  
 [CorGCReferenceType 枚举](corgcreferencetype-enumeration.md)  
 标识要进行垃圾回收的对象的源。  
  
 [ILCodeKind 枚举](ilcodekind-enumeration.md)  
 提供用于指定调试器是否能够访问在探查器 ReJIT 检测中添加的局部变量或代码的值。  
  
 [LoggingLevelEnum 枚举](logginglevelenum-enumeration.md)  
 指示在托管线程记录事件时写入事件日志的描述性消息的严重级别。  
  
 [LogSwitchCallReason 枚举](logswitchcallreason-enumeration.md)  
 指示已对调试/跟踪开关执行的操作。  
  
 [VariableLocationType 枚举](variablelocationtype-enumeration.md)  
 指示变量的本机位置类型。  
  
 [WriteableMetadataUpdateMode 枚举](writeablemetadataupdatemode-enumeration.md)  
 提供用于指定元数据的内存中更新对调试器是否可见的值。

 [ClrDataSourceType 枚举](clrdatasourcetype-enumeration.md) 提供 CLRDATA_IL_ADDRESS_MAP 结构使用的值。

## <a name="related-sections"></a>相关章节  

 [调试 Coclass](debugging-coclasses.md)  
  
 [调试接口](debugging-interfaces.md)  
  
 [调试全局静态函数](debugging-global-static-functions.md)  
  
 [调试结构](debugging-structures.md)
