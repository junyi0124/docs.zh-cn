---
title: ICorProfilerInfo2::GetGenerationBounds 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo2.GetGenerationBounds
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo2::GetGenerationBounds
helpviewer_keywords:
- ICorProfilerInfo2::GetGenerationBounds method [.NET Framework profiling]
- GetGenerationBounds method [.NET Framework profiling]
ms.assetid: 9c37185f-d1e0-4a6e-8b99-707f7df61d88
topic_type:
- apiref
ms.openlocfilehash: 2b9bf1a9f40764f6d0544bafb91f967905eb40c7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703900"
---
# <a name="icorprofilerinfo2getgenerationbounds-method"></a>ICorProfilerInfo2::GetGenerationBounds 方法

获取属于堆段的内存区域，堆段构成各代垃圾回收。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetGenerationBounds(  
    [in]  ULONG cObjectRanges,  
    [out] ULONG *pcObjectRanges,  
    [out, size_is(cObjectRanges), length_is(*pcObjectRanges)] COR_PRF_GC_GENERATION_RANGE ranges[]);  
```  
  
## <a name="parameters"></a>参数  

 `cObjectRanges`  
 [in] 由调用方为 `ranges` 数组分配的元素数目。  
  
 `pcObjectRanges`  
 [out] 指向指定范围总数的整数的指针，部分或所有范围都将在 `ranges` 数组中返回。  
  
 `ranges`  
 弄 [COR_PRF_GC_GENERATION_RANGE](cor-prf-gc-generation-range-structure.md) 结构的数组，其中每个结构都描述一个范围 (即，阻止生成中正在进行垃圾回收的内存) 。  
  
## <a name="remarks"></a>注解  

 可以从任何探查器回调调用 `GetGenerationBounds` 方法，前提是当前未进行垃圾回收。

 大多数代切换都发生在垃圾回收期间。 在回收之间，代可能会增长，但通常不会反复切换。 因此，调用 `GetGenerationBounds` 最具特色的地方在于 `ICorProfilerCallback2::GarbageCollectionStarted` 和 `ICorProfilerCallback2::GarbageCollectionFinished` 中。  
  
 在程序启动期间，某些对象是由公共语言运行时 (CLR) 自身分配的，通常发生在第 3 代和第 0 代中。 因此，当托管代码开始执行时，这些代将已经包含对象。 第 1 代和第 2 代通常将为空，但由垃圾回收器生成的虚拟对象除外。 在 CLR 的32位实现中 (虚拟对象的大小为12个字节;64位实现中的大小较大。 ) 你还可能看到 ( # A0) 的本机映像生成器生成的模块内的第2代范围。 在这种情况下，第2代中的对象是 *冻结对象*，在 NGen.exe 运行而不是垃圾回收器时分配。  
  
 此函数使用调用方分配的缓冲区。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [ICorProfilerInfo2 接口](icorprofilerinfo2-interface.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
