---
title: ICorDebugProcess5 接口
ms.date: 03/30/2017
api_name:
- ICorDebugProcess5
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugProcess5
helpviewer_keywords:
- ICorDebugProcess5 interface [.NET Framework debugging]
ms.assetid: 30a39d79-1f10-4328-9c5d-094ed824e2ba
topic_type:
- apiref
ms.openlocfilehash: cef69ac7e3572b67dd676ce8408e4210d93accf0
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717573"
---
# <a name="icordebugprocess5-interface"></a>ICorDebugProcess5 接口

扩展 ICorDebugProcess 接口以支持对托管堆的访问，以提供有关托管对象的垃圾回收的信息，以及确定调试器是否从应用程序本地本机映像缓存中加载图像。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EnableNGenPolicy 方法](icordebugprocess5-enablengenpolicy-method.md)|设置一个值，该值确定应用程序在托管调试器下运行时如何加载本机映像。|  
|[EnumerateGCReferences 方法](icordebugprocess5-enumerategcreferences-method.md)|获取一个枚举器，该枚举器用于进程中要进行垃圾回收的所有对象。|  
|[EnumerateHandles 方法](icordebugprocess5-enumeratehandles-method.md)|获取进程中的对象句柄的枚举器。|  
|[EnumerateHeap 方法](icordebugprocess5-enumerateheap-method.md)|获取托管堆上的对象的枚举器。|  
|[EnumerateHeapRegions 方法](icordebugprocess5-enumerateheapregions-method.md)|获取托管堆区域的枚举器。|  
|[GetArrayLayout 方法](icordebugprocess5-getarraylayout-method.md)|获取有关内存中数组的布局的信息。|  
|[GetGCHeapInformation 方法](icordebugprocess5-getgcheapinformation-method.md)|获取一个指向 [COR_HEAPINFO](cor-heapinfo-structure.md) 结构的指针，该结构包含有关要在托管堆上进行垃圾回收的对象的信息。|  
|[GetObject 方法](icordebugprocess5-getobject-method.md)|获取指向托管堆上的对象的指针。|  
|[GetTypeFields 方法](icordebugprocess5-gettypefields-method.md)|获取一个指针，该指针指向一个数组，该数组包含基于类型标识符的类型的字段信息。|  
|[GetTypeForTypeID 方法](icordebugprocess5-gettypefortypeid-method.md)|获取一个类型对象，该对象基于其类型标识符提供有关对象的信息。|  
|[GetTypeID 方法](icordebugprocess5-gettypeid-method.md)|获取指定地址处的对象的类型标识符。|  
|[GetTypeLayout 方法](icordebugprocess5-gettypelayout-method.md)|根据对象的类型标识符获取有关该对象在内存中的布局的信息。|  
  
## <a name="remarks"></a>注解  

 此接口以逻辑方式扩展了 ICorDebugProcess、ICorDebugProcess2 和 [ICorDebugProcess3](icordebugprocess3-interface.md) 接口。  
  
> [!NOTE]
> 此接口不支持从另一台计算机或从其他进程进行远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
- [调试](index.md)
