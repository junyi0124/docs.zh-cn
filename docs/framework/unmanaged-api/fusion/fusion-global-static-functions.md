---
title: 合成全局静态函数
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged global static functions [.NET Framework], fusion
- fusion global static functions [.NET Framework]
- global static functions [.NET Framework fusion]
ms.assetid: 229b2188-9168-4b44-a987-e1f515494688
ms.openlocfilehash: 691fd50e05cadd3f82196fc6ba5df9bb84a66faa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95688739"
---
# <a name="fusion-global-static-functions"></a>合成全局静态函数

本部分介绍了合成 API 使用的非托管全局静态函数。  
  
## <a name="in-this-section"></a>本节内容  

 [ClearDownloadCache 函数](cleardownloadcache-function.md)  
 清除已下载程序集的全局程序集缓存。  
  
 [CompareAssemblyIdentity 函数](compareassemblyidentity-function.md)  
 比较两个程序集标识以确定它们是否等效。  
  
 [CreateApplicationContext 函数](createapplicationcontext-function.md)  
 仅限内部。  (此函数支持 .NET Framework 基础结构，不应在代码中直接使用。 )   
  
 [CreateAssemblyCache 函数](createassemblycache-function.md)  
 获取一个指针，该指针指向表示全局程序集缓存的新 [IAssemblyCache](iassemblycache-interface.md) 实例。  
  
 [CreateAssemblyEnum 函数](createassemblyenum-function.md)  
 获取一个指针，该指针指向表示存在于指定程序集中的对象的列表的 [IAssemblyEnum](iassemblyenum-interface.md) 实例。  
  
 [CreateAssemblyNameObject 函数](createassemblynameobject-function.md)  
 获取一个指针，该指针指向表示具有指定名称的程序集的唯一标识的 [IAssemblyName](iassemblyname-interface.md) 实例。  
  
 [CreateHistoryReader 函数](createhistoryreader-function.md)  
 为指定的文件创建历史记录读取器。  
  
 [CreateInstallReferenceEnum 函数](createinstallreferenceenum-function.md)  
 获取一个指针，该指针指向表示应用程序对指定程序集的引用列表的 [IInstallReferenceEnum](iinstallreferenceenum-interface.md) 实例。  
  
 [GetAppIdAuthority 函数](getappidauthority-function.md)  
 获取一个指针，该指针指向管理应用程序标识和引用的密钥的 [IAppIdAuthority](iappidauthority-interface.md) 实例。  
  
 [GetAssemblyIdentityFromFile 函数](getassemblyidentityfromfile-function.md)  
 获取一个指针，该指针指向在 `IUnknown` `IID` 指定文件路径的程序集中具有指定的对象。  
  
 [GetCachePath 函数](getcachepath-function.md)  
 使用指定的标志获取缓存的程序集的路径。  
  
 [GetHistoryFileDirectory 函数](gethistoryfiledirectory-function.md)  
 检索应用程序历史记录目录的路径。  
  
 [GetIdentityAuthority 函数](getidentityauthority-function.md)  
 获取一个指针，该指针指向管理代码对象的键的 [IIdentityAuthority](iidentityauthority-interface.md) 实例。  
  
 [IsFrameworkAssembly 函数](isframeworkassembly-function.md)  
 获取一个值，该值指示是否托管指定的程序集。  
  
 [NukeDownloadedCache 函数](nukedownloadedcache-function.md)  
 删除公共语言运行时下载缓存。  
  
 [PreBindAssemblyEx 函数](prebindassemblyex-function.md)  
 获取程序集的策略后显示名称。  
  
## <a name="related-sections"></a>相关章节  

 [合成接口](fusion-interfaces.md)  
  
 [合成枚举](fusion-enumerations.md)  
  
 [合成结构](fusion-structures.md)  
  
 [全局程序集缓存](../../app-domains/gac.md)
