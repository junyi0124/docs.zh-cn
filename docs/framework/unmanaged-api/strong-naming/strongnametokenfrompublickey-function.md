---
title: StrongNameTokenFromPublicKey 函数
ms.date: 03/30/2017
api_name:
- StrongNameTokenFromPublicKey
api_location:
- mscoree.dll
- mscorsn.dll
- clr.dll
- mscorwks.dll
- mscoreei.dll
api_type:
- COM
f1_keywords:
- StrongNameTokenFromPublicKey
helpviewer_keywords:
- StrongNameTokenFromPublicKey function [.NET Framework strong naming]
ms.assetid: 997e9e57-abb2-4217-bf20-1df621a75add
topic_type:
- apiref
ms.openlocfilehash: 89556cf0e1ef65c35278a526e10fc791063ea2c6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95708343"
---
# <a name="strongnametokenfrompublickey-function"></a>StrongNameTokenFromPublicKey 函数

获取表示公钥的令牌。 强名称标记是公钥的缩写形式。  
  
 此函数已弃用。 改为使用 [ICLRStrongName：： StrongNameTokenFromPublicKey](../hosting/iclrstrongname-strongnametokenfrompublickey-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
BOOLEANStrongNameTokenFromPublicKey (
    [in]  BYTE    *pbPublicKeyBlob,  
    [in]  ULONG   cbPublicKeyBlob,  
    [out] BYTE    **ppbStrongNameToken,  
    [out] ULONG   *pcbStrongNameToken  
);  
```  
  
## <a name="parameters"></a>参数  

 `pbPublicKeyBlob`  
 中 [PublicKeyBlob](publickeyblob-structure.md) 类型的结构，它包含用于生成强名称签名的密钥对的公共部分。  
  
 `cbPublicKeyBlob`  
 中的大小（以字节为单位） `pbPublicKeyBlob` 。  
  
 `ppbStrongNameToken`  
 弄与传入的密钥对应的强名称标记 `pbPublicKeyBlob` 。 公共语言运行时分配要在其中返回标记的内存。 调用方必须通过使用 [StrongNameFreeBuffer](strongnamefreebuffer-function.md) 函数来释放此内存。  
  
 `pcbStrongNameToken`  
 弄返回的强名称标记的大小（以字节为单位）。  
  
## <a name="return-value"></a>返回值  

 `true` 成功完成时;否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 强名称标记是在元数据中存储密钥信息时用于节省空间的公钥的缩写形式。 具体而言，强名称令牌在程序集引用中用于引用依赖程序集。  
  
 如果 `StrongNameTokenFromPublicKey` 函数未成功完成，请调用 [StrongNameErrorInfo](strongnameerrorinfo-function.md) 函数来检索上次生成的错误。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 mscoree.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameTokenFromPublicKey 方法](../hosting/iclrstrongname-strongnametokenfrompublickey-method.md)
- [StrongNameGetPublicKey 方法](../hosting/iclrstrongname-strongnamegetpublickey-method.md)
- [PublicKeyBlob 结构](publickeyblob-structure.md)
