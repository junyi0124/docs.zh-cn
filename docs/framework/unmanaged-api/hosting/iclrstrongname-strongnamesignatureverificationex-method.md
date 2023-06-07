---
title: ICLRStrongName::StrongNameSignatureVerificationEx 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameSignatureVerificationEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameSignatureVerificationEx
helpviewer_keywords:
- StrongNameSignatureVerificationEx method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::StrongNameSignatureVerificationEx method [.NET Framework hosting]
ms.assetid: dbd2f662-208b-4174-b301-5c99af91040f
topic_type:
- apiref
ms.openlocfilehash: 9caeb72c57260012ae2b459950bf19d907da1d98
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671546"
---
# <a name="iclrstrongnamestrongnamesignatureverificationex-method"></a>ICLRStrongName::StrongNameSignatureVerificationEx 方法

获取一个值，该值指示所提供的路径处的程序集清单是否包含强名称签名。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameSignatureVerificationEx (  
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

 `S_OK` 如果验证成功，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 [ICLRStrongName：： StrongNameSignatureVerificationEx](iclrstrongname-strongnamesignatureverificationex-method.md)方法提供类似于[ICLRStrongName：： StrongNameSignatureVerification](iclrstrongname-strongnamesignatureverification-method.md)方法的功能。 但是， [ICLRStrongName：： StrongNameSignatureVerificationEx](iclrstrongname-strongnamesignatureverificationex-method.md) 的第二个输入参数和输出参数的类型为， `BOOLEAN` 而不是 `DWORD` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameSignatureVerification 方法](iclrstrongname-strongnamesignatureverification-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
