---
title: CorFileMapping 枚举
ms.date: 03/30/2017
api_name:
- CorFileMapping
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorFileMapping
helpviewer_keywords:
- CorFileMapping enumeration [.NET Framework metadata]
ms.assetid: 3ca41592-b8da-475a-8032-a15627730003
topic_type:
- apiref
ms.openlocfilehash: 63e27a62e176a92b03c10b59a55d9da3192918f4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726110"
---
# <a name="corfilemapping-enumeration"></a>CorFileMapping 枚举

包含一些值，这些值描述从对 [IMetaDataInfo：： GetFileMapping](imetadatainfo-getfilemapping-method.md) 方法的调用返回的文件映射的类型。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorFileMapping {  
  
    fmFlat                  =   0x0000,  
    fmExecutableImage       =   0x0001  
  
} CorFileMapping;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`fmFlat`|文件映射为数据文件。 也就是说， `SEC_IMAGE` 标志未传递到 Microsoft Win32 `CreateFileMapping` 函数。|  
|`fmExecutableImage`|使用 `LoadLibrary` 函数或带有标志的函数为执行映射文件 `CreateFileMapping` `SEC_IMAGE` 。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
- [GetFileMapping 方法](imetadatainfo-getfilemapping-method.md)
