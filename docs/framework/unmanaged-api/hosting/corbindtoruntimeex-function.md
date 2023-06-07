---
title: CorBindToRuntimeEx 函数
ms.date: 03/30/2017
api_name:
- CorBindToRuntimeEx
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- CorBindToRuntimeEx
helpviewer_keywords:
- CorBindToRuntimeEx function [.NET Framework hosting]
ms.assetid: aae9fb17-5d01-41da-9773-1b5b5b642d81
topic_type:
- apiref
ms.openlocfilehash: 55fbf0c37861029940422a10bd62f5ecfebf2b9a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673729"
---
# <a name="corbindtoruntimeex-function"></a>CorBindToRuntimeEx 函数

使非托管宿主能够将公共语言运行时 (CLR) 加载到进程中。 [CorBindToRuntime](corbindtoruntime-function.md)和 `CorBindToRuntimeEx` 函数执行相同的操作，但 `CorBindToRuntimeEx` 函数允许您设置标志来指定 CLR 的行为。  
  
 此函数已在 .NET Framework 4 中弃用。  
  
 此函数使用一组参数，这些参数允许主机执行以下操作：  
  
- 指定要加载的运行时版本。  
  
- 指示是否应加载服务器或工作站生成。  
  
- 控制并发垃圾回收或非并发垃圾回收是否已完成。  
  
> [!NOTE]
> 在64位系统上运行 WOW64 x86 模拟器的应用程序中不支持并发垃圾回收，该程序实现的 Intel Itanium 体系结构 (以前称为 IA-64) 。 有关在64位 Windows 系统上使用 WOW64 的详细信息，请参阅 [运行32位应用程序](/windows/desktop/WinProg64/running-32-bit-applications)。  
  
- 控制是否以非特定于域的形式加载程序集。  
  
