---
title: ICorProfilerCallback8：:D ynamicMethodJITCompilationFinished 方法
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback8.DynamicMethodJITCompilationFinished
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 6354667e754da42692cc0de2dc5330c56f951aa1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725438"
---
# <a name="icorprofilercallback8dynamicmethodjitcompilationfinished-method"></a>ICorProfilerCallback8：:D ynamicMethodJITCompilationFinished 方法

[.NET Framework 4.7 及更高版本中支持]  
  
当动态方法的 JIT 编译完成时，通知探查器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT DynamicMethodJITCompilationFinished(  
     [in]  FunctionID  functionId,
     [in]  BOOL        hrStatus,
     [in]  BOOL        fIsSafeToBlock
);  
```  
  
## <a name="parameters"></a>参数  

[in] `functionId`  
开始 JIT 编译的内存中函数的标识符。

[in] `hrStatus` 一个值，该值指示 JIT 编译是否成功。

[in] `fIsSafeToBlock` 
 `true`指示阻止可能会导致运行时等待调用线程从该回调返回;`false`指示阻止操作不会影响运行时的操作。  

## <a name="remarks"></a>注解  

只要动态方法的 JIT 编译完成，就会触发此回调。 这包括各种 IL 存根和 LCG 方法。 其目标是为探查器编写者提供足够的信息，以便向用户标识编译的方法。

> [!NOTE]
> `functionId` 由于动态方法没有元数据，因此不能使用值来解析为其元数据标记。

## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  
  
## <a name="see-also"></a>另请参阅

- [DynamicMethodJITCompilationStarted 方法](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)
- [ICorProfilerCallback8 接口](icorprofilercallback8-interface.md)
