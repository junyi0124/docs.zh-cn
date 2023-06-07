---
title: ICorDebugStaticFieldSymbol 接口
ms.date: 03/30/2017
ms.assetid: c0b93609-631e-4b15-878a-189ede922631
ms.openlocfilehash: fcf3bb61ccd903f2dd375e638814247a98aaf7b2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717560"
---
# <a name="icordebugstaticfieldsymbol-interface"></a>ICorDebugStaticFieldSymbol 接口

表示某个静态字段的调试符号信息。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetAddress 方法](icordebugstaticfieldsymbol-getaddress-method.md)|获取静态字段的地址。|  
|[GetName 方法](icordebugstaticfieldsymbol-getname-method.md)|获取静态字段的名称。|  
|[GetSize 方法](icordebugstaticfieldsymbol-getsize-method.md)|获取静态字段的大小（以字节为单位）。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugStaticFieldSymbol` 接口用于检索某个静态字段的调试符号信息。  
  
> [!NOTE]
> 此接口仅适用于 .NET Native。 如果在 .NET Native 外为 ICorDebug 方案实现此接口，则公共语言运行时将忽略此接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugInstanceFieldSymbol 接口](icordebuginstancefieldsymbol-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
