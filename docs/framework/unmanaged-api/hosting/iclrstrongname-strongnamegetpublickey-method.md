---
title: ICLRStrongName::StrongNameGetPublicKey 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameGetPublicKey
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameGetPublicKey
helpviewer_keywords:
- StrongNameGetPublicKey method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::StrongNameGetPublicKey method [.NET Framework hosting]
ms.assetid: a31dcaa9-a404-4c1d-8cc7-081827c52935
topic_type:
- apiref
ms.openlocfilehash: 5bd314b2b63474d2a1d159f74564e2d4ca13aef6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725542"
---
# <a name="iclrstrongnamestrongnamegetpublickey-method"></a>ICLRStrongName::StrongNameGetPublicKey 方法

获取公钥/私钥对中的公钥。 密钥对可以作为加密服务提供程序中的密钥容器名称提供 (CSP) 或原始字节集合。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameGetPublicKey (
    [in]  LPCWSTR   szKeyContainer,  
    [in]  BYTE      *pbKeyBlob,  
    [in]  ULONG     cbKeyBlob,  
    [out] BYTE      **ppbPublicKeyBlob,  
    [out] ULONG     *pcbPublicKeyBlob  
);  
```  
  
## <a name="parameters"></a>参数  

 `szKeyContainer`  
 中包含公钥/私钥对的密钥容器的名称。 如果 `pbKeyBlob` 为 null，则 `szKeyContainer` 必须在 CSP 内指定有效容器。 在这种情况下， [ICLRStrongName：： StrongNameGetPublicKey](iclrstrongname-strongnamegetpublickey-method.md) 方法将从存储在容器中的密钥对中提取公钥。  
  
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
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 公钥包含在 [PublicKeyBlob](../strong-naming/publickeyblob-structure.md) 结构中。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameTokenFromPublicKey 方法](iclrstrongname-strongnametokenfrompublickey-method.md)
- [PublicKeyBlob 结构](../strong-naming/publickeyblob-structure.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
