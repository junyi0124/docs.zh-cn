---
title: CertFreeAuthenticodeSignerInfo 函数
ms.date: 03/30/2017
api_name:
- CertFreeAuthenticodeSignerInfo
api_location:
- clr.dll
api_type:
- DLLExport
ms.assetid: 8029633c-b6e4-4665-a7c2-89607c3247ef
ms.openlocfilehash: dc6bb5a137a50ec07f89f292e5d9beac4349c3c7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674172"
---
# <a name="certfreeauthenticodesignerinfo-function"></a>CertFreeAuthenticodeSignerInfo 函数

释放为 [AXL_AUTHENTICODE_SIGNER_INFO](axl-authenticode-signer-info-structure.md) 结构分配的资源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT CertFreeAuthenticodeSignerInfo (  
    [in, out]  PAXL_AUTHENTICODE_SIGNER_INFO   pSignerInfo);  
```  
  
## <a name="parameters"></a>参数  

 `pSignerInfo`  
 [in, out] 要释放的签署人信息。 请参阅 [AXL_AUTHENTICODE_SIGNER_INFO](axl-authenticode-signer-info-structure.md) 结构。  
  
## <a name="return-value"></a>返回值  

 如果此函数成功，则返回 `S_OK`。 否则，返回错误代码。  
  
## <a name="see-also"></a>另请参阅

- [验证码](index.md)
