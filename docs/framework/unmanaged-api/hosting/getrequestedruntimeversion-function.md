---
title: GetRequestedRuntimeVersion 函数
ms.date: 03/30/2017
api_name:
- GetRequestedRuntimeVersion
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- GetRequestedRuntimeVersion
helpviewer_keywords:
- GetRequestedRuntimeVersion function [.NET Framework hosting]
ms.assetid: 82f596a4-483d-4509-b0c5-a84c53c3da1b
topic_type:
- apiref
ms.openlocfilehash: 6c16b02a5ae323ba80d44937f322810022dfa9f5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95711632"
---
# <a name="getrequestedruntimeversion-function"></a>GetRequestedRuntimeVersion 函数

获取指定应用程序 (CLR) 的公共语言运行时的版本号。 如果未安装该版本，则获取在请求版本之前安装的最新版本。  
  
 此函数已在 .NET Framework 4 中弃用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetRequestedRuntimeVersion (  
    [in]  LPWSTR  pExe,
    [out] LPWSTR  pVersion,
    [in]  DWORD   cchBuffer,
    [out] DWORD  *pdwLength  
);  
```  
  
## <a name="parameters"></a>参数  

 `pExe`  
 中应用程序的名称。  
  
 `pVersion`  
 弄一个缓冲区，其中包含成功完成后的版本号字符串。  
  
 `cchBuffer`  
 中版本缓冲区的长度。  
  
 `pdwLength`  
 弄指向版本号字符串长度的指针。  
  
## <a name="return-value"></a>返回值  

 除以下值外，此方法还 (COM) 错误代码（如 Winerror.h 中所定义）返回标准组件对象模型。  
  
|返回代码|说明|  
|-----------------|-----------------|  
|S_OK|该方法已成功完成。|  
|ERROR_INSUFFICIENT_BUFFER|版本缓冲区不够大，无法存储版本字符串。|  
|E_POINTER|`pdwLength` 为 null。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Mscoree.dll  
  
 **库：** MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetRequestedRuntimeInfo 函数](getrequestedruntimeinfo-function.md)
- [GetVersionFromProcess 函数](getversionfromprocess-function.md)
- [弃用的 CLR 承载函数](deprecated-clr-hosting-functions.md)
