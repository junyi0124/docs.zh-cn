---
title: ICorProfilerInfo7：： GetInMemorySymbolsLength 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo7.GetInMemorySymbolsLength
api_location:
- mscorwks.dll
- icorprof.idl
api_type:
- COM
ms.assetid: d62c4a4c-8a62-45aa-8f01-a8387cf36159
ms.openlocfilehash: 46ffa5cb4fac6988240d32cb1939cc25bdf0a412
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686067"
---
# <a name="icorprofilerinfo7getinmemorysymbolslength-method"></a>ICorProfilerInfo7：： GetInMemorySymbolsLength 方法

[仅在 .NET Framework 4.6.1 及更高版本中受支持]  
  
 返回内存中符号流的长度。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetInMemorySymbolsLength(  
        [in] ModuleID moduleId,  
        [out] DWORD* pCountSymbolBytes  
);  
```  
  
## <a name="parameters"></a>参数  

 `moduleId`  
 中包含内存中流的模块的标识符。  
  
 pCountSymbolBytes  
 弄一个指向 `DWORD` 值的指针，当该方法返回时，将包含流的长度（以字节为单位）。  
  
## <a name="return-value"></a>返回值  

 `S_OK`如果可以确定内存流的长度，则该方法返回，即使它为零 (0) 。  
  
 `CORPROF_E_MODULE_IS_DYNAMIC`如果该方法是使用创建的，则该方法返回 <xref:System.Reflection.Emit?displayProperty=nameWithType> 。  
  
## <a name="remarks"></a>注解  

 如果模块有内存中符号，则流的长度将被置于中 `pCountSymbolBytes` 。 如果模块没有内存中符号，则为 `*pCountSymbolBytes = 0` 。  
  
> [!NOTE]
> 当前实现不支持反射。发出。 如果该模块是使用反射创建的，则该方法返回 `CORPROF_E_MODULE_IS_DYNAMIC` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v461plus](../../../../includes/net-current-v461plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo7 接口](icorprofilerinfo7-interface.md)
