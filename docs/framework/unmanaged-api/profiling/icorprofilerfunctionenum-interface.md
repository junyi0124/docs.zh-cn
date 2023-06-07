---
title: ICorProfilerFunctionEnum 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionEnum
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionEnum
helpviewer_keywords:
- ICorProfilerFunctionEnum interface [.NET Framework profiling]
ms.assetid: 0a1d4a38-cd0b-4231-91df-13646218ae72
topic_type:
- apiref
ms.openlocfilehash: 84c3b504dff8a04172dde903c1681c9f3fb2fcd2
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95669222"
---
# <a name="icorprofilerfunctionenum-interface"></a>ICorProfilerFunctionEnum 接口

提供按顺序循环访问公共语言运行时中的函数集合的方法。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[Clone 方法](icorprofilerfunctionenum-clone-method.md)|获取指向此 `ICorProfilerFunctionEnum` 接口副本的接口指针。|  
|[GetCount 方法](icorprofilerfunctionenum-getcount-method.md)|获取应用程序加载的函数数量或探查器强制加载的函数数量。|  
|[Next 方法](icorprofilerfunctionenum-next-method.md)|从一个函数的顺序集合中获取指定数量的连续函数（从枚举器在序列中的当前位置开始）。|  
|[Reset 方法](icorprofilerfunctionenum-reset-method.md)|将枚举器的光标移动到序列的起始位置。|  
|[Skip 方法](icorprofilerfunctionenum-skip-method.md)|将枚举器的游标从其当前位置前移，以便跳过指定数量的元素。|  
  
## <a name="remarks"></a>注解  

 `ICorProfilerFunctionEnum` 接口是一个枚举器。 它可以让数组接收器以其合适的速率从发送器拉取元素。 换而言之，接收器可以显式控制数组元素流，从而避免将大型数组作为方法形参传递方面的相关问题。  
  
 `ICorProfilerFunctionEnum` 枚举已经过 JIT 编译的函数，但不包括从使用 Ngen.exe 生成的本机映像加载的函数。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [分析接口](profiling-interfaces.md)
- [EnumJITedFunctions 方法](icorprofilerinfo3-enumjitedfunctions-method.md)
