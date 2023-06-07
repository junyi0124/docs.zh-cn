---
title: CLRDATA_IL_ADDRESS_MAP 结构
ms.date: 01/16/2019
api.name:
- CLRDATA_IL_ADDRESS_MAP Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- CLRDATA_IL_ADDRESS_MAP Structure
helpviewer.keywords:
- CLRDATA_IL_ADDRESS_MAP Structure [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: e680a7a0dc3209d1988f6c84be0864572a74b3a4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79179376"
---
# <a name="clrdata_il_address_map-structure"></a>CLRDATA_IL_ADDRESS_MAP 结构

定义 IL 以解决映射。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
typedef struct
{
    ULONG32 ilOffset;
    CLRDATA_ADDRESS startAddress;
    CLRDATA_ADDRESS endAddress;
    CLRDataSourceType type;
} CLRDATA_IL_ADDRESS_MAP;
```

## <a name="members"></a>成员

| 成员         | 说明                                            |
| -------------- | ------------------------------------------------------ |
| `ilOffset`     | 包含地址范围的 IL 偏移量              |
| `startAddress` | 范围的起始地址。                        |
| `endAddress`   | 范围的结束地址。                          |
| `type`         | 数据的类型。 当前未使用此值 |

## <a name="remarks"></a>备注

此结构位于运行时内，不会通过任何标头或库文件公开。 要使用它，请定义上面指定的结构，其中`CLRDATA_ADDRESS`是 64 位无符号整数。

## <a name="requirements"></a>要求

**平台：** 请参阅[系统要求](../../get-started/system-requirements.md)。  
**标题：** 没有  
**库：** 无 **.NET 框架版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [CLRDataSource 类型枚举](clrdatasourcetype-enumeration.md)
- [调试](index.md)
- [调试结构](debugging-structures.md)
