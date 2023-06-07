---
title: CreateCoreClrDebugTarget 函数
ms.date: 03/30/2017
api_name:
- CreateCorClrDebugTarget
api_location:
- mscordbi_macx86.dll
api_type:
- COM
f1_keywords:
- CreateCoreClrDebugTarget
helpviewer_keywords:
- remote debugging API [Silverlight]
- Silverlight, remote debugging
- CreateCoreClrDebugTarget function
ms.assetid: 1cf4ca8e-d9bb-4633-9adf-5e24315bf87a
topic_type:
- apiref
ms.openlocfilehash: f0188facf0b7d33e6e1ecc12921a139165f777a1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95686626"
---
# <a name="createcoreclrdebugtarget-function"></a>CreateCoreClrDebugTarget 函数

创建与远程计算机上运行的调试器代理的连接，并返回一个 [ICoreClrDebugTarget](icoreclrdebugtarget-interface.md) 对象，该对象可用于查询远程计算机上正在运行的进程和已加载的运行时。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CreateCoreClrDebugTarget (  
       [in]  DWORD    dwAddress,
       [out] ICoreClrDebugTarget**     ppTarget  
);  
```  
  
## <a name="parameters"></a>参数  

 `dwAddress`  
 [in] 远程目标计算机的 IPv4 地址。  
  
 `ppTarget`  
 弄指向将创建的 [ICoreClrDebugTarget](icoreclrdebugtarget-interface.md) 对象的指针的指针。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 已成功确定该进程中的 CLR 的数目，并已正确填写相应的句柄数组和路径数组。  
  
 E_OUTOFMEMORY  
 无法为 `ppTarget` 分配足够的内存。  
  
 E_FAIL（或其他 E_ 返回代码）  
 其他故障。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CoreClrRemoteDebuggingInterfaces  
  
 **库：** mscordbi_macx86.dll  
  
 **.NET Framework 版本：** 3.5 SP1
