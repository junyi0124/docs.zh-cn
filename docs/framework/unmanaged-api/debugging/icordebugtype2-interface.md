---
title: ICorDebugType2 接口
ms.date: 03/30/2017
api_name:
- ICorDebugType2
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugType2
helpviewer_keywords:
- ICorDebugType2 interface
ms.assetid: 376fb03f-f1ef-4107-baa4-4d9d55884862
topic_type:
- apiref
ms.openlocfilehash: 0d5fffe4350cc1f58acf588f288db3bdb7e213d0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725663"
---
# <a name="icordebugtype2-interface"></a>ICorDebugType2 接口

扩展 ICorDebugType 接口以检索基类型或复杂 (用户定义的) 类型的类型标识符。  
  
## <a name="methods"></a>方法  
  
|方法||  
|------------|-|  
|[GetTypeID 方法](icordebugtype2-gettypeid-method.md)|获取此类型的 [COR_TYPEID](cor-typeid-structure.md) 。|  
  
## <a name="remarks"></a>注解  

 此接口是 ICorDebugType 接口的逻辑扩展。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="example"></a>示例  

 下面的代码段演示了如何使用 [ICorDebugType2：： GetTypeID](icordebugtype2-gettypeid-method.md) 方法。  
  
```cpp  
// (error checking omitted for brevity)  
// given an ICorDebugType *pType  
  
ICorDebugType2 *pType2 = NULL;  
pType->QueryInterface(IID_ICorDebugType2, &pType);  
  
COR_TYPEID id;  
pType2->GetTypeID(&id);  
  
// now we can use existing APIs to get information about this COR_TYPEID  
```  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v462plus](../../../../includes/net-current-v462plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
