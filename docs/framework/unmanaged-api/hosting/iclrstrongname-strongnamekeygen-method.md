---
title: ICLRStrongName::StrongNameKeyGen 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameKeyGen
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameKeyGen
helpviewer_keywords:
- StrongNameKeyGen method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::StrongNameKeyGen method [.NET Framework hosting]
ms.assetid: ac5c1245-9acf-4271-9c08-3d9b7c670df3
topic_type:
- apiref
ms.openlocfilehash: 42a9fc1a05e97bbd893f0a2e77087e6524ad844f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95674536"
---
# <a name="iclrstrongnamestrongnamekeygen-method"></a>ICLRStrongName::StrongNameKeyGen 方法

创建新的公钥/私钥对，以便强名称使用。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameKeyGen (  
    [in]  LPCWSTR   wszKeyContainer,  
    [in]  DWORD     dwFlags,  
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
  
 `ppbKeyBlob`  
 弄返回的公钥/私钥对。  
  
 `pcbKeyBlob`  
 弄的大小（以字节为单位） `ppbKeyBlob` 。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 [ICLRStrongName：： StrongNameKeyGen](iclrstrongname-strongnamekeygen-method.md)方法创建1024位键。 检索到密钥后，应调用 [ICLRStrongName：： StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) 方法来释放已分配的内存。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameKeyGenEx 方法](iclrstrongname-strongnamekeygenex-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
