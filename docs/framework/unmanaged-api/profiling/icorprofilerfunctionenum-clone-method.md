---
title: ICorProfilerFunctionEnum::Clone 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerFunctionEnum.Clone Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerFunctionEnum::Clone
helpviewer_keywords:
- ICorProfilerFunctionEnum::Clone method [.NET Framework profiling]
- Clone method, ICorProfilerFunctionEnum interface [.NET Framework profiling]
ms.assetid: 0c3d4835-e111-4e82-af6d-53f140b8f2c9
topic_type:
- apiref
ms.openlocfilehash: 092c3b328887b7d36ead77c64d31310b614b616a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95707524"
---
# <a name="icorprofilerfunctionenumclone-method"></a>ICorProfilerFunctionEnum::Clone 方法

获取一个接口指针，该指针指向此 [ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md) 接口的副本。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Clone([out] ICorProfilerFunctionEnum **ppEnum);  
```  
  
## <a name="parameters"></a>参数  

 `ppEnum`  
 弄指向接口指针的指针，该指针反过来指向此 [ICorProfilerFunctionEnum](icorprofilerfunctionenum-interface.md) 接口的副本。 枚举器的副本与此枚举器分别维护自己的枚举状态。 但是，副本的初始光标位置与此枚举器的当前游标位置相同。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerFunctionEnum 接口](icorprofilerfunctionenum-interface.md)
- [分析接口](profiling-interfaces.md)
