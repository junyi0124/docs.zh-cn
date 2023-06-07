---
title: CorExitProcess 函数
ms.date: 03/30/2017
api_name:
- CorExitProcess
api_location:
- mscoree.dll
- clr.dll
- mscorwks.dll
- mscoreei.dll
- mscorsvr.dll
api_type:
- DLLExport
f1_keywords:
- CorExitProcess
helpviewer_keywords:
- CorExitProcess function [.NET Framework hosting]
ms.assetid: a5cab4c6-990e-47f3-8798-cf422b791015
topic_type:
- apiref
ms.openlocfilehash: f6d8114732a3b7c15d0a0258a28a362d661b030a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95673617"
---
# <a name="corexitprocess-function"></a>CorExitProcess 函数

关闭当前的非托管进程。  
  
 此函数已在 .NET Framework 4 中弃用。 改为使用 [ICLRMetaHost：： ExitProcess](iclrmetahost-exitprocess-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
void STDMETHODCALLTYPE CorExitProcess (
  int  exitCode  
);  
```  
  
## <a name="parameters"></a>参数  

 `exitCode`  
 一个整数，指定进程退出代码。  
  
## <a name="remarks"></a>注解  
  
> [!NOTE]
> 从 .NET Framework 4 开始， `CorExitProcess` 退出进程中的每个已启动的运行时，而不只是旧 api 所绑定到的运行时。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
