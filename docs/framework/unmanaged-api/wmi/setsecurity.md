---
title: 设置安全功能（非托管 API 引用）
description: SetSecurity 函数检索当前线程的模拟令牌。
ms.date: 11/06/2017
api_name:
- SetSecurity
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- SetSecurity
helpviewer_keywords:
- SetSecurity function [.NET WMI and performance counters]
topic_type:
- Reference
ms.openlocfilehash: 809f6a95fdd6854b3a591b496877838c48d52199
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79176729"
---
# <a name="setsecurity-function"></a>SetSecurity 函数

检索与当前线程关联的模拟令牌。

[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT SetSecurity (
   [out] boolean* pNeedToReset,
   [out] HANDLE* pCurrentThreadToken
);
```

## <a name="parameters"></a>parameters

`pNeedToReset`\
[出]当函数返回时，包含指向 的`boolean`指针，指示是否应通过调用[ResetSecurity](resetsecurity.md)函数重置令牌。

`token`\
[出]当函数返回时，包含指向与当前线程关联的模拟令牌句柄的指针。 如果没有与当前线程`null`关联的令牌，则其值可以是。

## <a name="return-value"></a>返回值

如果函数成功，则返回值为`S_OK`（0）。

如果函数失败，返回值是非零错误代码。 要获取扩展的错误信息，请致电[GetErrorInfo](geterrorinfo.md)函数。

## <a name="requirements"></a>要求

 **平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。

 **标题：** WMINet_Utils.idl

 **.NET 框架版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]

## <a name="see-also"></a>另请参阅

- [WMI 和性能计数器（非托管 API 参考）](index.md)
