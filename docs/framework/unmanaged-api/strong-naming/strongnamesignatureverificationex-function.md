---
title: StrongNameSignatureVerificationEx 函数
ms.date: 03/30/2017
api_name:
- StrongNameSignatureVerificationEx
api_location:
- mscoree.dll
- mscorwks.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameSignatureVerificationEx
helpviewer_keywords:
- StrongNameSignatureVerificationEx function [.NET Framework strong naming]
ms.assetid: cfe4b634-18bf-44b8-9773-d94fb7e8a480
topic_type:
- apiref
ms.openlocfilehash: 27417c379411e242c48d6d9b0c99de833f7ede8a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95719263"
---
# <a name="strongnamesignatureverificationex-function"></a>StrongNameSignatureVerificationEx 函数

获取一个值，该值指示提供的路径中的程序集清单是否包含强名称签名。  
  
 此函数已弃用。 改为使用 [ICLRStrongName：： StrongNameSignatureVerificationEx](../hosting/iclrstrongname-strongnamesignatureverificationex-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
BOOLEAN StrongNameSignatureVerificationEx (  
    [in]  LPCWSTR   wszFilePath,  
    [in]  BOOLEAN   fForceVerification,  
    [out] BOOLEAN   *pfWasVerified  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszFilePath`  
 中要验证的程序集的可移植可执行文件的路径 ( .exe 或 .dll) 文件。  
  
 `fForceVerification`  
 [in] `true` 若要执行验证，即使需要重写注册表设置，否则为 `false` 。  
  
 `pfWasVerified`  
 [out] `true` 如果验证了强名称签名，则为;否则为 `false` 。 `pfWasVerified``false`如果验证因为注册表设置而成功，则也会设置为。  
  
## <a name="return-value"></a>返回值  

 `true` 如果验证成功，则为;否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 `StrongNameSignatureVerificationEx` 提供与 [StrongNameSignatureVerification](strongnamesignatureverification-function.md) 函数相似的功能。 但是，的第二个输入参数和输出参数 `StrongNameSignatureVerificationEx` 的类型为， `BOOLEAN` 而不是 `DWORD` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 mscoree.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameSignatureVerificationEx 方法](../hosting/iclrstrongname-strongnamesignatureverificationex-method.md)
- [StrongNameSignatureVerification 方法](../hosting/iclrstrongname-strongnamesignatureverification-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
