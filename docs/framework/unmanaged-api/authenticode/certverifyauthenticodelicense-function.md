---
title: CertVerifyAuthenticodeLicense 函数
ms.date: 03/30/2017
api_name:
- CertVerifyAuthenticodeLicense
api_location:
- clr.dll
api_type:
- DLLExport
ms.assetid: 00118de7-33c6-41c4-8e1f-5d5e35e0da83
ms.openlocfilehash: 388814d1c63f048c0aa231a1d0058a390cba9493
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674055"
---
# <a name="certverifyauthenticodelicense-function"></a>CertVerifyAuthenticodeLicense 函数

验证验证码 XrML 许可证的有效性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CertVerifyAuthenticodeLicense (  
    [in]   PCRYPT_DATA_BLOB                   pLicenseBlob,  
    [in]   OPTIONAL DWORD                     dwFlags,  
    [out]  PAXL_AUTHENTICODE_SIGNER_INFO      pSignerInfo,  
    [out]  PAXL_AUTHENTICODE_TIMESTAMPER_INFO pTimestamperInfo  
);  
```  
  
## <a name="parameters"></a>参数  

 `pLicenseBlob`  
 [in] 要验证的验证码 XrML 许可证。  
  
 请参阅 [CRYPTOAPI_BLOB](/windows/win32/api/dpapi/ns-dpapi-crypt_integer_blob) 结构。  
  
 `dwFlags`  
 [in] 可选。 以下值的组合：  
  
- AXL_REVOCATION_NO_CHECK  
  
- AXL_REVOCATION_CHECK_END_CERT_ONLY  
  
- AXL_REVOCATION_CHECK_ENTIRE_CHAIN  
  
- AXL_URL_CACHE_ONLY_RETRIEVAL  
  
- AXL_LIFETIME_SIGNING  
  
- AXL_TRUST_MICROSOFT_ROOT_ONLY  
  
 `pSignerInfo`  
 [out] 接收签署人的信息。 如果许可证未进行签名，则 `dwError` 将设置为 TRUST_E_NOSIGNATURE。 调用方负责在使用后使用 [CertFreeAuthenticodeSignerInfo](certfreeauthenticodesignerinfo-function.md) 函数释放资源。  
  
 请参阅 [AXL_AUTHENTICODE_SIGNER_INFO 结构](axl-authenticode-signer-info-structure.md)。  
  
 `pTimestamperInfo`  
 [out] 接收时间戳签署人的信息（如果有）。 如果未对许可证签署时间戳，则 `dwError` 将设置为 TRUST_E_NOSIGNATURE。 调用方负责在使用后使用 [CertFreeAuthenticodeTimestamperInfo](certfreeauthenticodetimestamperinfo-function.md) 函数释放资源。  
  
 请参阅 [AXL_AUTHENTICODE_TIMESTAMPER_INFO 结构](axl-authenticode-timestamper-info-structure.md)。  
  
## <a name="return-value"></a>返回值  

 如果成功，则返回 `S_OK`。 否则，返回错误代码。  
  
## <a name="see-also"></a>另请参阅

- [验证码](index.md)
- [GetHashFromHandle 方法](../hosting/iclrstrongname-gethashfromhandle-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
