---
title: AssemblyBindInfo 结构
ms.date: 03/30/2017
api_name:
- AssemblyBindInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- AssemblyBindInfo
helpviewer_keywords:
- AssemblyBindInfo structure [.NET Framework hosting]
ms.assetid: 6fc01e98-c2e7-49de-ab9f-95937cc89017
topic_type:
- apiref
ms.openlocfilehash: d2ba7d8e66472f771a932a2dfb05bb9e1ee96290
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685872"
---
# <a name="assemblybindinfo-structure"></a>AssemblyBindInfo 结构

提供有关所引用程序集的详细信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _AssemblyBindInfo {  
    DWORD       dwAppDomainId;  
    LPCWSTR     lpReferencedIdentity;  
    LPCWSTR     lpPostPolicyIdentity;  
    DWORD       ePolicyLevel;  
} AssemblyBindInfo;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`dwAppDomainId`|`IStream`通过调用[IHostAssemblyStore：:P rovideassembly](ihostassemblystore-provideassembly-method.md)返回的的唯一标识符，将从中加载所引用的程序集。|  
|`lpReferencedIdentity`|所引用程序集的唯一标识符。|  
|`lpPostPolicyIdentity`|在应用任何绑定策略值之后引用的程序集的标识符。|  
|`ePolicyLevel`|[EPolicyAction](epolicyaction-enumeration.md)值之一，指示应将哪些版本控制策略应用于所引用的程序集。|  
  
## <a name="remarks"></a>注解  

 宿主 `dwAppDomainId` (CLR) 向公共语言运行时提供唯一标识符。 在对的调用 `IHostAssemblyStore::ProvideAssembly` 返回后，运行时将使用标识符来确定是否已映射的内容 `IStream` 。 如果是这样，则运行时加载现有副本，而不是重新映射流。 运行时还会将此标识符用作从对 [IHostAssemblyStore：:P rovidemodule](ihostassemblystore-providemodule-method.md)的调用返回的流的查找密钥。 因此，对于模块请求和程序集请求，标识符必须是唯一的。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载结构](hosting-structures.md)
- [ICLRAssemblyIdentityManager 接口](iclrassemblyidentitymanager-interface.md)
- [ICLRAssemblyReferenceList 接口](iclrassemblyreferencelist-interface.md)
- [IHostAssemblyManager 接口](ihostassemblymanager-interface.md)
- [IHostAssemblyStore 接口](ihostassemblystore-interface.md)
- [ModuleBindInfo 结构](modulebindinfo-structure.md)
