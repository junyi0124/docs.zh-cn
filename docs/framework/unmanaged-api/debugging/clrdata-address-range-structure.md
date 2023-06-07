---
title: CLRDATA_ADDRESS_RANGE 结构
ms.date: 01/16/2019
api.name:
- CLRDATA_ADDRESS_RANGE Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- CLRDATA_ADDRESS_RANGE Structure
helpviewer.keywords:
- CLRDATA_ADDRESS_RANGE Structure [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 8eb841b4c4f06a3932805ae6222bdd693def5ea0
ms.sourcegitcommit: 3caa92cb97e9f6c31f21769c7a3f7c4304024b39
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71274301"
---
# <a name="clrdata_address_range-structure"></a>CLRDATA_ADDRESS_RANGE 结构

定义地址范围。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
typedef struct
{
    CLRDATA_ADDRESS startAddress;
    CLRDATA_ADDRESS endAddress;
} CLRDATA_ADDRESS_RANGE;
```

## <a name="members"></a>成员

| 成员         | 描述                     |
| -------------- | ------------------------------- |
| `startAddress` | 范围的起始地址。 |
| `endAddress`   | 范围的结束地址。   |

## <a name="remarks"></a>备注

此结构存在于运行时中，并且不会通过任何标头或库文件公开。 若要使用它，请定义上面指定的结构， `CLRDATA_ADDRESS`其中是一个64位无符号整数。

## <a name="requirements"></a>要求

**适用**请参阅[系统需求](../../get-started/system-requirements.md)。  
**标头：** None  
**类库**None  
**.NET Framework 版本：** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>请参阅

- [调试](index.md)
- [调试结构](debugging-structures.md)
