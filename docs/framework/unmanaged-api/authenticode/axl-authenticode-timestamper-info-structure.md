---
title: AXL_AUTHENTICODE_TIMESTAMPER_INFO 结构
ms.date: 03/30/2017
ms.assetid: 89e41a81-0f41-45ad-8f20-a120e4ff24fb
ms.openlocfilehash: b6852519da6cf4e12669aa2efa24862053adbc03
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674237"
---
# <a name="axl_authenticode_timestamper_info-structure"></a>AXL_AUTHENTICODE_TIMESTAMPER_INFO 结构

定义验证码时间戳签署人的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct _AXL_AUTHENTICODE_SIGNER_INFO {  
    DWORD cbSize;  
    HRESULT dwError;  
    ALG_ID algHash;  
    FILETIME ftTimestamp  
    PCCERT_CHAIN_CONTEXT pChainContext;  
} AXL_AUTHENTICODE_TIMESTAMPER_INFO, * PAXL_AUTHENTICODE_TIMESTAMPER_INFO;  
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`cbSize`|此结构的大小。|  
|`dwError`|错误代码。|  
|`algHash`|哈希算法。|  
|`ftTimestamp`|时间戳的时间。|  
|`pChainContext`|时间戳签署人的链上下文。  请参阅 [CERT_CONTEXT](/windows/win32/api/wincrypt/ns-wincrypt-cert_context) 结构。|  
  
## <a name="see-also"></a>另请参阅

- [验证码](index.md)
