---
title: CorBindToRuntimeHost 函数
ms.date: 03/30/2017
api_name:
- CorBindToRuntimeHost
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- CorBindToRuntimeHost
helpviewer_keywords:
- CorBindToRuntimeHost function [.NET Framework hosting]
ms.assetid: 5c826ba3-8258-49bc-a417-78807915fcaf
topic_type:
- apiref
ms.openlocfilehash: 7ba35823ccb670ad0201d1950687dc83cc9ba64a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673730"
---
# <a name="corbindtoruntimehost-function"></a>CorBindToRuntimeHost 函数

使宿主可以将指定版本的公共语言运行时 (CLR) 加载到进程中。  
  
 此函数已在 .NET Framework 4 中弃用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CorBindToRuntimeHost (  
    [in] LPCWSTR       pwszVersion,
    [in] LPCWSTR       pwszBuildFlavor,
    [in] LPCWSTR       pwszHostConfigFile,
    [in] VOID*         pReserved,
    [in] DWORD         startupFlags,
    [in] REFCLSID      rclsid,
    [in] REFIID        riid,
    [out] LPVOID FAR  *ppv  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwszVersion`  
 中一个字符串，描述要加载的 CLR 的版本。  
  
 .NET Framework 中的版本号由以句点分隔的四个部分组成： *主* 版本. 次要版本. 内部版本号. 修订号。 传递的字符串 `pwszVersion` 必须以字符 "v" 开头，后跟版本号的前三个部分 (例如，"1.0.1529" ) 。  
  
 某些版本的 CLR 随策略声明一起安装，后者指定与以前版本的 CLR 的兼容性。 默认情况下，启动填充程序将 `pwszVersion` 根据策略语句进行评估，并加载与请求的版本兼容的运行时的最新版本。 主机可以 `pwszVersion` 通过为参数传递值 STARTUP_LOADER_SAFEMODE 来强制填充程序跳过策略评估并加载中指定的确切版本 `startupFlags` 。  
  
 如果 `pwszVersion` 为，则 `null,` 此方法不加载任何版本的 CLR。 相反，它将返回 CLR_E_SHIM_RUNTIMELOAD，这表示它未能加载运行时。  
  
 `pwszBuildFlavor`  
 中一个字符串，指定是加载 CLR 的服务器还是工作站版本。 有效值为 `svr` 和 `wks`。 服务器版本经过优化，可利用多个处理器进行垃圾回收，工作站构建针对单处理器计算机上运行的客户端应用程序进行了优化。  
  
 如果 `pwszBuildFlavor` 设置为 null，则加载工作站生成。 在单处理器计算机上运行时，始终会加载工作站构建，即使将 `pwszBuildFlavor` 设置为 `svr` 。 但是，如果将 `pwszBuildFlavor` 设置为 `svr` 并指定了并发垃圾回收 (请参阅 `startupFlags`) 的参数说明，将加载服务器生成。  
  
> [!NOTE]
> 在64位系统上运行 WOW64 x86 模拟器的应用程序中不支持并发垃圾回收，该程序实现的 Intel Itanium 体系结构 (以前称为 IA-64) 。 有关在64位 Windows 系统上使用 WOW64 的详细信息，请参阅 [运行32位应用程序](/windows/desktop/WinProg64/running-32-bit-applications)。  
  
 `pwszHostConfigFile`  
 中主机配置文件的名称，该名称指定要加载的 CLR 的版本。 如果文件名不包含完全限定的路径，则假定该文件与进行调用的可执行文件位于同一目录中。  
  
 `pReserved`  
 中保留以供将来进行扩展。  
  
 `startupFlags`  
 中一组标志，这些标志控制并发垃圾回收、非特定于域的代码和参数的行为 `pwszVersion` 。 如果未设置任何标志，则默认值为单一域。 有关支持的值的列表，请参阅 [STARTUP_FLAGS 枚举](startup-flags-enumeration.md)。  
  
 `rclsid`  
 中 `CLSID` 用于实现 [ICorRuntimeHost](icorruntimehost-interface.md) 或 [ICLRRuntimeHost](iclrruntimehost-interface.md) 接口的 coclass 的。 支持的值为 CLSID_CorRuntimeHost 或 CLSID_CLRRuntimeHost。  
  
 `riid`  
 中 `IID` 你请求的接口的。 支持的值为 IID_ICorRuntimeHost 或 IID_ICLRRuntimeHost。  
  
 `ppv`  
 弄指向已加载的运行时版本的接口指针。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [CorBindToCurrentRuntime 函数](corbindtocurrentruntime-function.md)
- [CorBindToRuntime 函数](corbindtoruntime-function.md)
- [CorBindToRuntimeByCfg 函数](corbindtoruntimebycfg-function.md)
- [CorBindToRuntimeEx 函数](corbindtoruntimeex-function.md)
- [ICorRuntimeHost 接口](icorruntimehost-interface.md)
- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
