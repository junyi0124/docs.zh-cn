---
title: ICorDebugAssembly3::EnumerateContainedAssemblies 方法
ms.date: 03/30/2017
ms.assetid: 98f15b05-afad-4616-9e2a-1a9af31948b6
ms.openlocfilehash: 1e040453d5eb7a312f2e665974486492b99de16d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719679"
---
# <a name="icordebugassembly3enumeratecontainedassemblies-method"></a>ICorDebugAssembly3::EnumerateContainedAssemblies 方法

获取该程序集中所包含的程序集的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateContainedAssemblies(  
    ICorDebugAssemblyEnum **ppAssemblies  
);  
```  
  
## <a name="parameters"></a>参数  

 `ppAssemblies`  
 [输出] 指针指向 ICor调试程序集枚举器界面对象（即枚举器）的地址。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 若该 `ICorDebugAssembly3` 对象为容器；反之，`S_FALSE`，枚举为空。  
  
## <a name="remarks"></a>注解  

 需用符号来列举包含的程序集。 如果其并不存在，则方法返回 `S_FALSE`，且不提供有效枚举。  
  
> [!NOTE]
> 此方法仅适用于 .NET Native。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [“ICor调试程序集3”接口](icordebugassembly3-interface.md)
- [调试接口](debugging-interfaces.md)
