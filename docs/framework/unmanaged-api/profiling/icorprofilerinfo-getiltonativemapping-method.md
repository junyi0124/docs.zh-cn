---
title: ICorProfilerInfo::GetILToNativeMapping 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetILToNativeMapping
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetILToNativeMapping
helpviewer_keywords:
- GetILToNativeMapping method, ICorProfilerInfo interface [.NET Framework profiling]
- ICorProfilerInfo::GetILToNativeMapping method [.NET Framework profiling]
ms.assetid: 6a5431ef-22fb-4e53-bac5-703986297eb1
topic_type:
- apiref
ms.openlocfilehash: 1eb9b3af4c0e77fd1548de194d064eb85b86cdce
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95724140"
---
# <a name="icorprofilerinfogetiltonativemapping-method"></a>ICorProfilerInfo::GetILToNativeMapping 方法

针对指定函数中包含的代码，获取 Microsoft 中间语言 (MSIL) 偏移量到本机偏移量的映射。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetILToNativeMapping(  
    [in] FunctionID functionId,  
    [in] ULONG32 cMap,  
    [out] ULONG32 *pcMap,  
    [out, size_is(cMap), length_is(*pcMap)]  
        COR_DEBUG_IL_TO_NATIVE_MAP map[]);  
```  
  
## <a name="parameters"></a>参数  

 `functionId`  
 [in] 包含代码的函数 ID。  
  
 `cMap`  
 [in] `map` 数组的最大大小。  
  
 `pcMap`  
 [out] COR_DEBUG_IL_TO_NATIVE_MAP 结构的可用总数。  
  
 `map`  
 [out] `COR_DEBUG_IL_TO_NATIVE_MAP` 结构的数组，其中每个结构均指定偏移量。 `GetILToNativeMapping` 方法返回后，`map` 将包含部分或全部 `COR_DEBUG_IL_TO_NATIVE_MAP` 结构。  
  
## <a name="remarks"></a>注解  

 `GetILToNativeMapping` 方法返回 `COR_DEBUG_IL_TO_NATIVE_MAP` 结构的数组。 为了传达特定范围的本机指令与代码的特殊区域 (例如，prolog) ，数组中的条目可以 `ilOffset` 将其字段设置为 [CorDebugIlToNativeMappingTypes](../debugging/cordebugiltonativemappingtypes-enumeration.md) 枚举的值。  
  
 返回 `GetILToNativeMapping` 后，必须验证 `map` 缓冲区大小是否足以包含所有 `COR_DEBUG_IL_TO_NATIVE_MAP` 结构。 为此，请将 `cMap` 的值和 `pcMap` 参数的值进行比较。 如果 `pcMap` 值乘以 `COR_DEBUG_IL_TO_NATIVE_MAP` 结构的大小所得的值大于 `cMap`，请分配更大的 `map` 缓冲区、将 `cMap` 更新为新的更大大小，并再次调用 `GetILToNativeMapping`。  
  
 或者，可以先用长度为零的 `map` 缓冲区调用 `GetILToNativeMapping` 以获取正确的缓冲区大小。 然后，可将缓冲区大小设置为 `pcMap` 中返回的值，并再次调用 `GetILToNativeMapping`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **头文件：** CorProf.idl、CorProf.h  
  
 **库：** CorGuids.lib  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [ICorProfilerInfo 接口](icorprofilerinfo-interface.md)
- [GetILToNativeMapping2 方法](icorprofilerinfo4-getiltonativemapping2-method.md)
- [分析接口](profiling-interfaces.md)
- [分析](index.md)
