---
title: CLR_DEBUGGING_VERSION 结构
ms.date: 03/30/2017
api_name:
- CLR_DEBUGGING_VERSION
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- CLR_DEBUGGING_VERSION
helpviewer_keywords:
- CLR_DEBUGGING_VERSION structure [.NET Framework debugging]
ms.assetid: 4d821186-3ddf-405a-ac44-d79438a9f7f3
topic_type:
- apiref
ms.openlocfilehash: 8a2abe847728a2bb1f1345ef73e55b58e4704001
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729806"
---
# <a name="clr_debugging_version-structure"></a>CLR_DEBUGGING_VERSION 结构

出于调试目的，定义公共语言运行时 (CLR) 的产品版本。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _CLR_DEBUGGING_VERSION  
{  
    WORD wStructVersion;
    WORD wMajor;
    WORD wMinor;
    WORD wBuild;
    WORD wRevision;
} CLR_DEBUGGING_VERSION;
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`wStructVersion`|结构版本号|  
|`wMajor`|主版本号。|  
|`wMinor`|次版本号。|  
|`wBuild`|内部版本号。|  
|`wRevision`|修订号。|  
  
## <a name="remarks"></a>注解  

 `CLR_DEBUGGING_VERSION`结构与 COR_VERSION 结构相同，但 `CLR_DEBUGGING_VERSION` 结构提供了 () 的附加结构版本字段 `wStructVersion` 。 此字段当前必须设置为零。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cordebug.idl .idl  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试结构](debugging-structures.md)
- [调试](index.md)