- 获取一个指向 [ICorRuntimeHost](icorruntimehost-interface.md) 的接口指针，该指针可用于设置附加选项，以便在启动 CLR 实例之前对其进行配置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CorBindToRuntimeEx (  
    [in]  LPCWSTR      pwszVersion,
    [in]  LPCWSTR      pwszBuildFlavor,
    [in]  DWORD        startupFlags,
    [in]  REFCLSID     rclsid,
    [in]  REFIID       riid,
    [out] LPVOID FAR  *ppv  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwszVersion`  
 中一个字符串，描述要加载的 CLR 的版本。  
  
 .NET Framework 中的版本号由以句点分隔的四个部分组成： *主* 版本. 次要版本. 内部版本号. 修订号。 传递的字符串 `pwszVersion` 必须以字符 "v" 开头，后跟版本号的前三个部分 (例如，"1.0.1529" ) 。  
  
 某些版本的 CLR 随策略声明一起安装，后者指定与以前版本的 CLR 的兼容性。 默认情况下，启动填充程序将 `pwszVersion` 根据策略语句进行评估，并加载与请求的版本兼容的运行时的最新版本。 主机可以通过为参数传递值来强制填充程序跳过策略评估并加载中指定的确切版本 `pwszVersion`  `STARTUP_LOADER_SAFEMODE` `startupFlags` ，如下所述。  
  
 如果调用方为指定 null `pwszVersion` ，则 `CorBindToRuntimeEx` 标识已安装的运行时集，这些运行时的版本号低于 .NET Framework 4 运行时，并从该集中加载最新版本的运行时。 它不会加载 .NET Framework 4 或更高版本，如果未安装任何早期版本，则会失败。 请注意，传递 null 会使宿主无法控制加载的运行时版本。 虽然这种方法可能适用于某些情况，但强烈建议主机提供要加载的特定版本。  
  
 `pwszBuildFlavor`  
 中一个字符串，指定是加载 CLR 的服务器还是工作站版本。 有效值为 `svr` 和 `wks`。 服务器版本经过优化，可利用多个处理器进行垃圾回收，工作站构建针对单处理器计算机上运行的客户端应用程序进行了优化。  
  
 如果 `pwszBuildFlavor` 设置为 null，则加载工作站生成。 在单处理器计算机上运行时，始终会加载工作站构建，即使将 `pwszBuildFlavor` 设置为 `svr` 。 但是，如果将 `pwszBuildFlavor` 设置为 `svr` 并指定了并发垃圾回收 (请参阅 `startupFlags`) 的参数说明，将加载服务器生成。  
  
 `startupFlags`  
 中 [STARTUP_FLAGS](startup-flags-enumeration.md) 枚举的值的组合。 这些标志控制并发垃圾回收、非特定于域的代码和参数的行为 `pwszVersion` 。 如果未设置任何标志，则默认值为单一域。 以下为有效值：  
  
- `STARTUP_CONCURRENT_GC`  
  
- `STARTUP_LOADER_OPTIMIZATION_SINGLE_DOMAIN`  
  
- `STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN`  
  
- `STARTUP_LOADER_OPTIMIZATION_MULTI_DOMAIN_HOST`  
  
- `STARTUP_LOADER_SAFEMODE`  
  
- `STARTUP_LEGACY_IMPERSONATION`  
  
- `STARTUP_LOADER_SETPREFERENCE`  
  
- `STARTUP_SERVER_GC`  
  
- `STARTUP_HOARD_GC_VM`  
  
- `STARTUP_SINGLE_VERSION_HOSTING_INTERFACE`  
  
- `STARTUP_LEGACY_IMPERSONATION`  
  
- `STARTUP_DISABLE_COMMITTHREADSTACK`  
  
- `STARTUP_ALWAYSFLOW_IMPERSONATION`  
  
 有关这些标志的说明，请参阅 [STARTUP_FLAGS](startup-flags-enumeration.md) 枚举。  
  
 `rclsid`  
 中 `CLSID` 用于实现 [ICorRuntimeHost](icorruntimehost-interface.md) 或 [ICLRRuntimeHost](iclrruntimehost-interface.md) 接口的 coclass 的。 支持的值为 CLSID_CorRuntimeHost 或 CLSID_CLRRuntimeHost。  
  
 `riid`  
 中所 `IID` 请求的接口的 `rclsid` 。 支持的值为 IID_ICorRuntimeHost 或 IID_ICLRRuntimeHost。  
  
 `ppv`  
 弄返回的接口指针 `riid` 。  
  
## <a name="remarks"></a>注解  

 如果 `pwszVersion` 指定不存在的运行时版本，则 `CorBindToRuntimeEx` 返回的 HRESULT 值为 CLR_E_SHIM_RUNTIMELOAD。  
  
## <a name="execution-context-and-flow-of-windows-identity"></a>执行上下文和 Windows 标识流  

 在 CLR 版本1中， <xref:System.Security.Principal.WindowsIdentity> 对象不流经异步点，如新线程、线程池或计时器回调。 在 CLR 版本2.0 中， <xref:System.Threading.ExecutionContext> 对象包装了有关当前正在执行的线程的某些信息，并在所有异步点（而不是跨应用程序域边界）对其进行流传递。 同样， <xref:System.Security.Principal.WindowsIdentity> 对象还在任何异步点上流动。 因此，线程上的当前模拟（如果有）也会流动。  
  
 可以通过两种方式更改流：  
  
1. 通过修改 <xref:System.Threading.ExecutionContext> 设置以在每个线程的基础上取消流 (参见 <xref:System.Threading.ExecutionContext.SuppressFlow%2A> 、 <xref:System.Security.SecurityContext.SuppressFlow%2A> 和 <xref:System.Security.SecurityContext.SuppressFlowWindowsIdentity%2A> 方法) 。  
  
2. 通过将进程默认模式更改为版本1兼容模式，该模式中的 <xref:System.Security.Principal.WindowsIdentity> 对象不会在任何异步点上流动，无论 <xref:System.Threading.ExecutionContext> 当前线程上的设置如何。 更改默认模式的方式取决于您使用的是托管可执行文件还是非托管承载接口来加载 CLR：  
  
    1. 对于托管可执行文件，必须将 `enabled` 元素的属性设置 [\<legacyImpersonationPolicy>](../../configure-apps/file-schema/runtime/legacyimpersonationpolicy-element.md) 为 `true` 。  
  
    2. 对于非托管承载接口，请在 `STARTUP_LEGACY_IMPERSONATION` `startupFlags` 调用函数时在参数中设置标志 `CorBindToRuntimeEx` 。  
  
     版本1兼容性模式适用于整个进程和进程中的所有应用程序域。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CorBindToCurrentRuntime 函数](corbindtocurrentruntime-function.md)
- [CorBindToRuntime 函数](corbindtoruntime-function.md)
- [CorBindToRuntimeByCfg 函数](corbindtoruntimebycfg-function.md)
- [CorBindToRuntimeHost 函数](corbindtoruntimehost-function.md)
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
