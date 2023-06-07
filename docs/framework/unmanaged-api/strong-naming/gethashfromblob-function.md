---
title: GetHashFromBlob 函数
ms.date: 03/30/2017
api_name:
- GetHashFromBlob
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- GetHashFromBlob
helpviewer_keywords:
- GetHashFromBlob function [.NET Framework strong naming]
ms.assetid: b712d862-f2d0-4b55-87d4-65bbeadef982
topic_type:
- apiref
ms.openlocfilehash: d1027aea1d800bda1654b223fec992aa70efd4b7
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140709"
---
# <a name="gethashfromblob-function"></a>GetHashFromBlob 函数

使用指定的哈希算法获取指定内存地址处的程序集的哈希。

此函数已弃用。 改为使用[ICLRStrongName：： GetHashFromBlob](../hosting/iclrstrongname-gethashfromblob-method.md)方法。

## <a name="syntax"></a>语法

```cpp
HRESULT GetHashFromBlob (
    [in]  BYTE    *pbBlob,
    [in]  DWORD   cchBlob,
    [in, out] unsigned int   *piHashAlg,
    [out] BYTE    *pbHash,
    [in]  DWORD   cchHash,
    [out] DWORD   *pchHash
);
```

## <a name="parameters"></a>参数

`pbBlob`\
中一个指针，指向要进行哈希处理的内存块的地址。

`cchBlob`\
中内存块的长度（以字节为单位）。

`piHashAlg`\
[in，out]指定哈希算法的常量。 对于默认算法，使用零。

`pbHash`\
弄返回的哈希缓冲区。

`cchHash`\
中请求的最大 `pbHash`大小。

`pchHash`\
弄返回 `pbHash`的大小（以字节为单位）。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** Stackexchange.redis.strongname

**库：** 作为资源包括在 Mscoree.dll 中

**.NET Framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]

## <a name="see-also"></a>请参阅

- [GetHashFromBlob 方法](../hosting/iclrstrongname-gethashfromblob-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
