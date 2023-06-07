---
title: StrongNameKeyInstall 函数
ms.date: 03/30/2017
api_name:
- StrongNameKeyInstall
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameKeyInstall
helpviewer_keywords:
- StrongNameKeyInstall function [.NET Framework strong naming]
ms.assetid: e32fd546-7757-4681-be3d-658e93281e50
topic_type:
- apiref
ms.openlocfilehash: 9e441d4da64e9704fbda2368d2b07289aaea610a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73125198"
---
# <a name="strongnamekeyinstall-function"></a>StrongNameKeyInstall 函数

将公钥/私钥对导入容器。

此函数已弃用。 改为使用[ICLRStrongName：： StrongNameKeyInstall](../hosting/iclrstrongname-strongnamekeyinstall-method.md)方法。

## <a name="syntax"></a>语法

```cpp
BOOLEAN StrongNameKeyInstall (
    [in]  LPCWSTR   wszKeyContainer,
    [in]  BYTE      *pbKeyBlob,
    [in]  ULONG     cbKeyBlob
);
```

## <a name="parameters"></a>参数

`wszKeyContainer`\
中密钥容器的名称。 `wszKeyContainer` 必须为非空字符串。

`pbKeyBlob`\
中二进制密钥对。

`cbKeyBlob`\
中`pbKeyBlob`的大小（以字节为单位）。

## <a name="return-value"></a>返回值

成功完成后 `true`;否则，`false`。

## <a name="remarks"></a>备注

使用[StrongNameKeyDelete](strongnamekeydelete-function.md)函数可删除密钥容器。

如果 `StrongNameKeyInstall` 函数未成功完成，请调用[StrongNameErrorInfo](strongnameerrorinfo-function.md)函数以检索上次生成的错误。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** Stackexchange.redis.strongname

**库：** 作为资源包括在 Mscoree.dll 中

**.NET Framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]

## <a name="see-also"></a>请参阅

- [StrongNameKeyInstall 方法](../hosting/iclrstrongname-strongnamekeyinstall-method.md)
- [StrongNameKeyDelete 方法](../hosting/iclrstrongname-strongnamekeydelete-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
