---
title: ModuleBindInfo 结构
ms.date: 03/30/2017
api_name:
- ModuleBindInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ModuleBindInfo
helpviewer_keywords:
- ModuleBindInfo structure [.NET Framework hosting]
ms.assetid: 632d4adc-dbc9-4ce8-9397-abc3285c1c69
topic_type:
- apiref
ms.openlocfilehash: 505015877985492edab4b761b379f33f1e5c6660
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729975"
---
# <a name="modulebindinfo-structure"></a>ModuleBindInfo 结构

提供有关被引用模块和包含它的程序集的详细信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _ModuleBindInfo {  
    DWORD    dwAppDomainId;  
    LPCWSTR  lpAssemblyIdentity;  
    LPCWSTR  lpModuleName  
} ModuleBindInfo;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`dwAppDomainId`|的唯一标识符，该标识符由要从中 `IStream` 加载所引用的模块的 [IHostAssemblyStore：:P rovidemodule](ihostassemblystore-providemodule-method.md) 方法的调用返回。|  
|`lpAssemblyIdentity`|包含所引用的模块的程序集的唯一标识符。|  
|`lpModuleName`|引用的模块的名称。|  
  
## <a name="remarks"></a>注解  

 `ModuleBindInfo` 作为参数传递给 `IHostAssemblyStore::ProvideModule` 。 宿主 `dwAppDomainId` (CLR) 向公共语言运行时提供唯一标识符。 调用 [IHostAssemblyStore：:P rovideassembly](ihostassemblystore-provideassembly-method.md) 方法返回后，运行时将使用标识符来确定是否已映射的内容 `IStream` 。 如果是这样，则运行时加载现有副本，而不是重新映射流。 运行时还会将此标识符用作从对方法的调用返回的流的查找密钥 `IHostAssemblyStore::ProvideAssembly` 。 因此，对于模块请求和程序集请求，标识符必须是唯一的。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载结构](hosting-structures.md)
- [AssemblyBindInfo 结构](assemblybindinfo-structure.md)
- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)
