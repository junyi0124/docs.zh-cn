---
title: ICorProfilerInfo::GetModuleMetaData 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetModuleMetaData
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetModuleMetaData
helpviewer_keywords:
- GetModuleMetaData method [.NET Framework profiling]
- ICorProfilerInfo::GetModuleMetaData method [.NET Framework profiling]
ms.assetid: 7a439d92-348a-44dd-b60f-cad7cba56379
topic_type:
- apiref
ms.openlocfilehash: 74df0fb412e7fb3d9f779391ec84f07a0379a2cc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724112"
---
# <a name="icorprofilerinfogetmodulemetadata-method"></a>ICorProfilerInfo::GetModuleMetaData 方法

获取映射到指定模块的元数据接口实例。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetModuleMetaData(  
    [in]  ModuleID moduleId,  
    [in]  DWORD    dwOpenFlags,  
    [in]  REFIID   riid,  
    [out] IUnknown **ppOut);  
```  
  
## <a name="parameters"></a>参数  

 `moduleId`  
 中接口实例将映射到的模块的 ID。  
  
 `dwOpenFlags`  
 中 [CorOpenFlags](../metadata/coropenflags-enumeration.md) 枚举的一个值，该值指定用于打开清单文件的模式。 只有 `ofRead` 、 `ofWrite` 和 `ofNoTransform` 位有效。  
  
 `riid`  
 中将检索其实例的元数据接口的引用 ID (GUID) 。 有关接口列表，请参阅 [元数据接口](../metadata/metadata-interfaces.md) 。  
  
 `ppOut`  
 弄指向元数据接口实例的地址的指针。  
  
## <a name="remarks"></a>注解  

 你可能要求在读/写模式下打开元数据，但这会导致程序的元数据执行速度变慢，因为对元数据所做的更改不会因为它们来自编译器而进行优化。  
  
 某些模块 (例如资源模块) 没有元数据。 在这些情况下， `GetModuleMetaData` 将返回的 HRESULT 值 S_FALSE，并在 * 中返回 null `ppOut` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
