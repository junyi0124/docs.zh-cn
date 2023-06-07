---
title: ICorProfilerInfo8 接口
ms.date: 08/06/2019
author: davmason
ms.author: davmason
ms.openlocfilehash: eedd16006781de517587e5138543b9b9eca3ff90
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733602"
---
# <a name="icorprofilerinfo8-interface"></a>ICorProfilerInfo8 接口

[ICorProfilerInfo7](icorprofilerinfo7-interface.md)的子类，提供查询有关动态方法的信息的方法。

## <a name="methods"></a>方法  

| 方法|说明|  
| ------------|-----------------|  
|[IsFunctionDynamic 方法](icorprofilerinfo8-isfunctiondynamic-method.md)| 确定函数是否没有关联的元数据。|
|[GetFunctionFromIP3 方法](icorprofilerinfo8-getfunctionfromip3-method.md)| 将托管代码指令指针映射到 FunctionID。 此方法适用于动态和非动态方法。 |
|[GetDynamicFunctionInfo 方法](icorprofilerinfo8-getdynamicfunctioninfo-method.md)| 检索有关动态方法的信息。 |

## <a name="requirements"></a>要求  

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
**头文件：** CorProf.idl、CorProf.h  
**.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  

## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
