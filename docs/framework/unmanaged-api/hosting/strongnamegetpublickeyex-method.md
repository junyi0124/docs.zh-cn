---
title: StrongNameGetPublicKeyEx 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName2.StrongNameGetPublicKeyEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- StrongNameGetPublicKeyEx
helpviewer_keywords:
- StrongNameGetPublicKeyEx method, ICLRStrongName2 interface [.NET Framework hosting]
- ICLRStrongName2::StrongNameGetPublicKeyEx method [.NET Framework hosting]
ms.assetid: 63d8260c-fb32-4f8f-a357-768afd570f68
topic_type:
- apiref
ms.openlocfilehash: 8cc28d9ccd40c65d225a96b269562c9d3dfa2124
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729884"
---
# <a name="strongnamegetpublickeyex-method"></a>StrongNameGetPublicKeyEx 方法

获取公钥/私钥对中的公钥，并指定哈希算法和签名算法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameGetPublicKey (
    [in]  LPCWSTR   pwzKeyContainer,  
    [in]  BYTE      *pbKeyBlob,  
    [in]  ULONG     cbKeyBlob,  
    [out] BYTE      **ppbPublicKeyBlob,  
    [out] ULONG     *pcbPublicKeyBlob  
    [in]  ULONG     uHashAlgId,  
    [in]  ULONG     uReserved,  
);  
```  
  
## <a name="parameters"></a>参数  

 `pwzKeyContainer`  
 中包含公钥/私钥对的密钥容器的名称。 如果 `pbKeyBlob` 为 null，则 `szKeyContainer` 必须在加密服务提供程序 (CSP) 中指定有效容器。 在这种情况下，该 `StrongNameGetPublicKeyEx` 方法将从存储在容器中的密钥对中提取公钥。  
  
 如果不为 `pbKeyBlob` null，则假定密钥对包含在关键的二进制大型对象 (BLOB) 中。  
  
 密钥必须是1024位 Rivest-Rivest-shamir-adleman (RSA) 签名密钥。 此时不支持其他类型的密钥。  
  
 `pbKeyBlob`  
 中指向公钥/私钥对的指针。 此对采用 Win32 函数创建的格式 `CryptExportKey` 。 如果 `pbKeyBlob` 为 null，则假定指定的密钥容器 `szKeyContainer` 包含密钥对。  
  
 `cbKeyBlob`  
 中的大小（以字节为单位） `pbKeyBlob` 。  
  
 `ppbPublicKeyBlob`  
 弄返回的公钥 BLOB。 `ppbPublicKeyBlob`参数由公共语言运行时分配并返回给调用方。 调用方必须使用 [ICLRStrongName：： StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) 方法释放内存。  
  
 `pcbPublicKeyBlob`  
 弄返回的公钥 BLOB 的大小。  
  
 `uHashAlgId`  
 中程序集哈希算法。 有关接受值的列表，请参阅 "备注" 部分。  
  
 `uReserved`  
 中保留以供将来使用;默认值为 null。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 公钥包含在 [PublicKeyBlob](../strong-naming/publickeyblob-structure.md) 结构中。  
  
## <a name="remarks"></a>注解  

 下表显示了该参数的接受值集 `uHashAlgId` 。  
  
|名称|Value|  
|----------|-----------|  
|无|0|  
|SHA-1|0x8004|  
|SHA-256|0x800c|  
|SHA-384|0x800d|  
|SHA-512|0x800e|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameTokenFromPublicKey 方法](iclrstrongname-strongnametokenfrompublickey-method.md)
- [PublicKeyBlob 结构](../strong-naming/publickeyblob-structure.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
- [StrongNameGetPublicKey 方法](iclrstrongname-strongnamegetpublickey-method.md)
