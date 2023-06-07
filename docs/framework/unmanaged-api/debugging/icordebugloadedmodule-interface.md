---
title: ICorDebugLoadedModule 接口
ms.date: 03/30/2017
ms.assetid: 34be6369-2e75-4a95-a538-3b29ac97cf6d
ms.openlocfilehash: 6087411e8d23a9c3c97cb97ac8159d436e24759b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731822"
---
# <a name="icordebugloadedmodule-interface"></a>ICorDebugLoadedModule 接口

提供有关已加载模块的信息。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetBaseAddress 方法](icordebugloadedmodule-getbaseaddress-method.md)|获取已加载模块的基址。|  
|[GetName 方法](icordebugloadedmodule-getname-method.md)|获取加载模块的名称。|  
|[GetSize 方法](icordebugloadedmodule-getsize-method.md)|获取加载模块的大小（以字节为单位）。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugLoadedModule` 接口由调试器实现并且被 CLR 调试接口使用，以便从调试器中获取有关加载的模块的信息。  
  
> [!NOTE]
> 此接口仅适用于 .NET Native。 如果在 .NET Native 外为 ICorDebug 方案实现此接口，则公共语言运行时将忽略此接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
