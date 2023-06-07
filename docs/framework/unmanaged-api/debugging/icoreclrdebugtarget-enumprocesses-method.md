---
title: ICoreClrDebugTarget::EnumProcesses 方法
ms.date: 03/30/2017
api_name:
- ICoreClrDebugTarget.EnumProcesses
api_location:
- mscordbi_macx86.dll
api_type:
- COM
f1_keywords:
- ICoreClrDebugTarget::EnumProcesses
helpviewer_keywords:
- remote debugging API [Silverlight]
- EnumProcesses method, ICorClrDebugTarget interface [Silverlight debugging]
- ICorClrDebugTarget::EnumProcesses method [Silverlight debugging]
- Silverlight, remote debugging
ms.assetid: e00fd477-4f49-43d3-bd0e-3094824b1136
topic_type:
- apiref
ms.openlocfilehash: 7e0219ae0d7d474812865f01e4e2fcfe2e4da991
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679359"
---
# <a name="icoreclrdebugtargetenumprocesses-method"></a>ICoreClrDebugTarget::EnumProcesses 方法

枚举远程计算机上运行的进程。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT EnumProcesses (  
       [out]  DWORD*                  pcProcs,
       [out]  CoreClrDebugProcInfo**  ppProcs  
);  
```  
  
## <a name="parameters"></a>参数  

 `pcProcs`  
 [out] `ppProcs` 中返回的进程数。 此值可为 0（零）。  
  
 `ppProcs`  
 弄 [CoreClrDebugProcInfo](coreclrdebugprocinfo-structure.md) 结构的数组，这些结构表示远程计算机上运行的进程。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 成功。  
  
 E_OUTOFMEMORY  
 无法为 `ppProcs` 分配足够的内存。  
  
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
