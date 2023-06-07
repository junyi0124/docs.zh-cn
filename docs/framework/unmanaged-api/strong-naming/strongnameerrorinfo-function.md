---
title: StrongNameErrorInfo 函数
ms.date: 03/30/2017
api_name:
- StrongNameErrorInfo
api_location:
- mscoree.dll
- mscorsn.dll
- clr.dll
- mscorwks.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameErrorInfo
helpviewer_keywords:
- StrongNameErrorInfo function [.NET Framework strong naming]
ms.assetid: e91bf8c3-7c26-4732-938e-2e5b04abfc99
topic_type:
- apiref
ms.openlocfilehash: 90abfcd573795ae529714e21b13f90d6e15c7dad
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732263"
---
# <a name="strongnameerrorinfo-function"></a>StrongNameErrorInfo 函数

获取由其中一个强名称函数引发的最后一个错误代码。  
  
 此函数已弃用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameErrorInfo ();
```  
  
## <a name="return-value"></a>返回值  

 由一个强名称函数设置的最后一个 COM 错误代码。  
  
## <a name="remarks"></a>注解  

 大多数强名称方法返回成功完成的简单 `true` 或 `false` 指示。 使用 `StrongNameErrorInfo` 函数检索 HRESULT，该 HRESULT 指定强名称函数生成的最后一个错误。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
