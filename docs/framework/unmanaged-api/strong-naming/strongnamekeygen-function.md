---
title: StrongNameKeyGen 函数
ms.date: 03/30/2017
api_name:
- StrongNameKeyGen
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameKeyGen
helpviewer_keywords:
- StrongNameKeyGen function [.NET Framework strong naming]
ms.assetid: 883e413a-ad2f-4f7f-b1b9-aeb8fe5b65f8
topic_type:
- apiref
ms.openlocfilehash: 4844701784a3e6a1008a5deb2bdff3b3ba47aa7e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95691404"
---
# <a name="strongnamekeygen-function"></a>StrongNameKeyGen 函数

创建新的公钥/私钥对，以便强名称使用。  
  
 此函数已弃用。 改为使用 [ICLRStrongName：： StrongNameKeyGen](../hosting/iclrstrongname-strongnamekeygen-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
BOOLEAN StrongNameKeyGen (  
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
 中指定是否保留注册的密钥。 支持以下值：  
  
- 0x00000000-在 `wszKeyContainer` 为 null 时用于生成临时密钥容器名称。  
  
- 0x00000001 (`SN_LEAVE_KEY`) -指定应保持注册该密钥。  
  
 `ppbKeyBlob`  
 弄返回的公钥/私钥对。  
  
 `pcbKeyBlob`  
 弄的大小（以字节为单位） `ppbKeyBlob` 。  
  
## <a name="return-value"></a>返回值  

 `true` 成功完成时;否则为 `false` 。  
  
## <a name="remarks"></a>注解  

 `StrongNameKeyGen`函数创建1024位键。 检索到密钥后，应调用 [StrongNameFreeBuffer](strongnamefreebuffer-function.md) 函数以释放已分配的内存。  
  
 如果 `StrongNameKeyGen` 函数未成功完成，请调用 [StrongNameErrorInfo](strongnameerrorinfo-function.md) 函数来检索上次生成的错误。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameKeyGen 方法](../hosting/iclrstrongname-strongnamekeygen-method.md)
- [StrongNameKeyGenEx 方法](../hosting/iclrstrongname-strongnamekeygenex-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
