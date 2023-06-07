---
title: ICorDebugGuidToTypeEnum::Next 方法
ms.date: 03/30/2017
api_name:
- ICorDebugGuidToTypeEnum.Next
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugGuidToTypeEnum::Next
helpviewer_keywords:
- ICorDebugGuidToTypeEnum::Next method [.NET Framework debugging]
- Next method, ICorDebugGuidToTypeEnum interface [.NET Framework debugging]
ms.assetid: c9937666-8e18-484d-9fe0-b9ac95199530
topic_type:
- apiref
ms.openlocfilehash: 68f548705213da7d715ae569116abae0cd24129d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95705652"
---
# <a name="icordebugguidtotypeenumnext-method"></a>ICorDebugGuidToTypeEnum::Next 方法

获取指定数量的 [CorDebugGuidToTypeMapping](cordebugguidtotypemapping-structure.md) 实例，这些实例将 guid 映射到类型信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Next(  
    [in] ULONG celt,  
    [out, size_is(celt), length_is(*pceltFetched] CorDebugGuidToTypeMapping values[  ],  
    [out] ULONG *pceltFetched  
);  
```  
  
## <a name="parameters"></a>参数  

 `celt`  
 中要检索的 GUID 到类型的映射对象的数量。  
  
 `values`  
 弄指针的数组，其中每个都指向将 Windows 运行时 GUID 映射到其对应 ICorDebugType 对象的 [CorDebugGuidToTypeMapping](cordebugguidtotypemapping-structure.md) 对象。  
  
 `pceltFetched`  
 弄一个指针，指向在中实际返回的 [CorDebugGuidToTypeMapping](cordebugguidtotypemapping-structure.md) 对象的数目 `values` 。  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** Windows 运行时  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugGuidToTypeEnum 接口](icordebugguidtotypeenum-interface.md)
- [调试接口](debugging-interfaces.md)
