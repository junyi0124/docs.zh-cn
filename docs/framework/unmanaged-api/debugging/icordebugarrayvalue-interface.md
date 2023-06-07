---
title: ICorDebugArrayValue 接口
ms.date: 03/30/2017
api_name:
- ICorDebugArrayValue
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugArrayValue interface
helpviewer_keywords:
- ICorDebugArrayValue interface [.NET Framework debugging]
ms.assetid: dc437751-7093-44e2-bfdc-191d9ce3c192
topic_type:
- apiref
ms.openlocfilehash: 90688132b98f8316a4b08988c08b2f7cc7ce0fd8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725039"
---
# <a name="icordebugarrayvalue-interface"></a>ICorDebugArrayValue 接口

表示一维或多维数组的 ICorDebugHeapValue 的子类。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetBaseIndicies 方法](icordebugarrayvalue-getbaseindicies-method.md)|获取数组中每个维的基索引。|  
|[GetCount 方法](icordebugarrayvalue-getcount-method.md)|获取数组中的元素总数。|  
|[GetDimensions 方法](icordebugarrayvalue-getdimensions-method.md)|获取数组的尺寸。|  
|[GetElement 方法](icordebugarrayvalue-getelement-method.md)|获取一个值，该值表示数组中的给定元素。|  
|[GetElementAtPosition 方法](icordebugarrayvalue-getelementatposition-method.md)|获取位于给定位置的元素，并将该数组视为从零开始的一维数组。|  
|[GetElementType 方法](icordebugarrayvalue-getelementtype-method.md)|获取数组中元素的简单类型。|  
|[GetRank 方法](icordebugarrayvalue-getrank-method.md)|获取数组中的维度数。|  
|[HasBaseIndicies 方法](icordebugarrayvalue-hasbaseindicies-method.md)|确定数组是否具有基索引。|  
  
## <a name="remarks"></a>注解  

 `ICorDebugArrayValue` 支持一维数组和多维数组。  
  
> [!NOTE]
> 此接口不支持跨计算机或跨进程远程调用。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [调试接口](debugging-interfaces.md)
