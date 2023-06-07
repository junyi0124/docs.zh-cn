---
title: _AxlRSAKeyValueToPublicKeyToken 函数
ms.date: 03/30/2017
api_name:
- _AxlRSAKeyValueToPublicKeyToken
api_location:
- clr.dll
api_type:
- DLLExport
ms.assetid: d60f19fe-7bec-47ba-b60e-ba9ce66abf8c
ms.openlocfilehash: 5c1e2bfc7fd55e807af68744e28faa473daea772
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674211"
---
# <a name="_axlrsakeyvaluetopublickeytoken-function"></a>\_AxlRSAKeyValueToPublicKeyToken 函数

将模数和指数转换为强名称公钥标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT _AxlRSAKeyValueToPublicKeyToken (  
    [in]  PCRYPT_DATA_BLOB pModulusBlob,  
    [in]  PCRYPT_DATA_BLOB pExponentBlob,  
    [out] LPWSTR           *ppwszPublicKeyToken  
);  
```  
  
## <a name="parameters"></a>参数  

 `pModulusBlob`  
 中Base64 编码的模数 blob 从 \<Modulus> 元素)  (。  请参阅 [CRYPTOAPI_BLOB](/windows/win32/api/dpapi/ns-dpapi-crypt_integer_blob) 结构。  
  
 `pExponentBlob`  
 中Base64 编码的指数 blob 从 \<Exponent> 元素)  (。 请参阅 [CRYPTOAPI_BLOB](/windows/win32/api/dpapi/ns-dpapi-crypt_integer_blob) 结构。  
  
 `ppwszPublicKeyToken`  
 [out] 指向 WCHAR * 的指针，用于接收十六进制编码的公钥标记。  
  
## <a name="return-value"></a>返回值  

 如果此函数成功，则返回 `S_OK`。 否则，返回错误代码。  
  
## <a name="see-also"></a>另请参阅

- [验证码](index.md)
