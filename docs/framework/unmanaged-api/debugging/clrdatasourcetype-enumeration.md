---
title: CLRDataSourceType 枚举
ms.date: 01/16/2019
api.name:
- CLRDataSourceType Enumeration
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- CLRDataSourceType Enumeration
helpviewer.keywords:
- CLRDataSourceType Enumeration [.NET Framework debugging]
topic_type:
- apiref
author: cshung
ms.author: andrewau
ms.openlocfilehash: 7ace405e2624f15b1cdb6d383222ae87c93289bb
ms.sourcegitcommit: 3caa92cb97e9f6c31f21769c7a3f7c4304024b39
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2019
ms.locfileid: "71274099"
---
# <a name="clrdatasourcetype-enumeration"></a>CLRDataSourceType 枚举

提供 CLRDATA_IL_ADDRESS_MAP 结构使用的值。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
typedef enum
{
    CLRDATA_SOURCE_TYPE_INVALID        = 0x00, // To indicate that nothing else applies
} CLRDataSourceType;
```

## <a name="members"></a>成员

| 成员                        | 描述                           |
| ----------------------------- | ------------------------------------- |
| `CLRDATA_SOURCE_TYPE_INVALID` | 指示无其他应用 |

## <a name="remarks"></a>备注

此枚举位于运行时中，并且不会通过任何标头或库文件公开。 若要使用它，请在代码中定义上面定义的枚举。 这也会化名为`CLRDATA_ENUM` ，如[常用数据类型](../common-data-types-unmanaged-api-reference.md)中所述。

## <a name="requirements"></a>要求

**适用**请参阅[系统需求](../../get-started/system-requirements.md)。  
**标头：** None  
**类库**None  
**.NET Framework 版本：** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>请参阅

- [调试](index.md)
- [调试枚举](debugging-enumerations.md)
