---
title: CorMethodSemanticsAttr 枚举
ms.date: 03/30/2017
api_name:
- CorMethodSemanticsAttr
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorMethodSemanticsAttr
helpviewer_keywords:
- CorMethodSemanticsAttr enumeration [.NET Framework metadata]
ms.assetid: ca2af325-eb9d-4a91-90e4-267e45b98611
topic_type:
- apiref
ms.openlocfilehash: 347b323951b0125ffa5f82626b2d9b235079492c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676941"
---
# <a name="cormethodsemanticsattr-enumeration"></a>CorMethodSemanticsAttr 枚举

包含一些值，用于描述方法和关联属性或事件之间的关系。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum CorMethodSemanticsAttr {  
  
    msSetter    =   0x0001,  
    msGetter    =   0x0002,  
    msOther     =   0x0004,  
    msAddOn     =   0x0008,  
    msRemoveOn  =   0x0010,  
    msFire      =   0x0020,  
  
} CorMethodSemanticsAttr;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`msSetter`|指定该方法是属性的 `set` 访问器。|  
|`msGetter`|指定该方法是属性的 `get` 访问器。|  
|`msOther`|指定该方法与在此处定义的属性或事件无关。|  
|`msAddOn`|指定方法为事件添加处理程序方法。|  
|`msRemoveOn`|指定该方法移除事件的处理程序方法。|  
|`msFire`|指定方法引发事件。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
