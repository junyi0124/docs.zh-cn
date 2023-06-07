---
title: StrongNameKeyDelete 函数
ms.date: 03/30/2017
api_name:
- StrongNameKeyDelete
api_location:
- mscoree.dll
api_type:
- DLLExport
f1_keywords:
- StrongNameKeyDelete
helpviewer_keywords:
- StrongNameKeyDelete function [.NET Framework strong naming]
ms.assetid: 313e71e4-1790-4d2f-b68b-5040ebd1c149
topic_type:
- apiref
ms.openlocfilehash: d37f990241ae704abef55d863da0f40a31284837
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73141597"
---
# <a name="strongnamekeydelete-function"></a>StrongNameKeyDelete 函数

删除指定的密钥容器。

此函数已弃用。 改为使用[ICLRStrongName：： StrongNameKeyDelete](../hosting/iclrstrongname-strongnamekeydelete-method.md)方法。

## <a name="syntax"></a>语法

```cpp
BOOLEAN StrongNameKeyDelete (
    [in]  LPCWSTR   wszKeyContainer
);
```

## <a name="parameters"></a>参数

`wszKeyContainer`\
中要删除的密钥容器的名称。

## <a name="return-value"></a>返回值

成功完成后 `true`;否则，`false`。

## <a name="remarks"></a>备注

使用[StrongNameKeyInstall](strongnamekeyinstall-function.md)函数将公钥/私钥对导入到容器中。

如果 `StrongNameKeyDelete` 函数未成功完成，请调用[StrongNameErrorInfo](strongnameerrorinfo-function.md)函数以检索上次生成的错误。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

**标头：** Stackexchange.redis.strongname

**库：** 作为资源包括在 Mscoree.dll 中

**.NET Framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]

## <a name="see-also"></a>请参阅

- [StrongNameKeyDelete 方法](../hosting/iclrstrongname-strongnamekeydelete-method.md)
- [StrongNameKeyInstall 方法](../hosting/iclrstrongname-strongnamekeyinstall-method.md)
- [ICLRStrongName 接口](../hosting/iclrstrongname-interface.md)
