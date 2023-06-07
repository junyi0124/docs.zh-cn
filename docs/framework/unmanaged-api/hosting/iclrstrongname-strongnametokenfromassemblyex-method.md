---
title: ICLRStrongName::StrongNameTokenFromAssemblyEx 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.StrongNameTokenFromAssemblyEx
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::StrongNameTokenFromAssemblyEx
helpviewer_keywords:
- StrongNameTokenFromAssemblyEx method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::StrongNameTokenFromAssemblyEx method [.NET Framework hosting]
ms.assetid: 648ea90e-5e60-40a0-a56a-3e61bf2fba7c
topic_type:
- apiref
ms.openlocfilehash: 35fe86f856360814cfbb5453bfb6e5efaaef02fd
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677721"
---
# <a name="iclrstrongnamestrongnametokenfromassemblyex-method"></a>ICLRStrongName::StrongNameTokenFromAssemblyEx 方法

从指定的程序集文件创建强名称令牌，并返回该令牌表示的公钥。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT StrongNameTokenFromAssemblyEx (  
    [in]  LPCWSTR   wszFilePath,  
    [out] BYTE      **ppbStrongNameToken,  
    [out] ULONG     *pcbStrongNameToken,  
    [out] BYTE      **ppbPublicKeyBlob,  
    [out] ULONG     *pcbPublicKeyBlob  
);  
```  
  
## <a name="parameters"></a>参数  

 `wszFilePath`  
 中适用于程序集的可移植可执行文件 (PE) 文件的路径。  
  
 `ppbStrongNameToken`  
 弄返回的强名称标记。  
  
 `pcbStrongNameToken`  
 弄强名称令牌的大小（以字节为单位）。  
  
 `ppbPublicKeyBlob`  
 弄返回的公钥。  
  
 `pcbPublicKeyBlob`  
 弄公钥的大小（以字节为单位）。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 强名称标记是公钥的缩写形式。 标记是从用于对程序集进行签名的公钥创建的64位哈希。 该令牌是程序集的强名称的一部分，并且可以从程序集元数据中读取。  
  
 检索到密钥并创建令牌后，应调用 [ICLRStrongName：： StrongNameFreeBuffer](iclrstrongname-strongnamefreebuffer-method.md) 方法来释放已分配的内存。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [StrongNameTokenFromAssembly 方法](iclrstrongname-strongnametokenfromassembly-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
