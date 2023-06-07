---
title: DacpGetModuleAddress::Request 方法
ms.date: 01/16/2019
api.name:
- DacpGetModuleAddress::Request Method
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- DacpGetModuleAddress::Request Method
helpviewer.keywords:
- DacpGetModuleAddress::Request Method [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 1755526636bed6d78663112e4c2ad5ab7c3f731c
ms.sourcegitcommit: d9c7ac5d06735a01c1fafe34efe9486734841a72
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2020
ms.locfileid: "82860838"
---
# <a name="dacpgetmoduleaddressrequest-method"></a>DacpGetModuleAddress::Request 方法

执行从给定的运行时结构填充结构的请求。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
HRESULT Request(
    [in] IXCLRDataModule* pDataModule
);
```

## <a name="parameters"></a>参数

`pDataModule`\
中指向种子数据模块的指针。

## <a name="remarks"></a>备注

此结构存在于运行时中，并且不会通过任何标头或库文件公开。 若要使用它，最简单的方法是模拟实现：

- 返回通过在`Request` `IXCLRDataModule*`参数上调用方法获取的值，该方法具有以下参数：`((uint32) 0xf0000000, 0, 0, (uint32) sizeof(*this), (uint8*) this)`

## <a name="requirements"></a>要求

**平台：** 查看[系统要求](../../get-started/system-requirements.md)\
**标头：** 内容
**库：** 内容
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [DacpGetModuleAddress 结构](dacpgetmoduleaddress-structure.md)
