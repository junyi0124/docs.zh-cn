---
title: ICLRStrongName::GetHashFromFile 方法
ms.date: 03/30/2017
api_name:
- ICLRStrongName.GetHashFromFile
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRStrongName::GetHashFromFile
helpviewer_keywords:
- ICLRStrongName::GetHashFromFile method [.NET Framework hosting]
- GetHashFromFile method, ICLRStrongName interface [.NET Framework hosting]
ms.assetid: 9e50480a-8ada-4044-b2a5-97bb14ed3525
topic_type:
- apiref
ms.openlocfilehash: ff346d8f7ba321904a8d91079298b58039e6eb54
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727596"
---
# <a name="iclrstrongnamegethashfromfile-method"></a>ICLRStrongName::GetHashFromFile 方法

生成指定文件内容的哈希。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetHashFromFile (  
    [in]  LPCSTR   szFilePath,  
    [in, out] unsigned int   *piHashAlg,
    [out] BYTE     *pbHash,
    [in]  DWORD    cchHash,
    [out] DWORD    *pchHash  
);  
```  
  
## <a name="parameters"></a>参数  

 `szFilePath`  
 中要进行哈希处理的文件的名称。  
  
 `piHashAlg`  
 [in，out]生成哈希时要使用的算法。 有效算法是由 Win32 CryptoAPI 定义的算法。 如果 `piHashAlg` 设置为0，则使用默认算法 CALG_SHA-1。  
  
 `pbHash`  
 弄一个字节数组，其中包含生成的哈希。  
  
 `cchHash`  
 中指向的缓冲区的最大大小 `pbHash` 。  
  
 `pchHash`  
 弄返回的的大小（以字节为单位） `pbHash` 。  
  
## <a name="return-value"></a>返回值  

 `S_OK` 如果该方法已成功完成，则为;否则，表示失败的 HRESULT 值 (参阅) 列表的 [常见 HRESULT 值](/windows/win32/seccrypto/common-hresult-values) 。  
  
## <a name="remarks"></a>注解  

 此方法与 [ICLRStrongName：： GetHashFromFileW](iclrstrongname-gethashfromfilew-method.md) 方法相同，不同之处在于文件名规范为 ANSI 而不是 Unicode。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** MetaHost  
  
 **库：** 作为中的资源包含 MSCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetHashFromFileW 方法](iclrstrongname-gethashfromfilew-method.md)
- [ICLRStrongName 接口](iclrstrongname-interface.md)
