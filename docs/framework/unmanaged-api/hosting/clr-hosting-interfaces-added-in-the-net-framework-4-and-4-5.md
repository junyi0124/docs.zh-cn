---
title: .NET Framework 4 和 4.5 中添加的 CLR 承载接口
ms.date: 03/30/2017
helpviewer_keywords:
- hosting interfaces [.NET Framework], version 4
- .NET Framework 4, hosting interfaces
- interfaces [.NET Framework hosting], version 4
ms.assetid: f6af6116-f5b0-4bda-a276-fffdba70893d
ms.openlocfilehash: 6ee3b8cdf348a5eade3903e2d26b4f9b93886305
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706809"
---
# <a name="clr-hosting-interfaces-added-in-the-net-framework-4-and-45"></a>.NET Framework 4 和 4.5 中添加的 CLR 承载接口

本节介绍了非托管主机可用于将 .NET Framework 4、.NET Framework 4.5 和更高版本中的公共语言运行时 (CLR) 集成到其应用程序的接口。 这些接口为主机提供用于配置运行时并将其加载到进程中的方法。  
  
 从 .NET Framework 4 开始，所有宿主接口都具有以下特征：  
  
- 它们使用生存期管理 (`AddRef` 和 `Release`) ，将 (隐式) 上下文与 `QueryInterface` COM 进行封装。  
  
- 它们不使用 COM 类型 `BSTR` ，如、 `SAFEARRAY` 或 `VARIANT` 。  
  
- 没有使用 [CoCreateInstance 函数](/windows/win32/api/combaseapi/nf-combaseapi-cocreateinstance)的单元模型、聚合或注册表激活。  
  
## <a name="in-this-section"></a>本节内容  

 [ICLRAppDomainResourceMonitor 接口](iclrappdomainresourcemonitor-interface.md)  
 提供检查应用程序域的内存和 CPU 使用情况的方法。  
  
 [ICLRDomainManager 接口](iclrdomainmanager-interface.md)  
 使宿主能够指定将用于初始化默认应用程序域的应用程序域管理器，并指定初始化属性。  
  
 [ICLRGCManager2 接口](iclrgcmanager2-interface.md)  
 提供 [SetGCStartupLimitsEx](iclrgcmanager2-setgcstartuplimitsex-method.md) 方法，该方法允许主机将垃圾回收段的大小和垃圾回收系统的第0代的最大大小设置为大于的值 `DWORD` 。  
  
 [ICLRMetaHost 接口](iclrmetahost-interface.md)  
 提供一些方法，这些方法可返回 CLR 的特定版本，列出所有已安装的 Clr，列出所有进程内运行时，返回激活接口，并发现编译程序集所用的 CLR 版本。  
  
 [ICLRMetaHostPolicy 接口](iclrmetahostpolicy-interface.md)  
 提供 [GetRequestedRuntime](iclrmetahostpolicy-getrequestedruntime-method.md) 方法，该方法基于策略条件、托管程序集、版本和配置文件提供 CLR 接口。  
  
 [ICLRRuntimeInfo 接口](iclrruntimeinfo-interface.md)  
 提供返回有关特定运行时的信息的方法，包括版本、目录和加载状态。  
  
 [ICLRStrongName 接口](iclrstrongname-interface.md)  
 提供用于对具有强名称的程序集进行签名的基本全局静态函数。 所有 [ICLRStrongName](iclrstrongname-interface.md) 方法都返回标准 COM hresult。  
  
 [ICLRStrongName2 接口](iclrstrongname2-interface.md)  
 提供使用 SHA-256、SHA-384 和 SHA-512)  (sha-1 安全哈希算法组创建强名称的功能。  
  
 [ICLRTask2 接口](iclrtask2-interface.md)  
 提供 [ICLRTask 接口](iclrtask-interface.md)的所有功能;此外，还提供了允许线程中止在当前线程上延迟的方法。  
  
## <a name="related-sections"></a>相关章节  

 [弃用的 CLR 承载接口和 Coclass](deprecated-clr-hosting-interfaces-and-coclasses.md)  
 介绍 .NET Framework 版本1.0 和1.1 随附的托管接口。  
  
 [CLR 承载接口](clr-hosting-interfaces.md)  
 描述随 .NET Framework 版本2.0、3.0 和3.5 一起提供的托管接口。  
  
 [承载](index.md)  
 介绍 .NET Framework 中的托管。
