---
title: SqlStreamChars （Int64，SeekOrigin）方法（SqlTypes）
author: stevestein
ms.author: sstein
ms.date: 12/20/2018
ms.technology: dotnet-data
topic_type:
- apiref
api_name:
- System.Data.SqlTypes.SqlStreamChars.Seek
api_location:
- System.Data.dll
api_type:
- Assembly
ms.openlocfilehash: db8aba0a86c140ba62af8056011226532d415951
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72395594"
---
# <a name="sqlstreamcharsseekint64-seekorigin-method"></a>SqlStreamChars （Int64，SeekOrigin）方法

当在派生类中重写时，设置当前流中的位置。 包含此方法的程序集与 SQLAccess 具有友元关系。 它旨在 SQL Server 使用。 对于其他数据库，请使用该数据库提供的托管机制。

```csharp
public abstract long Seek (long offset, System.IO.SeekOrigin origin);
```

## <a name="parameters"></a>参数

`offset`\
相对于 `origin`的字节偏移量。

`origin`\
枚举值之一，指示要从中获取新位置的参考点。

## <a name="returns"></a>返回

<xref:System.Int32>\
当前流中的新位置。

## <a name="remarks"></a>备注

> [!WARNING]
> `SqlStreamChars.Seek` 方法是私有的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此方法。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Data.SqlTypes>

**程序集：** System.object （在 System.web 中）

**.NET framework 版本**：自 2.0 之后可用。
