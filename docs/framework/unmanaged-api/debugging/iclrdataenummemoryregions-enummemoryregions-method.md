---
title: ICLRDataEnumMemoryRegions::EnumMemoryRegions 方法
ms.date: 03/30/2017
api_name:
- ICLRDataEnumMemoryRegions.EnumMemoryRegions
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICLRDataEnumMemoryRegions::EnumMemoryRegions
helpviewer_keywords:
- ICLRDataEnumMemoryRegions::EnumMemoryRegions method [.NET Framework debugging]
- EnumMemoryRegions method [.NET Framework debugging]
ms.assetid: 22d2e339-f174-40b5-a478-0b744501566f
topic_type:
- apiref
ms.openlocfilehash: 386f975ab0bbbe804fda2bd7567acf24f69e77fb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676070"
---
# <a name="iclrdataenummemoryregionsenummemoryregions-method"></a>ICLRDataEnumMemoryRegions::EnumMemoryRegions 方法

枚举指定的内存区域。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumMemoryRegions (  
    [in] ICLRDataEnumMemoryRegionsCallback  *callback,  
    [in] ULONG32                            miniDumpFlags,  
    [in] CLRDataEnumMemoryFlags             clrFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `callback`  
 中指向 [ICLRDataEnumMemoryRegionsCallback](iclrdataenummemoryregionscallback-interface.md) 实例的指针，此方法为每个要枚举的内存区域调用此方法，以通知调试器结果。  
  
 即使回调指示失败，内存区域的枚举仍将继续。  
  
 `miniDumpFlags`  
 中不使用。  
  
 `clrFlags`  
 中 [CLRDataEnumMemoryFlags](clrdataenummemoryflags-enumeration.md) 枚举的一个值，该值指定要枚举的内存区域。  
  
## <a name="remarks"></a>注解  

 此方法使用指定的 [ICLRDataEnumMemoryRegionsCallback](iclrdataenummemoryregionscallback-interface.md) 实例将结果通知给调用方。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** ClrData，ClrData  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICLRDataEnumMemoryRegions 接口](iclrdataenummemoryregions-interface.md)
