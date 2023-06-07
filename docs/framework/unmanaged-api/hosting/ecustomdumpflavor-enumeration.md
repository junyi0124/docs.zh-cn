---
title: ECustomDumpFlavor 枚举
ms.date: 03/30/2017
api_name:
- ECustomDumpFlavor
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ECustomDumpFlavor
helpviewer_keywords:
- ECustomDumpFlavor enumeration [.NET Framework hosting]
ms.assetid: b39b3320-fac7-41f1-9a03-ab6fb0cd89c7
topic_type:
- apiref
ms.openlocfilehash: 1b8440ed6e878aac3dd08d9f8ed528c93739a724
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686314"
---
# <a name="ecustomdumpflavor-enumeration"></a>ECustomDumpFlavor 枚举

包含一些值，这些值指示在报告错误时要包含在堆转储的自定义子集中的项。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum {  
    DUMP_FLAVOR_Mini            = 1,  
    DUMP_FLAVOR_NonHeapCLRState = 2  
} ECustomDumpFlavor;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`DUMP_FLAVOR_Mini`|指定自定义堆转储应作为小型转储开始，并包含传递给同一方法的任何 [CustomDumpItem](customdumpitem-structure.md) 实例指定的额外数据。|  
|`DUMP_FLAVOR_NonHeapCLRState`|指定自定义堆转储应收集未动态分配的所有运行时状态数据。|  
  
## <a name="remarks"></a>注解  

 类型的参数 `ECustomDumpFlavor` 传递给 [ICLRErrorReportingManager：： BeginCustomDump](iclrerrorreportingmanager-begincustomdump-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ECustomDumpItemKind 枚举](ecustomdumpitemkind-enumeration.md)
- [ICLRErrorReportingManager 接口](iclrerrorreportingmanager-interface.md)
- [承载枚举](hosting-enumerations.md)
