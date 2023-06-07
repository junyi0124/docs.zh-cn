---
title: CertTimestampAuthenticodeLicense 函数
ms.date: 03/30/2017
api_name:
- CertTimestampAuthenticodeLicense
api_location:
- clr.dll
api_type:
- DLLExport
ms.assetid: d468325a-21c5-43ce-8567-84e342b22308
ms.openlocfilehash: fc1a99572406a38aee8133d6435134b78a134175
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674094"
---
# <a name="certtimestampauthenticodelicense-function"></a>CertTimestampAuthenticodeLicense 函数

为验证码 XrML 许可证添加时间戳。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CertTimestampAuthenticodeLicense (  
    [in]  PCRYPT_DATA_BLOB   pSignedLicenseBlob,  
    [in]  LPCWSTR            pwszTimestampURI,  
    [out] PCRYPT_DATA_BLOB   pTimestampSignatureBlob  
);  
```  
  
## <a name="parameters"></a>参数  

 `pSignedLicenseBlob`  
 [in] 要添加时间戳的已签名验证码 XrML 许可证。 请参阅 [CRYPTOAPI_BLOB](/windows/win32/api/dpapi/ns-dpapi-crypt_integer_blob) 结构。  
  
 `pwszTimestampURI`  
 [in] 时间戳服务器的 URI。  
  
 `pTimestampSignatureBlob`  
 [out] 指向 CRYPT_DATA_BLOB 的指针，用于接收 base64 编码的时间戳签名。 调用方负责 `pTimestampSignatureBlob` -> `pbData` `HepFree()` 在使用后释放。 请参阅 [CRYPTOAPI_BLOB](/windows/win32/api/dpapi/ns-dpapi-crypt_integer_blob) 结构。  
  
## <a name="remarks"></a>注解  

 时间戳签名实际上是一条 PKCS #7 SignedData 消息，其内容是许可证签名中 SignatureValue 的二进制格式。 基本上，它充当许可证的副署。  
  
## <a name="return-value"></a>返回值  

 如果此函数成功，则返回 `S_OK`。 否则，返回错误代码。  
  
## <a name="see-also"></a>另请参阅

- [验证码](index.md)
