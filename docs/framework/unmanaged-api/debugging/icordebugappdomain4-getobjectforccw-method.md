---
title: ICorDebugAppDomain4::GetObjectForCCW 方法
ms.date: 03/30/2017
ms.assetid: 2cacdb85-e7b8-42e7-b310-c3e8c22e5d33
ms.openlocfilehash: f3e64def16fb2817244ef7669ff4bb7fef0bd07c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95684443"
---
# <a name="icordebugappdomain4getobjectforccw-method"></a>ICorDebugAppDomain4::GetObjectForCCW 方法

从 COM 可调用包装器 (CCW) 指针获取托管对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetObjectForCCW(  
   [in]CORDB_ADDRESS ccwPointer,
   [out]ICorDebugValue **ppManagedObject  
);  
```  
  
## <a name="parameters"></a>参数  

 `ccwPointer`  
 [in] COM 可调用包装器 (CCW) 指针。  
  
 `ppManagedObject`  
 弄一个指向 "ICorDebugValue" 对象地址的指针，该对象表示与给定的 CCW 指针相对应的托管对象。  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugAppDomain4 接口](icordebugappdomain4-interface.md)
- [调试接口](debugging-interfaces.md)
