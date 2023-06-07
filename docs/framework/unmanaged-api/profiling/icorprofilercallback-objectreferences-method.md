---
title: ICorProfilerCallback::ObjectReferences 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ObjectReferences
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ObjectReferences
helpviewer_keywords:
- ObjectReferences method [.NET Framework profiling]
- ICorProfilerCallback::ObjectReferences method [.NET Framework profiling]
ms.assetid: dd5e9b64-b4a3-4ba6-9be6-ddb540f4ffcf
topic_type:
- apiref
ms.openlocfilehash: 9485e3ca657ab108d2bcc9d00b1c475f8ee3c086
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703949"
---
# <a name="icorprofilercallbackobjectreferences-method"></a>ICorProfilerCallback::ObjectReferences 方法

通知探查器有关指定的对象正在引用的内存中的对象。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT ObjectReferences(  
    [in]  ObjectID objectId,  
    [in]  ClassID  classId,  
    [in]  ULONG    cObjectRefs,  
    [in, size_is(cObjectRefs)] ObjectID objectRefIds[] );  
```  
  
## <a name="parameters"></a>参数  

 `objectId`  
 中引用对象的对象的 ID。  
  
 `classId`  
 中指定对象是其实例的类的 ID。  
  
 `cObjectRefs`  
 中指定的对象引用的对象数 (即数组中的元素数 `objectRefIds`) 。  
  
 `objectRefIds`  
 中由引用的对象的 Id 数组 `objectId` 。  
  
## <a name="remarks"></a>注解  

 `ObjectReferences`完成垃圾回收后，将为堆中剩余的每个对象调用方法。 如果探查器从此回调返回错误，则分析服务将停止调用此回调，直到下一次垃圾回收。  
  
 `ObjectReferences`回调可与[ICorProfilerCallback：： RootReferences](icorprofilercallback-rootreferences-method.md)回调结合使用，以便为运行时创建完整的对象引用关系图。 公共语言运行时 (CLR) 确保每个对象引用只通过方法报告一次 `ObjectReferences` 。  
  
 返回的对象 Id 在 `ObjectReferences` 回调过程中无效，因为垃圾回收可能是在移动对象的中间。 因此，探查器不能尝试在调用过程中检查对象 `ObjectReferences` 。 调用 [ICorProfilerCallback2：： GarbageCollectionFinished](icorprofilercallback2-garbagecollectionfinished-method.md) 时，垃圾回收已完成，可安全完成检查。  
  
 如果为 null `ClassId` ， `objectId` 则表示具有正在卸载的类型。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerCallback 接口](icorprofilercallback-interface.md)
