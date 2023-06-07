---
title: ICorDebugSymbolProvider2 接口
ms.date: 03/30/2017
ms.assetid: 1c9c3d92-f0de-4d4d-87f1-0c702a4808af
ms.openlocfilehash: 3bef03e9e85224058bc17e1f0ce8746e98aa30f6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95688336"
---
# <a name="icordebugsymbolprovider2-interface"></a>ICorDebugSymbolProvider2 接口

以逻辑方式扩展 [ICorDebugSymbolProvider](icordebugsymbolprovider-interface.md) 接口以检索其他调试符号信息。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetFrameProps 方法](icordebugsymbolprovider2-getframeprops-method.md)|返回启动方法相对虚拟地址的方法和具有代码相对虚拟地址的父框架。|  
|[GetGenericDictionaryInfo 方法](icordebugsymbolprovider2-getgenericdictionaryinfo-method.md)|检索泛型字典映射。|  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 此接口仅适用于 .NET Native。 如果在 .NET Native 外为 ICorDebug 方案实现此接口，则公共语言运行时将忽略此接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_46_native](../../../../includes/net-46-native-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugSymbolProvider 接口](icordebugsymbolprovider-interface.md)
- [调试接口](debugging-interfaces.md)
- [调试](index.md)
