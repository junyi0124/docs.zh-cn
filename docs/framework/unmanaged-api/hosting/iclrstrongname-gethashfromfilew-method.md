---
title: ICLRStrongName::GetHashFromFileW 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.GetHashFromFileW
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::GetHashFromFileW
helpviewer_keywords:
- GetHashFromFileW method, ICLRStrongName interface [.NET Framework hosting]
- ICLRStrongName::GetHashFromFileW method [.NET Framework hosting]
ms.assetid: c6ff45fc-905d-4c6e-b00c-97c6c7c55d99
topic_type:
- apiref
ms.openlocfilehash: eda78976f175230cc2405b9cb151993e63697e48
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685755"
---
# <a name="iclrstrongnamegethashfromfilew-method"></a>ICLRStrongName::GetHashFromFileW 方法

生成由 Unicode 字符串指定的文件内容的哈希。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetHashFromFileW (
    [in]  LPCWSTR   wszFilePath,  
    [in, out] unsigned int   *piHashAlg,  
    [out] BYTE      *pbHash,  
    [in]  DWORD     cchHash,  
    [out] DWORD     *pchHash  
);
```  
  
## <a name="parameters"></a>参数  

 `wszFilePath`  
 中要进行哈希处理的文件的 Unicode 名称。  
  
 `piHashAlg`  
 [in，out]生成哈希时要使用的算法。 有效算法是由 Win32 CryptoAPI 定义的算法。 如果 `piHashAlg` 设置为0，则使用默认算法 CALG_SHA-1。  
  
 `pbHash`  
 弄一个字节数组，其中包含生成的哈希。  
  
 `cchHash`  
 中所指向的缓冲区的最大大小 `pbHash` 。  
  
 `pchHash`  
 弄的大小（以字节为单位） `pbHash` 。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 此方法与 [ICLRStrongName：： GetHashFromFile](iclrstrongname-gethashfromfile-method.md) 方法相同，不同之处在于文件名规范是 Unicode 而不是 ANSI。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetHashFromFile 方法](iclrstrongname-gethashfromfile-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
