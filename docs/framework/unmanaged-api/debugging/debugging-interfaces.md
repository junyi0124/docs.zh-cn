---
title: 调试接口
ms.date: 02/07/2019
helpviewer_keywords:
- unmanaged interfaces [.NET Framework], debugging
- debugging interfaces [.NET Framework]
- interfaces [.NET Framework debugging]
ms.assetid: b6297c26-7624-4431-8af4-14112d07bcd5
ms.openlocfilehash: a3dd81ceaab2ba467d4c8ca091c1c2219040a273
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676291"
---
# <a name="debugging-interfaces"></a>调试接口

本节描述进行程序调试处理的非托管接口，所调试的程序在公共语言运行时 (CLR) 中执行。  
  
## <a name="in-this-section"></a>本节内容  

 [ICLRDataEnumMemoryRegions 接口](iclrdataenummemoryregions-interface.md)\
 提供对由调用方指定的内存区域进行枚举的方法。  
  
 [ICLRDataEnumMemoryRegionsCallback 接口](iclrdataenummemoryregionscallback-interface.md)\
 为 `EnumMemoryRegions` 提供一种回调方法，用于向调试器报告尝试枚举指定内存区域的结果。  
  
 [ICLRDataTarget 接口](iclrdatatarget-interface.md)\
 提供与目标 CLR 进程进行交互的方法。  
  
 [ICLRDataTarget2 接口](iclrdatatarget2-interface.md)\
 数据访问服务层在目标进程中操作虚拟内存区域时所用的 `ICLRDataTarget` 的子类。  
  
 [ICLRDataTarget3 接口](iclrdatatarget3-interface.md)\
 提供对异常信息的访问的 [ICLRDataTarget2](iclrdatatarget2-interface.md) 的子类。  
  
 [ICLRDebugging 接口](iclrdebugging-interface.md)\
 提供一些方法，用于处理模块的加载和卸载以进行调试。  
  
 [ICLRDebuggingLibraryProvider 接口](iclrdebugginglibraryprovider-interface.md)\
 包括 [ProvideLibrary 方法](iclrdebugginglibraryprovider-providelibrary-method.md) 方法，该方法可获取一个库提供程序回调接口，该接口允许根据需要定位和加载特定于公共语言运行时版本的调试库。  
  
 [ICLRMetadataLocator 接口](iclrmetadatalocator-interface.md)\
 数据访问服务层用于在目标进程中定位程序集的元数据的接口。  
  
 [ICorDebug 接口](icordebug-interface.md)\
 提供允许开发人员在 CLR 环境中调试应用程序的方法。  
  
 [ICorDebugAppDomain 接口](icordebugappdomain-interface.md)\
 提供用于调试应用程序域的方法。  
  
 [ICorDebugAppDomain2 接口](icordebugappdomain2-interface.md)\
 提供处理数组、指针、函数指针和 ByRef 类型的方法。 此接口是 `ICorDebugAppDomain` 接口的扩展。  
  
 [ICorDebugAppDomain3 接口](icordebugappdomain3-interface.md)\
 提供用于在应用程序域中使用 Windows 运行时类型的方法。 此接口是 `ICorDebugAppDomain` 和 `ICorDebugAppDomain2` 接口的扩展。  
  
 [ICorDebugAppDomain4 接口](icordebugappdomain4-interface.md)\
 对 [ICorDebugAppDomain](icordebugappdomain-interface.md) 接口进行逻辑扩展，以从 COM 可调用包装器获取托管对象。  
  
 [ICorDebugAppDomainEnum 接口](icordebugappdomainenum-interface.md)\
 提供一种方法，此方法从枚举中的下一个位置开始，返回指定数目的 `ICorDebugAppDomain` 值。  
  
 [ICorDebugArrayValue 接口](icordebugarrayvalue-interface.md)\
 表示一维或多维数组的 `ICorDebugHeapValue` 的子类。  
  
 [ICorDebugAssembly 接口](icordebugassembly-interface.md)\
 表示一个程序集。  
  
 [ICorDebugAssembly2 接口](icordebugassembly2-interface.md)\
 表示一个程序集。 此接口是 `ICorDebugAssembly` 接口的扩展。  
  
 [ICorDebugAssembly3 接口](icordebugassembly3-interface.md)\
 对 [ICorDebugAssembly](icordebugassembly-interface.md) 接口进行逻辑扩展，以便为容器程序集及其包含的程序集提供支持。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugAssemblyEnum 接口](icordebugassemblyenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugAssembly` 数组。  
  
 [ICorDebugBlockingObjectEnum 接口](icordebugblockingobjectenum-interface.md)\
 为 [CorDebugBlockingObject](cordebugblockingobject-structure.md) 结构的列表提供枚举器。  
  
 [ICorDebugBoxValue 接口](icordebugboxvalue-interface.md)\
 表示装箱的值类对象的 `ICorDebugHeapValue` 的子类。  
  
 [ICorDebugBreakpoint 接口](icordebugbreakpoint-interface.md)\
 表示函数中的断点，或值的观察点。  
  
 [ICorDebugBreakpointEnum 接口](icordebugbreakpointenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugBreakpoint` 数组。  
  
 [ICorDebugChain 接口](icordebugchain-interface.md)\
 表示一个物理或逻辑调用堆栈段。  
  
 [ICorDebugChainEnum 接口](icordebugchainenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugChain` 数组。  
  
 [ICorDebugClass 接口](icordebugclass-interface.md)\
 表示基类型或复杂类型（即用户定义的类型）。 如果该类型为泛型类型，则 `ICorDebugClass` 表示实例化的泛型类型。  
  
 [ICorDebugClass2 接口](icordebugclass2-interface.md)\
 表示泛型类或具有 <xref:System.Type> 类型的方法参数的类。 此接口扩展了 `ICorDebugClass`。  
  
 [ICorDebugCode 接口](icordebugcode-interface1.md)\
 表示 Microsoft 中间语言 (MSIL) 代码段或本机代码段。  
  
 [ICorDebugCode2 接口](icordebugcode2-interface.md)\
 提供扩展 `ICorDebugCode` 的功能的方法。  
  
 [ICorDebugCode3 接口](icordebugcode3-interface.md)\
 提供扩展 [ICorDebugCode](icordebugcode-interface1.md) 和 [ICorDebugCode2](icordebugcode2-interface.md) 的方法，以提供有关托管返回值的信息。  
  
 [ICorDebugCode4 接口](icordebugcode4-interface.md)\
 提供一个方法，该方法使调试器可以枚举函数中的局部变量和参数。  
  
 [ICorDebugCodeEnum 接口](icordebugcodeenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugCode` 数组。  
  
 [ICorDebugComObjectValue 接口](icordebugcomobjectvalue-interface.md)\
 提供用来检索缓存的接口对象的方法。  
  
 [ICorDebugContext 接口](icordebugcontext-interface.md)\
 表示一个上下文对象。 此接口尚未实现。  
  
 [ICorDebugController 接口](icordebugcontroller-interface.md)\
 表示可以控制代码执行上下文的 <xref:System.Diagnostics.Process> 或 <xref:System.AppDomain> 范围。  
  
 [ICorDebugDataTarget 接口](icordebugdatatarget-interface.md)\
 提供一个回调接口，该接口可提供对特定目标进程的访问。  
  
 [ICorDebugDataTarget2 接口](icordebugdatatarget2-interface.md)\
 对 [ICorDebugDataTarget](icordebugdatatarget-interface.md) 接口进行逻辑扩展。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugDataTarget3 接口](icordebugdatatarget3-interface.md)\
 对 [ICorDebugDataTarget](icordebugdatatarget-interface.md) 接口进行逻辑扩展，以提供有关已加载模块的信息。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugDebugEvent 接口](icordebugdebugevent-interface.md)\
 定义所有 `ICorDebug` 调试事件派生的基接口。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugEditAndContinueErrorInfo 接口](icordebugeditandcontinueerrorinfo-interface.md)\
 已过时。 不要使用此接口。  
  
 [ICorDebugEditAndContinueSnapshot 接口](icordebugeditandcontinuesnapshot-interface.md)\
 已过时。 不要使用此接口。  
  
 [ICorDebugEnum 接口](icordebugenum-interface1.md)\
 作为调试枚举器的抽象基接口。  
  
 [ICorDebugErrorInfoEnum 接口](icordebugerrorinfoenum-interface.md)\
 已过时。 不要使用此接口。  
  
 [ICorDebugEval 接口](icordebugeval-interface.md)\
 提供使调试器能够在正在调试的代码的上下文中执行代码的方法。  
  
 [ICorDebugEval2 接口](icordebugeval2-interface.md)\
 扩展 `ICorDebugEval` 以对泛型类型提供支持。  
  
 [ICorDebugExceptionDebugEvent 接口](icordebugexceptiondebugevent-interface.md)\
 扩展 [ICorDebugDebugEvent](icordebugdebugevent-interface.md) 接口以支持异常事件。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugExceptionObjectCallStackEnum 接口](icordebugexceptionobjectcallstackenum-interface.md)\
 为嵌入在异常对象中的调用堆栈信息提供枚举器。  
  
 [ICorDebugExceptionObjectValue 接口](icordebugexceptionobjectvalue-interface.md)\
 扩展 [ICorDebugObjectValue](icordebugobjectvalue-interface.md) 接口以提供来自托管异常对象的堆栈跟踪信息。  
  
 [ICorDebugFrame 接口](icordebugframe-interface.md)\
 表示当前堆栈上的帧。  
  
 [ICorDebugFrameEnum 接口](icordebugframeenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugFrame` 数组。  
  
 [ICorDebugFunction 接口](icordebugfunction-interface1.md)\
 表示一个托管函数或方法。  
  
 [ICorDebugFunction2 接口](icordebugfunction2-interface.md)\
 对 `ICorDebugFunction` 进行逻辑扩展，以支持“仅我的代码”的单步执行调试。  
  
 [ICorDebugFunction3 接口](icordebugfunction3-interface.md)\
 对 [ICorDebugFunction](icordebugfunction-interface1.md) 接口进行逻辑扩展，以提供对 ReJIT 请求中的代码的访问。  
  
 [ICorDebugFunctionBreakpoint 接口](icordebugfunctionbreakpoint-interface.md)\
 扩展 `ICorDebugBreakpoint` 以支持函数中的断点。  
  
 [ICorDebugGCReferenceEnum 接口](icordebuggcreferenceenum-interface.md)\
 提供针对将进行垃圾回收的对象的枚举器。  
  
 [ICorDebugGenericValue 接口](icordebuggenericvalue-interface.md)\
 应用于所有值的 `ICorDebugValue` 的子类。 此接口可为值提供 Get 和 Set 方法。  
  
 [ICorDebugGuidToTypeEnum 接口](icordebugguidtotypeenum-interface.md)\
 提供针对映射 GUID 的对象及其相应的 `ICorDebugType` 对象的枚举器。  
  
 [ICorDebugHandleValue 接口](icordebughandlevalue-interface.md)\
 `ICorDebugReferenceValue` 的一个子类，前者表示调试器已为其创建了垃圾回收句柄的引用值。  
  
 [ICorDebugHeapEnum 接口](icordebugheapenum-interface.md)\
 提供针对托管堆上的对象的枚举器。  
  
 [ICorDebugHeapSegmentEnum 接口](icordebugheapsegmentenum-interface.md)\
 提供针对托管堆的内存区域的枚举器。  
  
 [ICorDebugHeapValue 接口](icordebugheapvalue-interface.md)\
 表示 CLR 垃圾回收器已收集的对象的 `ICorDebugValue` 的子类。  
  
 [ICorDebugHeapValue2 接口](icordebugheapvalue2-interface1.md)\
 对运行时句柄提供支持的 `ICorDebugHeapValue` 的扩展。  
  
 [ICorDebugHeapValue3 接口](icordebugheapvalue3-interface.md)\
 公开对象的监视器锁属性。  
  
 [ICorDebugILCode 接口](icordebugilcode-interface.md)\
 表示中间语言 (IL) 代码段。  
  
 [ICorDebugILCode2 接口](icordebugilcode2-interface.md)\
 对 [ICorDebugILCode](icordebugilcode-interface.md) 接口进行逻辑扩展，以提供返回函数的局部变量签名的标记的方法，并将探查器检测到的中间语言 (il) 偏移量映射到原始方法的 il 偏移量。  
  
 [ICorDebugILFrame 接口](icordebugilframe-interface.md)\
 表示 MSIL 代码的堆栈帧。  
  
 [ICorDebugILFrame2 接口](icordebugilframe2-interface.md)\
 `ICorDebugILFrame` 的逻辑扩展。  
  
 [ICorDebugILFrame3 接口](icordebugilframe3-interface.md)\
 提供用于封装函数的返回值的方法。  
  
 [ICorDebugILFrame4 接口](icordebugilframe4-interface.md)\
 提供使你能够访问中间语言 (IL) 代码的堆栈帧中的局部变量和代码的方法。 一个用于指定调试器是否可以访问在探查器 ReJIT 检测中添加的变量和代码的参数。  
  
 [ICorDebugInstanceFieldSymbol 接口](icordebuginstancefieldsymbol-interface.md)\
 表示某一实例字段的调试符号信息。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugInternalFrame 接口](icordebuginternalframe-interface.md)\
 标识调试器的帧类型。  
  
 [ICorDebugInternalFrame2 接口](icordebuginternalframe2-interface.md)\
 提供有关内部帧的信息，包括堆栈地址和相对于 [ICorDebugFrame](icordebugframe-interface.md) 对象的位置。  
  
 [ICorDebugLoadedModule 接口](icordebugloadedmodule-interface.md)\
 提供有关已加载模块的信息。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugManagedCallback 接口](icordebugmanagedcallback-interface.md)\
 提供用于处理调试器回调的方法。  
  
 [ICorDebugManagedCallback2 接口](icordebugmanagedcallback2-interface.md)\
 提供支持调试器异常处理和托管调试助手 (MDA) 的方法。 `ICorDebugManagedCallback2` 是 `ICorDebugManagedCallback` 的逻辑扩展。  
  
 [ICorDebugManagedCallback3 接口](icordebugmanagedcallback3-interface.md)\
 提供一个回调方法，该方法指示已发出启用的自定义调试器通知。  
  
 [ICorDebugMDA 接口](icordebugmda-interface.md)\
 表示托管调试助手 (MDA) 消息。  
  
 [ICorDebugMemoryBuffer 接口](icordebugmemorybuffer-interface.md)\
 表示内存中缓冲区。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugMergedAssemblyRecord 接口](icordebugmergedassemblyrecord-interface.md)\
 提供有关合并的程序集的信息。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugMetaDataLocator 接口](icordebugmetadatalocator-interface.md)\
 向调试器提供元数据信息。  
  
 [ICorDebugModule 接口](icordebugmodule-interface.md)\
 表示 CLR 模块，它是可执行文件或动态链接库 (DLL)。  
  
 [ICorDebugModule2 接口](icordebugmodule2-interface.md)\
 用作 `ICorDebugModule` 的逻辑扩展。  
  
 [ICorDebugModule3 接口](icordebugmodule3-interface.md)\
 为动态模块创建符号读取器。  
  
 [ICorDebugModuleBreakpoint 接口](icordebugmodulebreakpoint-interface.md)\
 扩展 `ICorDebugBreakpoint` 以提供对特定模块的访问。  
  
 [ICorDebugModuleDebugEvent 接口](icordebugmoduledebugevent-interface.md)\
 扩展 [ICorDebugDebugEvent](icordebugdebugevent-interface.md) 接口以支持模块级事件。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugModuleEnum 接口](icordebugmoduleenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugModule` 数组。  
  
 [ICorDebugMutableDataTarget 接口](icordebugmutabledatatarget-interface.md)\
 扩展 [ICorDebugDataTarget](icordebugdatatarget-interface.md) 接口以支持可变数据目标。  
  
 [ICorDebugNativeFrame 接口](icordebugnativeframe-interface.md)\
 用于本机帧的 `ICorDebugFrame` 的专用实现。  
  
 [ICorDebugNativeFrame2 接口](icordebugnativeframe2-interface.md)\
 提供用于测试子帧与父帧关系的方法。  
  
 [ICorDebugObjectEnum 接口](icordebugobjectenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并通过对象数组的相对虚拟地址 (RVA) 对其进行枚举。  
  
 [ICorDebugObjectValue 接口](icordebugobjectvalue-interface.md)\
 表示包含对象的值的 `ICorDebugValue` 的子类。  
  
 [ICorDebugObjectValue2 接口](icordebugobjectvalue2-interface.md)\
 扩展 `ICorDebugObjectValue` 以支持继承和重写。  
  
 [ICorDebugProcess 接口](icordebugprocess-interface.md)\
 表示正在执行托管代码的进程。  
  
 [ICorDebugProcess2 接口](icordebugprocess2-interface1.md)\
 `ICorDebugProcess` 的逻辑扩展。  
  
 [ICorDebugProcess3 接口](icordebugprocess3-interface.md)\
 控制自定义调试器通知。

 [ICorDebugProcess4 接口](icordebugprocess4-interface.md)\
 为进程外执行控制提供支持。
  
 [ICorDebugProcess5 接口](icordebugprocess5-interface.md)\
 扩展 [ICorDebugProcess](icordebugprocess-interface.md) 接口以支持对托管堆的访问，以提供有关托管对象的垃圾回收的信息，以及确定调试器是否从应用程序的本地本机映像缓存中加载图像。  
  
 [ICorDebugProcess6 接口](icordebugprocess6-interface.md)\
 对 [ICorDebugProcess](icordebugprocess-interface.md) 接口进行逻辑扩展，以启用一些功能，如对以本机异常调试事件和虚拟模块拆分方式编码的托管调试事件进行解码。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugProcess7 接口](icordebugprocess7-interface.md)\
 提供了一种方法，用于配置调试器以处理目标进程中的内存中元数据更新。  
  
 [ICorDebugProcess8 接口](icordebugprocess8-interface.md)\
 对 [ICorDebugProcess](icordebugprocess-interface.md) 接口进行逻辑扩展，以启用或禁用某些类型的 [ICorDebugManagedCallback2](icordebugmanagedcallback2-interface.md) 异常回调。  
  
 [ICorDebugProcessEnum 接口](icordebugprocessenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugProcess` 数组。  
  
 [ICorDebugReferenceValue 接口](icordebugreferencevalue-interface.md)\
 `ICorDebugValue` 的子类，它支持引用类型。  
  
 [ICorDebugRegisterSet 接口](icordebugregisterset-interface.md)\
 表示在当前正在执行代码的计算机上可用的寄存器组。  
  
 [ICorDebugRegisterSet2 接口](icordebugregisterset2-interface.md)\
 为具有 64 个以上寄存器的硬件平台扩展 `ICorDebugRegisterSet` 的功能。  
  
 [ICorDebugRemote 接口](icordebugremote-interface.md)\
 提供启动托管调试器或将其附加到远程目标进程的能力。  
  
 [ICorDebugRemoteTarget 接口](icordebugremotetarget-interface.md)\
 提供使你能够在 CLR 环境中调试基于 Silverlight 的应用程序的方法。  
  
 [ICorDebugRuntimeUnwindableFrame 接口](icordebugruntimeunwindableframe-interface.md)\
 提供对非托管方法的支持，这些方法需要公共语言运行时 (CLR) 来展开帧。  
  
 [ICorDebugStackWalk 接口](icordebugstackwalk-interface.md)\
 提供用于获取线程堆栈上的托管方法或帧的方法。  
  
 [ICorDebugStaticFieldSymbol 接口](icordebugstaticfieldsymbol-interface.md)\
 表示某个静态字段的调试符号信息。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugStepper 接口](icordebugstepper-interface.md)\
 表示在代码执行过程中由调试器执行的一个步骤。此步骤作为命令颁发和完成之间的标识符使用，可以实现取消对某个步骤的执行。  
  
 [ICorDebugStepper2 接口](icordebugstepper2-interface1.md)\
 提供对“仅我的代码”(JMC) 调试的支持。  
  
 [ICorDebugStepperEnum 接口](icordebugstepperenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugStepper` 数组。  
  
 [ICorDebugStringValue 接口](icordebugstringvalue-interface.md)\
 应用于字符串值的 `ICorDebugHeapValue` 的子类。  
  
 [ICorDebugSymbolProvider 接口](icordebugsymbolprovider-interface.md)\
 提供可用于检索调试符号信息的方法。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugSymbolProvider2 接口](icordebugsymbolprovider2-interface.md)\
 以逻辑方式扩展 [ICorDebugSymbolProvider](icordebugsymbolprovider-interface.md) 接口以检索其他调试符号信息。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugThread 接口](icordebugthread-interface.md)\
 表示进程中的线程。 `ICorDebugThread` 实例的生存期与它表示的线程的生存期相同。  
  
 [ICorDebugThread2 接口](icordebugthread2-interface.md)\
 用作 `ICorDebugThread` 的逻辑扩展。  
  
 [ICorDebugThread3 接口](icordebugthread3-interface.md)\
 提供 [ICorDebugStackWalk](icordebugstackwalk-interface.md) 和相应接口的入口点。  
  
 [ICorDebugThread4 接口](icordebugthread4-interface.md)\
 提供线程阻塞信息。  
  
 [ICorDebugThreadEnum 接口](icordebugthreadenum-interface1.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugThread` 数组。  
  
 [ICorDebugType 接口](icordebugtype-interface.md)\
 表示基类型或复杂类型（即用户定义的类型）。 如果该类型是泛型类型，则 `ICorDebugType` 表示未实例化的泛型类型。  
  
 [ICorDebugType2 接口](icordebugtype2-interface.md)\
 扩展 [ICorDebugType](icordebugtype-interface.md) 接口以检索基类型或复杂 (用户定义的) 类型的类型标识符。  
  
 [ICorDebugTypeEnum 接口](icordebugtypeenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugType` 数组。  
  
 [ICorDebugUnmanagedCallback 接口](icordebugunmanagedcallback-interface.md)\
 提供与 CLR 没有直接关系的本机事件的通知。  
  
 [ICorDebugValue](icordebugvalue-interface.md)\
 表示正在调试的进程中的读取或写入值。  
  
 [ICorDebugValue2](icordebugvalue2-interface.md)\
 扩展 `ICorDebugValue` 以提供对 `ICorDebugType` 的支持。  
  
 [ICorDebugValue3 接口](icordebugvalue3-interface.md)\
 扩展了 "ICorDebugValue" 和 "ICorDebugValue2" 接口，为大于 2 GB 的数组提供支持。  
  
 [ICorDebugValueBreakpoint](icordebugvaluebreakpoint-interface.md)\
 扩展 `ICorDebugBreakpoint` 以提供对特定值的访问。  
  
 [ICorDebugValueEnum](icordebugvalueenum-interface.md)\
 实现 `ICorDebugEnum` 方法，并枚举 `ICorDebugValue` 数组。  
  
 [ICorDebugVariableHome 接口](icordebugvariablehome-interface.md)\
 表示函数的局部变量或自变量。  
  
 [ICorDebugVariableHomeEnum 接口](icordebugvariablehomeenum-interface.md)\
 提供对函数中的局部变量和参数的枚举器。  
  
 [ICorDebugVariableSymbol 接口](icordebugvariablesymbol-interface.md)\
 检索变量的调试符号信息。 **仅在 .NET Native 上可用。**  
  
 [ICorDebugVirtualUnwinder 接口](icordebugvirtualunwinder-interface.md)\
 提供帮助堆栈展开的方法。 **仅在 .NET Native 上可用。**  
  
 [ICorPublish 接口](icorpublish-interface.md)\
 用作发布进程的常规接口。  
  
 [ICorPublishAppDomain 接口](icorpublishappdomain-interface.md)\
 表示并提供关于应用程序域的信息。  
  
 [ICorPublishAppDomainEnum 接口](icorpublishappdomainenum-interface.md)\
 提供遍历进程中当前存在的 `ICorPublishAppDomain` 对象的集合的方法。  
  
 [ICorPublishEnum 接口](icorpublishenum-interface.md)\
 用作发布枚举器的抽象基。  
  
 [ICorPublishProcess 接口](icorpublishprocess-interface.md)\
 提供用于访问有关进程的信息的方法。  
  
 [ICorPublishProcessEnum 接口](icorpublishprocessenum-interface.md)\
 提供遍历 `ICorPublishProcess` 对象的集合的方法。  

 [ISOSDacInterface 接口](isosdacinterface-interface.md)\
 提供用于访问中的数据的帮助器方法 `SOS` 。

 [IXCLRDataMethodDefinition 接口](ixclrdatamethoddefinition-interface.md)\
 提供用于查询有关方法定义的信息的方法。

 [IXCLRDataMethodInstance 接口](ixclrdatamethodinstance-interface.md)\
 提供用于查询有关方法实例的信息的方法。

 [IXCLRDataModule 接口](ixclrdatamodule-interface.md)\
 提供用于查询有关已加载模块的信息的方法。

 [IXCLRDataProcess 接口](ixclrdataprocess-interface.md)\
 提供用于查询有关进程的信息的方法。
  
## <a name="related-sections"></a>相关章节  

 [调试组件类](debugging-coclasses.md)\
 [调试全局静态函数](debugging-global-static-functions.md)\
 [调试枚举](debugging-enumerations.md)\
 [调试结构](debugging-structures.md)\
