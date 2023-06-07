---
title: 弃用的 CLR 承载函数
ms.date: 03/30/2017
helpviewer_keywords:
- .NET Framework 1.1, hosting global static functions
- global static functions [.NET Framework hosting], version 2.0
- .NET Framework 2.0, hosting global static functions
- hosting global static functions [.NET Framework], version 2.0
ms.assetid: 91fbbb35-e543-4814-b806-371cebae8c5a
ms.openlocfilehash: 9e19502672973f292991b72c7ea9b4fdc17f5707
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673119"
---
# <a name="deprecated-clr-hosting-functions"></a>弃用的 CLR 承载函数

本部分介绍了早期版本的托管 API 使用的非托管全局静态函数。  
  
 除了基础结构函数 (`_Cor*` 函数) （仅由 .NET Framework 使用），这些函数在 .NET Framework 4 中已弃用。  
  
## <a name="activation-functions"></a>激活函数  

 [ClrCreateManagedInstance 函数](clrcreatemanagedinstance-function.md)  
 已弃用。 创建指定托管类型的实例。  
  
 [CoInitializeCor 函数](coinitializecor-function.md)  
 已过时。 若要 (CLR) 初始化公共语言运行时，请使用 [CorBindToRuntimeEx](corbindtoruntimeex-function.md) 或 [CorBindToCurrentRuntime](corbindtocurrentruntime-function.md)。  
  
 [CoInitializeEE 函数](coinitializeee-function.md)  
 已弃用。 确保 CLR 执行引擎已加载到进程中。 改为使用 [ICLRRuntimeHost：： Start](iclrruntimehost-start-method.md) 方法。  
  
 [CorBindToCurrentRuntime 函数](corbindtocurrentruntime-function.md)  
 已弃用。 通过使用存储在 XML 文件中的版本信息，将 (CLR) 中的公共语言运行时加载到进程中。  
  
 [CorBindToRuntime 函数](corbindtoruntime-function.md)  
 已弃用。 使非托管宿主能够将 CLR 加载到进程中。  
  
 [CorBindToRuntimeByCfg 函数](corbindtoruntimebycfg-function.md)  
 已弃用。 使用从 XML 文件读取的版本信息将 CLR 加载到进程中。  
  
 [CorBindToRuntimeEx 函数](corbindtoruntimeex-function.md)  
 已弃用。 使非托管宿主能够将 CLR 加载到进程中，并允许您设置标志来指定 CLR 的行为。  
  
 [CorBindToRuntimeHost 函数](corbindtoruntimehost-function.md)  
 已弃用。 使宿主可以将指定版本的 CLR 加载到进程中。  
  
 [GetCORRequiredVersion 函数](getcorrequiredversion-function.md)  
 已弃用。 获取所需的 CLR 版本号。  
  
 [GetCORSystemDirectory 函数](getcorsystemdirectory-function.md)  
 已弃用。 返回加载到进程中的 CLR 的安装目录。  
  
 [GetRealProcAddress 函数](getrealprocaddress-function.md)  
 已弃用。 获取从安装的最新版本的 CLR 导出的指定函数的地址。  
  
 [GetRequestedRuntimeInfo 函数](getrequestedruntimeinfo-function.md)  
 已弃用。 获取有关应用程序请求的 CLR 的版本和目录信息。  
  
## <a name="clr-version-functions"></a>CLR 版本函数  

 本节中的函数返回 CLR 版本;它们不激活 CLR。  
  
 [GetCORVersion 函数](getcorversion-function.md)  
 已弃用。 返回当前进程中正在运行的 CLR 的版本号。  
  
 [GetFileVersion 函数](getfileversion-function.md)  
 已弃用。 使用指定的缓冲区获取指定文件的 CLR 版本信息。  
  
 [GetRequestedRuntimeVersion 函数](getrequestedruntimeversion-function.md)  
 已弃用。 获取指定应用程序请求的 CLR 的版本号。 如果未安装该版本，则获取在请求版本之前安装的最新版本。  
  
 [GetRequestedRuntimeVersionForCLSID 函数](getrequestedruntimeversionforclsid-function.md)  
 已弃用。 获取具有指定 CLSID 的类的相应 CLR 版本信息。  
  
 [GetVersionFromProcess 函数](getversionfromprocess-function.md)  
 已弃用。 获取与指定的进程句柄关联的 CLR 的版本号。  
  
 [LockClrVersion 函数](lockclrversion-function.md)  
 已弃用。 允许主机在显式初始化 CLR 之前确定将在进程内使用的 CLR 版本。  
  
