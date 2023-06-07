---
title: ICorProfilerObjectEnum 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerObjectEnum
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerObjectEnum
helpviewer_keywords:
- ICorProfilerObjectEnum interface [.NET Framework profiling]
ms.assetid: 13e1651c-9523-40ef-bfd7-87fb94519f8b
topic_type:
- apiref
ms.openlocfilehash: 73c9f07ff9a7bffc2fb01c0dde390ca8364500b4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731171"
---
# <a name="icorprofilerobjectenum-interface"></a>ICorProfilerObjectEnum 接口

提供按顺序循环访问由 [Ngen.exe (本机映像生成器) ](../../tools/ngen-exe-native-image-generator.md)生成的冻结对象的集合的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Clone 方法](icorprofilerobjectenum-clone-method.md)|获取指向此 `ICorProfilerObjectEnum` 接口副本的接口指针。|  
|[GetCount 方法](icorprofilerobjectenum-getcount-method.md)|获取集合中冻结对象的总数。|  
|[Next 方法](icorprofilerobjectenum-next-method.md)|从对象的顺序集合中获取指定数目的连续对象，从该序列中的枚举器的当前位置开始。|  
|[Reset 方法](icorprofilerobjectenum-reset-method.md)|将此枚举器的光标移动到序列的起始位置。|  
|[Skip 方法](icorprofilerobjectenum-skip-method.md)|将此枚举器的光标从其当前位置前移，以便跳过指定数目的元素。|  
  
## <a name="remarks"></a>注解  

 `ICorProfilerObjectEnum` 接口是一个枚举器。 它可以让数组接收器以其合适的速率从发送器拉取元素。 换句话说，接收方能够显式控制数组元素的流，从而避免了将大型数组作为方法参数传递的相关问题。  
  
 使用 [ICorProfilerInfo2：： EnumModuleFrozenObjects](icorprofilerinfo2-enummodulefrozenobjects-method.md) 获取指向接口的指针 `ICorProfilerObjectEnum` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [EnumModuleFrozenObjects 方法](icorprofilerinfo2-enummodulefrozenobjects-method.md)
