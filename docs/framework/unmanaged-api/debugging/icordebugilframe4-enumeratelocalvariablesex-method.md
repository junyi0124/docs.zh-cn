---
title: ICorDebugILFrame4::EnumerateLocalVariablesEx 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorDebugILFrame4.EnumerateLocalVariablesEx
api_location:
- mscordbi.dll
api_type:
- COM
ms.assetid: 6f60aae6-70ec-4c4c-963a-138df98c4668
topic_type:
- apiref
ms.openlocfilehash: 86a3b22851aa07a546cba5a0c0b69c81ec580cee
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724970"
---
# <a name="icordebugilframe4enumeratelocalvariablesex-method"></a>ICorDebugILFrame4::EnumerateLocalVariablesEx 方法

[仅在 .NET Framework 4.5.2 及更高版本中受支持]  
  
 获取帧中局部变量的枚举，并且（可选）包括在探查器 ReJIT 检测中添加的变量。  
  
## <a name="syntax"></a>语法  
  
```cpp
HRESULT EnumerateLocalVariablesEx(  
   [in] ILCodeKind flags,
   [out] ICorDebugValueEnum **ppValueEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `flags`  
 中一个 [ILCodeKind](ilcodekind-enumeration.md) 枚举成员，用于指定在探查器 ReJIT 检测中添加的变量是否包含在帧中。  
  
 `ppValueEnum`  
 弄一个指向 "ICorDebugValueEnum" 对象地址的指针，该对象是此帧中局部变量的枚举器。  
  
## <a name="remarks"></a>注解  

 此方法类似于 [EnumerateLocalVariables](icordebugilframe-enumeratelocalvariables-method.md) 方法，不同之处在于它可以访问在探查器 ReJIT 检测中添加的变量。 将设置 `flags` 为 `ILCODE_ORIGINAL_IL` 等效于调用 [ICorDebugILFrame：： EnumerateLocalVariables](icordebugilframe-enumeratelocalvariables-method.md)。 将 `flags` 设置为 `ILCODE_REJIT_IL` 使调试器能够访问在探查器 ReJIT 检测中添加的局部变量。 如果未检测到中间语言 (IL)，则枚举为空且该方法将返回 `S_OK`。  
  
 由于某些局部变量可能未处于活动状态，因此枚举器可能不包括正在运行的方法中的所有局部变量。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugILFrame4 接口](icordebugilframe4-interface.md)
- [调试接口](debugging-interfaces.md)
- [ReJIT： How-To 指南](/archive/blogs/davbr/rejit-a-how-to-guide)
