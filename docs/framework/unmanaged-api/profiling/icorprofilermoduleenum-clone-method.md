---
title: ICorProfilerModuleEnum::Clone 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerModuleEnum.Clone Method
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerModuleEnum::Clone
helpviewer_keywords:
- Clone method, ICorProfilerModuleEnum interface [.NET Framework profiling]
- ICorProfilerModuleEnum::Clone method [.NET Framework profiling]
ms.assetid: 7dec7e36-8d88-416d-b437-abf98b76c1df
topic_type:
- apiref
ms.openlocfilehash: ae9f6b7865a80e3edc4cae8fd1298e5eed864377
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722825"
---
# <a name="icorprofilermoduleenumclone-method"></a>ICorProfilerModuleEnum::Clone 方法

获取一个接口指针，该指针指向此 [ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md) 接口的副本。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT Clone([out] ICorProfilerObjectEnum **ppEnum);  
```  
  
## <a name="parameters"></a>参数  

 `ppEnum`  
 弄指向接口指针的指针，该指针指向此 [ICorProfilerModuleEnum](icorprofilermoduleenum-interface.md) 接口的副本。 枚举器的副本与此枚举器分别维护自己的枚举状态。 但是，副本的初始光标位置与此枚举器的当前游标位置相同。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerModuleEnum 接口](icorprofilermoduleenum-interface.md)
- [分析接口](profiling-interfaces.md)
