---
title: StrongNameFreeBuffer 函数
ms.date: 03/30/2017
api_name:
- StrongNameFreeBuffer
api_location:
- mscoree.dll
- mscorsn.dll
- clr.dll
- mscorwks.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameFreeBuffer
helpviewer_keywords:
- StrongNameFreeBuffer function [.NET Framework strong naming]
ms.assetid: eda21ecf-4734-4f92-aaba-9f34884385db
topic_type:
- apiref
ms.openlocfilehash: d93bda046a79dbdec2195eee48fefc1e5538b2e4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732250"
---
# <a name="strongnamefreebuffer-function"></a>StrongNameFreeBuffer 函数

释放先前调用 [StrongNameGetPublicKey](strongnamegetpublickey-function.md)、[StrongNameTokenFromPublicKey](strongnametokenfrompublickey-function.md) 或 [StrongNameSignatureGeneration](strongnamesignaturegeneration-function.md) 强名称函数分配的内存。  
  
 此函数已弃用。 改为使用 [ICLRStrongName：： StrongNameFreeBuffer](../hosting/iclrstrongname-strongnamefreebuffer-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
VOID StrongNameFreeBuffer (
   [in] BYTE   *pbMemory  
);  
```  
  
## <a name="parameters"></a>参数  

 `pbMemory`  
 中指向要释放的内存的指针。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameFreeBuffer 方法](../hosting/iclrstrongname-strongnamefreebuffer-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
