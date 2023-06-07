---
title: IXCLRDataModule：： Request 方法
ms.date: 01/16/2019
api.name:
- IXCLRDataModule::Request Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- IXCLRDataModule::Request Method
helpviewer.keywords:
- IXCLRDataModule::Request Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 3c18fc5c947cbb89fc4e9aed60d3cedcbe22d749
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83420807"
---
# <a name="ixclrdatamodulerequest-method"></a>IXCLRDataModule：： Request 方法

请求填充模块数据所提供的缓冲区。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT Request([in] ULONG32 reqCode,
    [in] ULONG32 inBufferSize,
    [in, size_is(inBufferSize)] BYTE* inBuffer,
    [in] ULONG32 outBufferSize,
    [out, size_is(outBufferSize)] BYTE* outBuffer);
```

## <a name="parameters"></a>参数

`reqCode`\
中要发送的请求类型。

`inBufferSize`\
[in] 要传入的输入缓冲区的大小。

`inBuffer`\
[in，size_is （inBufferSize）]要在请求中发送的原始数据的缓冲区指针。

`outBufferSize`\
中输出缓冲区的大小。

`outBuffer`\
[out，size_is （outBufferSize）]用于存储请求响应的缓冲区指针。

## <a name="remarks"></a>备注

提供的方法是接口的一部分 `IXCLRDataModule` ，并且对应于虚拟方法表的37th 槽。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。
**标头：** 无**库：** 无 **.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [IXCLRDataModule 接口](ixclrdatamodule-interface.md)
