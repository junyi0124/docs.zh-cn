---
title: EnumerateCLRs 函数
ms.date: 03/30/2017
api_name:
- EnumerateCLRs
api_location:
- dbgshim.dll
api_type:
- COM
f1_keywords:
- EnumerateCLRs
helpviewer_keywords:
- debugging API [Silverlight]
- Silverlight, debugging
- EnumerateCLRs function
ms.assetid: f8d50cb3-ec4f-4529-8fe3-bd61fd28e13c
topic_type:
- apiref
ms.openlocfilehash: 8fe0df3ee08f9915ed43990b6f9686e25b183a0b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676096"
---
# <a name="enumerateclrs-function"></a>EnumerateCLRs 函数

提供枚举进程中 CLR 的机制。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumerateCLRs (  
    [in]  DWORD      debuggeePID,  
    [out] HANDLE**   ppHandleArrayOut,  
    [out] LPWSTR**   ppStringArrayOut,  
    [out] DWORD*     pdwArrayLengthOut  
);  
```  
  
## <a name="parameters"></a>参数  

 `debuggeePID`  
 [in] 要从其枚举已加载的 CLR 的进程的进程标识符。  
  
 `ppHandleArrayOut`  
 [out] 指向包含用于继续 CLR 启动的事件句柄的数组的指针。 不保证数组中的每个句柄都有效。 有效的句柄将作为相应运行时（位于与 `ppStringArrayOut` 相同的索引中）的继续启动事件使用。  
  
 `ppStringArrayOut`  
 [out] 指向一个字符串数组的指针，该字符串数组指定进程中加载的 CLR 的完整路径。  
  
 `pdwArrayLengthOut`  
 [out] 指向包含大小相等的 `ppHandleArrayOut` 和 `pdwArrayLengthOut` 的长度的 DWORD 的指针。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 已成功确定该进程中的 CLR 的数目，并已正确填写相应的句柄数组和路径数组。  
  
 E_INVALIDARG  
 `ppHandleArrayOut` 或 `ppStringArrayOut` 为 null，或 `pdwArrayLengthOut` 为 null。  
  
 E_OUTOFMEMORY  
 该函数不能为句柄和路径数组分配足够的内存。  
  
 E_FAIL（或其他 E_ 返回代码）  
 无法枚举加载的 CLR。  
  
## <a name="remarks"></a>注解  

 对于由 `debuggeePID` 标识的目标进程，函数将路径数组 `ppStringArrayOut` 返回到进程中加载的 CLR；并返回事件句柄数组 `ppHandleArrayOut`（其中可能包含相同索引处 CLR 的继续启动事件）以及数组的大小 `pdwArrayLengthOut`（它指定已加载的 CLR 的数目）。  
  
 在 Windows 操作系统上，`debuggeePID` 映射到 OS 进程标识符。  
  
 `ppHandleArrayOut` 和 `ppStringArrayOut` 的内存由此函数分配。 若要释放分配的内存，必须调用 [CloseCLREnumeration 函数](closeclrenumeration-function.md)。  
  
 为了在目标进程中返回 CLR 计数，可在两个数组参数均设为 null 的情况下调用此函数。 调用方可以从此计数推断将创建的缓冲区的大小：`(sizeof(HANDLE) * count) + (sizeof(LPWSTR) * count) + (sizeof(WCHAR*) * count * MAX_PATH)`。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** dbgshim.dll  
  
 **库：** dbgshim.dll  
  
 **.NET Framework 版本：** 3.5 SP1
