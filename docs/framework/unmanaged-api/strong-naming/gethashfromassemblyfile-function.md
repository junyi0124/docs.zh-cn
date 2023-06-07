---
title: GetHashFromAssemblyFile 函数
ms.date: 03/30/2017
api_name:
- GetHashFromAssemblyFile
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- GetHashFromAssemblyFile
helpviewer_keywords:
- GetHashFromAssemblyFile function [.NET Framework strong naming]
ms.assetid: 751ed69f-b7ab-4e07-80de-e17ca9319b0c
topic_type:
- apiref
ms.openlocfilehash: caefc9773b0d208f8b20847b664f7bc017d2c076
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730989"
---
# <a name="gethashfromassemblyfile-function"></a>GetHashFromAssemblyFile 函数

使用指定的哈希算法获取指定程序集文件的哈希。  
  
 此函数已弃用。 改为使用 [ICLRStrongName：： GetHashFromAssemblyFile](../hosting/iclrstrongname-gethashfromassemblyfile-method.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetHashFromAssemblyFile (  
    [in]  LPCSTR   szFilePath,  
    [in, out] unsigned int   *piHashAlg,  
    [out] BYTE     *pbHash,  
    [in]  DWORD    cchHash,  
    [out] DWORD    *pchHash  
);  
```  
  
## <a name="parameters"></a>参数  

 `szFilePath`  
 中要进行哈希处理的文件的路径。  
  
 `piHashAlg`  
 [in，out]指定哈希算法的常量。 使用零作为默认哈希算法。  
  
 `pbHash`  
 弄返回的哈希缓冲区。  
  
 `cchHash`  
 中请求的最大大小 `pbHash` 。  
  
 `pchHash`  
 弄返回的的大小（以字节为单位） `pbHash` 。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Stackexchange.redis.strongname  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [GetHashFromAssemblyFile 方法](../hosting/iclrstrongname-gethashfromassemblyfile-method.md)
- [GetHashFromAssemblyFileW 方法](../hosting/iclrstrongname-gethashfromassemblyfilew-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
