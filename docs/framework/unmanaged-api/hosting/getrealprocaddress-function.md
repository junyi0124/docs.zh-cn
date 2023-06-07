---
title: GetRealProcAddress 函数
ms.date: 03/30/2017
api_name:
- GetRealProcAddress
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- GetRealProcAddress
helpviewer_keywords:
- GetRealProcAddress function [.NET Framework hosting]
ms.assetid: f1f2fab1-400b-488f-95f2-d49c4fca3556
topic_type:
- apiref
ms.openlocfilehash: d48106fca6008955409581ad9ac202aebe785cb4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733217"
---
# <a name="getrealprocaddress-function"></a>GetRealProcAddress 函数

获取从 (CLR) 的公共语言运行时的最新安装版本导出的指定函数的地址。  
  
 此函数已在 .NET Framework 4 中弃用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRealProcAddress (  
    [in]  LPCSTR  pwszProcName,
    [out] VOID  **ppv  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwszProcName`  
 中函数的名称。  
  
 `ppv`  
 弄接收指向函数地址的指针的位置。  
  
## <a name="return-value"></a>返回值  

 除了在 CorError 中定义的以下值外，此方法还会返回 (COM) 错误代码的标准组件对象模型，如 Winerror.h 中所定义。  
  
|返回代码|说明|  
|-----------------|-----------------|  
|S_OK|该方法已成功完成。|  
|E_POINTER|`ppv` 无效。|  
|CLR_E_SHIM_RUNTIMEEXPORT|不从运行时导出函数。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
