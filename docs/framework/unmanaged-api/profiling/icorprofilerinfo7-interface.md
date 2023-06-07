---
title: ICorProfilerInfo7 接口
ms.date: 03/30/2017
ms.assetid: cf37c462-73c5-412a-a7f8-bb26ca746313
ms.openlocfilehash: 4acafafa284549fe1b078542a88c0818dcde3038
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686054"
---
# <a name="icorprofilerinfo7-interface"></a>ICorProfilerInfo7 接口

[仅在 .NET Framework 4.6.1 及更高版本中受支持]  
  
 [ICorProfilerInfo6](icorprofilerinfo6-interface.md)的子类，它提供了一种方法，用于将新定义的元数据应用于模块，并提供对内存中符号流的访问。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[ApplyMetaData 方法](icorprofilerinfo7-applymetadata-method.md)|将方法的新定义的元数据应用于 `IMetadataEmit::Define*` 指定的模块。|  
|[GetInMemorySymbolsLength 方法](icorprofilerinfo7-getinmemorysymbolslength-method.md)|返回内存中符号流的长度。|  
|[ReadInMemorySymbols](icorprofilerinfo7-readinmemorysymbols.md)|从内存中的符号流中读取字节。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
