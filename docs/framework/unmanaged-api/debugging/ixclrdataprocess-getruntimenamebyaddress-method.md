---
title: IXCLRDataProcess：： GetRuntimeNameByAddress 方法
ms.date: 4/27/2020
api.name:
- IXCLRDataProcess::GetRuntimeNameByAddress Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataProcess::GetRuntimeNameByAddress Method
helpviewer.keywords:
- IXCLRDataProcess::GetRuntimeNameByAddress Method [.NET Framework debugging]
topic_type:
- apiref
author: tommcdon
ms.author: tommcdon
ms.openlocfilehash: 6648bdafe6e5cdd402bcfa02a148c57f0f1e209e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275564"
---
# <a name="ixclrdataprocessgetruntimenamebyaddress-method"></a>IXCLRDataProcess：： GetRuntimeNameByAddress 方法

获取给定地址的名称。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT GetRuntimeNameByAddress(
    [in] CLRDATA_ADDRESS address,
    [in] ULONG32 flags,
    [in] ULONG32 bufLen,
    [out] ULONG32 *nameLen,
    [out, size_is(bufLen)] WCHAR nameBuf[],
    [out] CLRDATA_ADDRESS* displacement
);
```

## <a name="parameters"></a>参数

`address`\
中一个 `CLRDATA_ADDRESS` 表示代码地址的值。

`flags`\
中设置为 "0"。

`bufLen`\
中缓冲区的长度。

`namLen`\
弄一个指针，指向返回的字符数。

`namBuf`\
[out，size_is (`bufLen`) ] `bufLen` 存储运行时名称的长度的输入缓冲区。

`displacement`\
弄 `CLRDATA_ADDRESS` 指向返回符号的代码偏移量的指针。

## <a name="remarks"></a>备注

提供的方法是接口的一部分 `IXCLRDataProcess` ，并且对应于虚拟方法表的第16个槽。

> [!NOTE]
> 如果缓冲区不够大，则此方法返回， `S_FALSE` 并将设置 `nameLen` 为所需的缓冲区长度。

## <a name="requirements"></a>要求

**平台：** 查看 [系统要求](../../get-started/system-requirements.md)\
**标头：** 内容
**库：** 内容
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [IXCLRDataProcess 接口](ixclrdataprocess-interface.md)
