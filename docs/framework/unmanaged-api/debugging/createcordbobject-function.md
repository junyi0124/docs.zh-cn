---
title: CreateCordbObject 函数
ms.date: 03/30/2017
api_name:
- CreateCoredbObject
api_location:
- mscordbi_macx86.dll
api_type:
- COM
f1_keywords:
- CreateCordbObject
helpviewer_keywords:
- remote debugging API [Silverlight]
- CreateCordbObject function
- Silverlight, remote debugging
ms.assetid: b259821d-4fa7-464d-85cf-304dfffc8089
topic_type:
- apiref
ms.openlocfilehash: eccdfcb60b2d2b5d652ccac948c01c16e7cb828d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725971"
---
# <a name="createcordbobject-function"></a>CreateCordbObject 函数

创建一个 ([ICorDebug](icordebug-interface.md)) 的调试器接口，该接口提供用于在远程进程中实例化托管调试会话的功能。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CordbCreateObject (  
       [in]  int         iDebuggerVersion,
       [out] IUnknown**  ppCordb  
);  
```  
  
## <a name="parameters"></a>参数  

 `iDebuggerVersion`  
 [in] 目标进程的调试器版本。 此参数必须为 CorDebugVersion_2_0 以用于远程调试。  
  
 `ppCordb`  
 弄指向对象的指针的指针，该对象将被强制转换为 [ICorDebug](icordebug-interface.md) 接口并返回。  
  
## <a name="return-value"></a>返回值  

 S_OK  
 已成功确定该进程中的 CLR 的数目，并已正确填写相应的句柄数组和路径数组。  
  
 E_INVALIDARG  
 `ppCordb` 为 null，或 `iDebuggerVersion` 不是 CorDebugVersion_2_0。  
  
 E_OUTOFMEMORY  
 无法为 `ppCordb` 分配足够的内存  
  
 E_FAIL（或其他 E_ 返回代码）  
 其他故障。  
  
## <a name="remarks"></a>注解  

 中返回的 [ICorDebug](icordebug-interface.md) 接口 `ppCordb` 是所有托管调试服务的顶级调试接口。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** CoreClrRemoteDebuggingInterfaces  
  
 **库：** mscordbi_macx86.dll  
  
 **.NET Framework 版本：** 3.5 SP1
