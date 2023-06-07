---
title: ICorDebugAppDomain3::GetCachedWinRTTypesForIIDs 方法
ms.date: 03/30/2017
api_name:
- ICorDebugAppDomain3.GetCachedWinRTTypesForIIDs
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugAppDomain3::GetCachedWinRTTypesForIIDs
helpviewer_keywords:
- ICorDebugAppDomain3::GetCachedWinRTTypesForIIDs method, [.NET Framework debugging]
- GetCachedWinRTTypesForIIDs method, ICorDebugAppDomain3 interface [.NET Framework debugging]
ms.assetid: 23682ca0-1bcf-48e6-996e-69f7ba337682
topic_type:
- apiref
ms.openlocfilehash: 2aff86fb63b87869ed13028bd7344afe11363f51
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723176"
---
# <a name="icordebugappdomain3getcachedwinrttypesforiids-method"></a>ICorDebugAppDomain3::GetCachedWinRTTypesForIIDs 方法

基于其接口标识符获取应用程序域中缓存的 Windows 运行时类型的枚举器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetCachedWinRTTypesForIIDs (
    [in]  ULONG32            cReqTypes,  
    [in]  GUID                *iidsToResolve,  
    [out] ICorDebugTypeEnum   **ppTypesEnum  
);  
```  
  
## <a name="parameters"></a>参数  

 `cReqTypes`  
 中所需类型的数目。  
  
 `iidsToResolve`  
 中指向数组的指针，该数组包含与要检索的 Windows 运行时类型的托管表示形式对应的接口标识符。  
  
 `ppTypesEnum`  
 弄一个指向 "ICorDebugTypeEnum" 接口对象地址的指针，该对象可基于中的接口标识符枚举检索到的 Windows 运行时类型的缓存托管表示形式 `iidsToResolve` 。  
  
## <a name="remarks"></a>注解  

 如果该方法无法检索特定接口标识符的信息，则 "ICorDebugTypeEnum" 集合中的对应条目将有一种类型的 `ELEMENT_TYPE_END` 用于错误，这是由于数据检索问题或 `ELEMENT_TYPE_VOID` 未知接口标识符导致的。  
  
## <a name="requirements"></a>要求  

 **平台：** Windows 运行时  
  
 **标头**：CorDebug.idl、CorDebug.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorDebugAppDomain3 接口](icordebugappdomain3-interface.md)
