---
title: _AxlPublicKeyBlobToPublicKeyToken 函数
ms.date: 03/30/2017
api_name:
- _AxlPublicKeyBlobToPublicKeyToken
api_location:
- clr.dll
api_type:
- DLLExport
ms.assetid: 2d92a746-d68c-4f53-a16e-727f071a2d80
ms.openlocfilehash: 989e99198efd1519f607a2e3164ff4de584e88af
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95679879"
---
# <a name="_axlpublickeyblobtopublickeytoken-function"></a>\_AxlPublicKeyBlobToPublicKeyToken 函数

从 CSP PUBLICKEYBLOB 格式计算强名称公钥标记。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT _AxlPublicKeyBlobToPublicKeyToken (  
    [in]  PCCERT_CHAIN_CONTEXT   pCspPublicKeyBlob,  
    [out] LPWSTR                 *ppwszPublicKeyToken  
);  
```  
  
## <a name="parameters"></a>参数  

 `pCspPublicKeyBlob`  
 [in] CSP 公钥 Blob。  
  
 `ppwszPublicKeyHash`  
 [out] 指向 WCHAR * 的指针，用于接收十六进制编码的公钥哈希。  
  
## <a name="return-value"></a>返回值  

 如果函数成功，则为 `S_OK`；否则为 `S_FALSE`。  
  
## <a name="see-also"></a>另请参阅

- [验证码](index.md)
