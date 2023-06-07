---
title: CoInitializeEE 函数
ms.date: 03/30/2017
api_name:
- CoInitializeEE
api_location:
- mscoree.dll
- mscorsvr.dll
api_type:
- DLLExport
f1_keywords:
- CoInitializeEE
helpviewer_keywords:
- CoInitializeEE function [.NET Framework hosting]
ms.assetid: 7e42a928-5068-4ba6-b8c3-806551a01fa8
topic_type:
- apiref
ms.openlocfilehash: 707e182e62f4a7b7b813e6b288c6825b0d3d2eab
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731743"
---
# <a name="coinitializeee-function"></a>CoInitializeEE 函数

确保将公共语言运行时执行引擎加载到进程中。 此函数在 .NET Framework 4 中已弃用。 改为使用 [ICLRRuntimeHost：： Start](iclrruntimehost-start-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CoInitializeEE (  
   [in] DWORD fFlags  
);  
```  
  
## <a name="parameters"></a>参数  

 `fFlags`  
 中 [COINITIEE](../metadata/coinitiee-enumeration.md) 枚举常量之一。  
  
## <a name="return-value"></a>返回值  

 此方法返回 Winerror.h 中定义的标准 COM 错误代码，以及下表中的值。  
  
|返回代码|说明|  
|-----------------|-----------------|  
|S_OK|已成功加载执行引擎。|  
|S_FALSE|已经加载了执行引擎。|  
|E_FAIL|未能加载执行引擎。|  
  
## <a name="remarks"></a>注解  

 此方法加载尚未加载的执行引擎。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据全局静态函数](../metadata/metadata-global-static-functions.md)
