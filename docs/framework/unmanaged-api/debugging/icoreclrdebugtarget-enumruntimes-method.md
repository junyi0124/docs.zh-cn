---
title: ICoreClrDebugTarget::EnumRuntimes 方法
ms.date: 03/30/2017
api_name:
- ICoreClrDebugTarget.EnumRuntimes
api_location:
- mscordbi_macx86.dll
api_type:
- COM
f1_keywords:
- ICoreClrDebugTarget::EnumRuntimes
helpviewer_keywords:
- remote debugging API [Silverlight]
- ICorClrDebugTarget::EnumRuntimes method [Silverlight debugging]
- EnumRuntimes method, ICoreClrDebugTarget interface [Silverlight debugging]
- Silverlight, remote debugging
ms.assetid: 316df866-442d-40cc-b049-45e8adcb65d1
topic_type:
- apiref
ms.openlocfilehash: 093f49508e8e96a4003f1aab8eed59e2fd196ba9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679268"
---
# <a name="icoreclrdebugtargetenumruntimes-method"></a>ICoreClrDebugTarget::EnumRuntimes 方法

枚举在远程计算机上运行的指定进程中的公共语言运行时 (CLR)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumRuntimes (  
      [in] DWORD       dwInternalProcessID,  
      [out] DWORD*     pcRuntimes,  
      [out] CoreClrDebugRuntimeInfo**    ppRuntimes  
    );  
```  
  
## <a name="parameters"></a>参数  

 `dwInternalProcessID`  
 [in] 要枚举运行时的进程的内部进程 ID。 这将 `m_dwInternalID` 来自相应的 [CoreClrDebugProcInfo](coreclrdebugprocinfo-structure.md)。  
  
 `pcRuntimes`  
 [out] `ppRuntimes` 中返回的运行时的数量。 此值可为 0（零）。  
  
 `ppRuntimes`  
 弄 [CoreClrDebugRuntimeInfo](coreclrdebugruntimeinfo-structure.md) 结构的数组，这些结构表示远程目标进程中加载的运行时。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 成功。  
  
 S_FALSE  
 `dwInternalProcessID` 不匹配计算机上运行的任何进程，可能因为进程已终止。 `pcRuntimes` 和 `ppRuntimes` 将为 null。  
  
 E_OUTOFMEMORY  
 无法为 `ppRuntimes` 分配足够的内存。  
  
 E_FAIL（或其他 E_ 返回代码）  
 其他故障。  
  
## <a name="remarks"></a>注解  

 若要释放此方法分配的内存，请调用 [ICoreClrDebugTarget：： FreeMemory](icoreclrdebugtarget-freememory-method.md) 方法。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CoreClrRemoteDebuggingInterfaces  
  
 **库：** mscordbi_macx86.dll  
  
 **.NET Framework 版本：** 3.5 SP1  
  
## <a name="see-also"></a>另请参阅

- [ICoreClrDebugTarget 接口](icoreclrdebugtarget-interface.md)
