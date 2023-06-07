---
title: ICLRStrongName::StrongNameKeyGenEx 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameKeyGenEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameKeyGenEx
helpviewer_keywords:
- ICLRStrongName::StrongNameKeyGenEx method [.NET Framework hosting]
- StrongNameKeyGenEx method, ICLRStrongName interface [.NET Framework hosting]
ms.assetid: 1f8b59d0-5b72-45b8-ab74-c2b43ffc806e
topic_type:
- apiref
ms.openlocfilehash: 9ba50616b25f9c7c592f19947c82a890ae6b5a4a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95671676"
---
# <a name="iclrstrongnamestrongnamekeygenex-method"></a>ICLRStrongName::StrongNameKeyGenEx 方法

使用指定的密钥大小生成新的公钥/私钥对，以便使用强名称。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameKeyGenEx (  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  DWORD     dwFlags,  
    [in]  DWORD     dwKeySize,  
    [out] BYTE      **ppbKeyBlob,  
    [out] ULONG     *pcbKeyBlob  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszKeyContainer`  
 中请求的密钥容器名称。 `wszKeyContainer` 必须为非空字符串，或者为 null 以生成临时名称。  
  
 `dwFlags`  
 中一个值，该值指定是否保留注册的密钥。 支持以下值：  
  
- 0x00000000-在 `wszKeyContainer` 为 null 时用于生成临时密钥容器名称。  
  
- 0x00000001 (`SN_LEAVE_KEY`) -指定应保持注册该密钥。  
  
 `dwKeySize`  
 中请求的密钥大小，以位为单位。  
  
 `ppbKeyBlob`  
 弄返回的公钥/私钥对。  
  
 `pcbKeyBlob`  
 弄的大小（以字节为单位） `ppbKeyBlob` 。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 .NET Framework 版本1.0 和1.1 需要 `dwKeySize` 1024 位才能使用强名称对程序集进行签名; 版本2.0 增加了对2048位密钥的支持。  
  
 检索到密钥后，应调用 [ICLRStrongName：： StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) 方法来释放已分配的内存。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameKeyGen 方法](iclrstrongname-strongnamekeygen-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
