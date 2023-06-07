---
title: ILCodeKind 枚举
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ILCodeKind
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: b91765e4-82db-46f9-a6dc-6b80610276af
topic_type:
- apiref
ms.openlocfilehash: 7e9cf760ec609786804a05177349ee2eacd79eaf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95692236"
---
# <a name="ilcodekind-enumeration"></a>ILCodeKind 枚举

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 提供用于指定调试器是否能够访问在探查器 ReJIT 检测中添加的局部变量或代码的值。  
  
## <a name="syntax"></a>语法  
  
```cpp
typedef enum ILCodeKind {  
   ILCODE_ORIGINAL_IL = 0x1,  
   ILCODE_REJIT_IL = 0x2,  
} ILCodeKind;  
```  
  
## <a name="members"></a>成员  
  
|成员名称|说明|  
|-----------------|-----------------|  
|`ILCODE_ORIGINAL_IL`|调试器无法访问 ReJIT 检测的信息。|  
|`ILCODE_REJIT_IL`|调试器可以访问 ReJIT 检测的信息。|  
  
## <a name="remarks"></a>注解  

 枚举的成员 `ILCodeKind` 可以传递给 [EnumerateLocalVariablesEx](icordebugilframe4-enumeratelocalvariablesex-method.md) 和 [GetLocalVariableEx](icordebugilframe4-getlocalvariableex-method.md) 方法，以确定调试器是否可以访问在探查器 ReJIT 检测中添加的变量，以及如何访问 [GetCodeEx](icordebugilframe4-getcodeex-method.md) 方法，以确定调试器是否可以访问检测的 IL。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试枚举](debugging-enumerations.md)
- [ICorDebugILFrame4 接口](icordebugilframe4-interface.md)
- [ReJIT： How-To 指南](/archive/blogs/davbr/rejit-a-how-to-guide)
