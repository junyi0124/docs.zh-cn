---
title: SYMLINEDELTA 结构
ms.date: 03/30/2017
api_name:
- SYMLINEDELTA
api_location:
- diasymreader.dll
api_type:
- COM
f1_keywords:
- SYMLINEDELTA
helpviewer_keywords:
- SYMLINEDELTA structure [.NET Framework debugging]
ms.assetid: 9634e995-d46d-4397-ab66-cc5781d11e4e
topic_type:
- apiref
ms.openlocfilehash: dd45703540f8dc41b746ca03b4f09d74186aa9aa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95690936"
---
# <a name="symlinedelta-structure"></a>SYMLINEDELTA 结构

向符号处理程序提供有关因编辑而移动的方法的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _SYMLINEDELTA  
    {  
        mdMethodDef  mdMethod;  
        INT32        delta;  
    } SYMLINEDELTA;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`mdMethod`|方法的元数据标记。|  
|`delta`|此方法已移动的行数。|  
  
## <a name="requirements"></a>要求  

 **标头：** CorSym .idl  
  
## <a name="see-also"></a>另请参阅

- [诊断符号存储区结构](diagnostics-symbol-store-structures.md)