## <a name="hosting-functions"></a>承载函数  

 [CallFunctionShim 函数](callfunctionshim-function.md)  
 已弃用。 对指定库中具有指定名称和参数的函数进行调用。  
  
 [CoEEShutDownCOM 函数](coeeshutdowncom-function.md)  
 已弃用。 从进程中卸载 COM 程序集。  
  
 [CorExitProcess 函数](corexitprocess-function.md)  
 已弃用。 关闭当前的非托管进程。  
  
 [CorLaunchApplication 函数](corlaunchapplication-function.md)  
 已弃用。 使用指定的清单和其他应用程序数据，在指定的网络路径下启动应用程序。  
  
 [CorMarkThreadInThreadPool 函数](cormarkthreadinthreadpool-function.md)  
 已弃用。 标记当前正在执行的线程池线程以执行托管代码。 从 .NET Framework 版本2.0 开始，此函数不起作用。 这不是必需的，并且可以从代码中删除。  
  
 [CoUninitializeCor 函数](couninitializecor-function.md)  
 已过时。 CLR 无法从进程中卸载。  
  
 [CoUninitializeEE 函数](couninitializeee-function.md)  
 已过时。  
  
 [CreateDebuggingInterfaceFromVersion 函数](createdebugginginterfacefromversion-function.md)  
 已弃用。 基于指定的版本信息创建 [ICorDebug](../debugging/icordebug-interface.md) 对象。  
  
 [CreateICeeFileGen 函数](createiceefilegen-function.md)  
 已弃用。 创建 [ICeeFileGen](iceefilegen-class.md) 对象。  
  
 [DestroyICeeFileGen 函数](destroyiceefilegen-function.md)  
 已弃用。 销毁 [ICeeFileGen](iceefilegen-class.md) 对象。  
  
 [FExecuteInAppDomainCallback 函数指针](fexecuteinappdomaincallback-function-pointer.md)  
 已弃用。 指向 CLR 调用以执行托管代码的函数。  
  
 [FLockClrVersionCallback 函数指针](flockclrversioncallback-function-pointer.md)  
 已弃用。 指向一个函数，CLR 将调用该函数以通知宿主初始化已启动或已完成。  
  
 [GetCLRIdentityManager 函数](getclridentitymanager-function.md)  
 已弃用。 获取一个指向接口的指针，该接口允许 CLR 管理标识。  
  
 [LoadLibraryShim 函数](loadlibraryshim-function.md)  
 已弃用。 加载 .NET Framework DLL 的指定版本。  
  
 [LoadStringRC 函数](loadstringrc-function.md)  
 已弃用。 使用当前线程的默认区域性将 HRESULT 值转换为错误消息。  
  
 [LoadStringRCEx 函数](loadstringrcex-function.md)  
 已弃用。 将 HRESULT 值转换为指定区域性的适当错误消息。  
  
 [LPOVERLAPPED_COMPLETION_ROUTINE 函数指针](lpoverlapped-completion-routine-function-pointer.md)  
 已弃用。 指向一个函数，该函数在重叠 (即设备的异步) i/o 完成时通知宿主。  
  
 [LPTHREAD_START_ROUTINE 函数指针](lpthread-start-routine-function-pointer.md)  
 已弃用。 指向一个函数，该函数通知宿主线程已开始执行。  
  
 [RunDll32ShimW 函数](rundll32shimw-function.md)  
 已弃用。 执行指定的命令。  
  
 [WAITORTIMERCALLBACK 函数指针](waitortimercallback-function-pointer.md)  
 已弃用。 指向一个函数，该函数通知宿主等待句柄已终止或超时。  
  
## <a name="infrastructure-functions"></a>基础结构函数  

 本节中的函数仅供 .NET Framework 使用。  
  
 [_CorDllMain 函数](cordllmain-function.md)  
 初始化 CLR，查找 DLL 程序集的 CLR 头中的托管入口点，然后开始执行。  
  
 [_CorExeMain 函数](corexemain-function.md)  
 初始化 CLR，查找可执行程序集的 CLR 头中的托管入口点，然后开始执行。  
  
 [_CorExeMain2 函数](corexemain2-function.md)  
 执行指定的内存映射代码中的入口点。 此函数由操作系统加载程序调用。  
  
 [_CorImageUnloading 函数](corimageunloading-function.md)  
 卸载托管模块映像时通知加载程序。  
  
 [_CorValidateImage 函数](corvalidateimage-function.md)  
 验证托管模块映像，并在加载后通知操作系统加载程序。  
  
## <a name="see-also"></a>另请参阅

- [.NET Framework 4 承载全局静态函数](net-framework-4-hosting-global-static-functions.md)
