---
title: SqlStreamChars. CanSeek 属性（SqlTypes）
author: stevestein
ms.author: sstein
ms.date: 12/19/2018
ms.technology: dotnet-data
topic_type:
- apiref
api_name:
- System.Data.SqlTypes.SqlStreamChars.CanSeek
- System.Data.SqlTypes.SqlStreamChars.get_CanSeek
api_location:
- System.Data.dll
api_type:
- Assembly
ms.openlocfilehash: eb32978f62b7d46f0abf715e2bca347592c0fda8
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72395781"
---
# <a name="sqlstreamcharscanseek-property"></a>SqlStreamChars. CanSeek 属性

当在派生类中重写时，获取一个值，该值指示当前流是否支持查找操作。 包含此属性的程序集与 SQLAccess 具有友元关系。 它旨在 SQL Server 使用。 对于其他数据库，请使用该数据库提供的托管机制。

```csharp
public abstract bool CanSeek { get; }
```

## <a name="property-value"></a>属性值

<xref:System.Boolean>\
`true` 如果当前流支持查找操作，则为; 否则为。否则，`false`。

## <a name="remarks"></a>备注

> [!WARNING]
> `SqlStreamChars.CanSeek` 属性是私有的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此属性。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Data.SqlTypes>

**程序集：** System.object （在 System.web 中）

**.NET framework 版本**：自 2.0 之后可用。
