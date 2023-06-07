---
title: DacpReJitData 结构
ms.date: 02/01/2019
api.name:
- DacpReJitData Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- DacpReJitData Structure
helpviewer.keywords:
- DacpReJitData Structure [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: 46708acc77dad00727ee35e7d06e4228eacbd502
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723033"
---
# <a name="dacprejitdata-structure"></a>DacpReJitData 结构

定义有关给定探查器检测到的方法的基本信息。

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>语法

```cpp
struct MSLAYOUT DacpReJitData
{
    enum Flags
    {
        kUnknown,
        kRequested,
        kActive,
        kReverted,
    };

    CLRDATA_ADDRESS                 rejitID;
    Flags                           flags;
    CLRDATA_ADDRESS                 NativeCodeAddr;
};
```

## <a name="members"></a>成员

| 成员           | 说明                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| `rejitID`        | 方法的 ReJit 修订号。                                                          |
| `flags`          | 一个标志，指示给定版本的该方法的 ReJit 检测的当前状态。 |
| `NativeCodeAddr` | 该方法的 rejitted 实现的基址。                                         |

## <a name="remarks"></a>注解

此结构存在于运行时中，并且不会通过任何标头或库文件公开。 若要使用它，请定义上面指定的结构。 如果不使用 Microsoft 编译器，还必须使用打包来定义结构 `ms_struct` 。

## <a name="requirements"></a>要求

**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
**标头：** 内容  
**库：** 内容  
**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>另请参阅

- [调试](index.md)
- [调试结构](debugging-structures.md)
