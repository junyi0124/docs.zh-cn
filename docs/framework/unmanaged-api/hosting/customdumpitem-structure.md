---
title: CustomDumpItem 结构
ms.date: 03/30/2017
api_name:
- CustomDumpItem
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CustomDumpItem
helpviewer_keywords:
- CustomDumpItem structure [.NET Framework hosting]
ms.assetid: fd9085ff-7beb-4c38-97f0-037cd8ba4f65
topic_type:
- apiref
ms.openlocfilehash: c77e93686c7d121e9fe2a92f03970404ab823dc0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673236"
---
# <a name="customdumpitem-structure"></a>CustomDumpItem 结构

描述要添加到错误报告中的自定义转储的项。  
  
## <a name="syntax"></a>语法  
  
```cpp  
struct {  
    ECustomDumpItemKind itemKind;
    union {  
        UINT_PTR pReserved;  
    }  
} CustomDumpItem;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`itemKind`|一个 [ECustomDumpItemKind](ecustomdumpitemkind-enumeration.md) 值，指示要添加的项的类型。|  
|`pReserved`|当前未使用。 添加到联合的任何项都必须不大于指针大小。 如果 `struct` 需要，您必须单独分配并指向它。|  
  
## <a name="remarks"></a>注解  

 [ICLRErrorReportingManager：： BeginCustomDump](iclrerrorreportingmanager-begincustomdump-method.md) 采用类型为的参数 `CustomDumpItem` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [承载结构](hosting-structures.md)
