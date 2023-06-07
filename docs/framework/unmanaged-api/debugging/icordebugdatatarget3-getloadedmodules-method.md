---
title: ICorDebugDataTarget3::GetLoadedModules 方法
ms.date: 03/30/2017
ms.assetid: 9a48c05b-1949-416e-933c-52549b6fcf5e
ms.openlocfilehash: efbada02b7a24e0a7ed613b86b8a4a1a0b5b051a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713739"
---
# <a name="icordebugdatatarget3getloadedmodules-method"></a>ICorDebugDataTarget3::GetLoadedModules 方法

获取到目前为止已加载的模块的列表。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetLoadedModules(  
   [in] ULONG32 cRequestedModules,  
   [out] ULONG32 *pcFetchedModules,  
   [out, size_is(cRequestedModules), length_is(*pcFetchedModules)] ICorDebugLoadedModule *pLoadedModules[]  
);  
```  
  
## <a name="parameters"></a>参数  

 `cRequestedModules`  
 [in] 请求了其信息的模块数。  
  
 `pcFetchedModules`  
 [out] 指向返回了相关信息的模块数的指针。  
  
 `pLoadedModules`  
 弄指向 [ICorDebugLoadedModule](icordebugloadedmodule-interface.md) 对象数组的指针，这些对象提供有关已加载模块的信息。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugDataTarget3 接口](icordebugdatatarget3-interface.md)
- [调试接口](debugging-interfaces.md)
