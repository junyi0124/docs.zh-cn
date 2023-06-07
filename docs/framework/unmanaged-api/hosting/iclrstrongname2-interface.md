---
title: ICLRStrongName2 接口
ms.date: 03/30/2017
api_name:
- ICLRStrongName2
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName2
helpviewer_keywords:
- ICLRStrongName2 interface [.NET Framework hosting]
ms.assetid: 9f098a74-201e-4628-a468-8dee9ab99b4a
topic_type:
- apiref
ms.openlocfilehash: df570f32d694ec21e9642e75b4965e169afaccfc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677643"
---
# <a name="iclrstrongname2-interface"></a>ICLRStrongName2 接口

提供使用 SHA-256、SHA-384 和 SHA-512)  (sha-1 安全哈希算法组创建强名称的功能。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[StrongNameGetPublicKeyEx 方法](strongnamegetpublickeyex-method.md)|获取公钥/私钥对中的公钥，并指定哈希算法和签名算法。|  
|[StrongNameSignatureVerificationEx2 方法](strongnamesignatureverificationex2-method.md)|验证强名称程序集的签名，并提供从 ECMA 密钥到实际密钥的映射。|  
  
## <a name="remarks"></a>备注  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]
