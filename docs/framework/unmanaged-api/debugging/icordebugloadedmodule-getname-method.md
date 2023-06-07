---
title: ICorDebugLoadedModule::GetName 方法
ms.date: 03/30/2017
ms.assetid: 88c304d5-edaa-4c0e-a8e1-144e8a76877e
ms.openlocfilehash: c18af45184f5a9485e13b9d4789bff2c570834cc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731847"
---
# <a name="icordebugloadedmodulegetname-method"></a>ICorDebugLoadedModule::GetName 方法

获取加载模块的名称。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetName(  
   [in] ULONG32 cchName,  
   [out] ULONG32 *pcchName,  
   [out, size_is(cchName),  
   length_is(*pcchName)] WCHAR szName[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `cchName`  
 [in] `szName` 缓冲区中的字符数。  
  
 `pcchName`  
 [out] 指向实际写入 `szName` 缓冲区的字符数。缓冲区的字符数的指针。  
  
 `szName`  
 [out] 包含加载模块名称的字符数组。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugLoadedModule 接口](icordebugloadedmodule-interface.md)
- [调试接口](debugging-interfaces.md)
