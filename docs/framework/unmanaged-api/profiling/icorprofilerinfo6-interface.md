---
title: ICorProfilerInfo6 接口
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo6
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 6f2bb148-1e2b-4e45-a5a5-0ceddc40064b
ms.openlocfilehash: b3aed97e19694675fd5e0c1070dbbf6d9321eedd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733836"
---
# <a name="icorprofilerinfo6-interface"></a>ICorProfilerInfo6 接口

[.NET Framework 4.6 及更高版本中支持]  
  
 [ICorProfilerInfo5](icorprofilerinfo5-interface.md)的子类，为在给定的 NGen 模块中定义并内联给定方法的所有方法提供了一个枚举器。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ICorProfilerInfo6::EnumNgenModuleMethodsInliningThisMethod 方法](icorprofilerinfo6-enumngenmodulemethodsinliningthismethod-method.md)|返回属于给定的 NGen 模块并在给定方法的主体中内联的所有方法的枚举器。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
