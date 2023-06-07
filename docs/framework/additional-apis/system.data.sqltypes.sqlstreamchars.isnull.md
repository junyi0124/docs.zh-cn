---
title: SqlStreamChars 属性（SqlTypes）
author: stevestein
ms.author: sstein
ms.date: 12/19/2018
ms.technology: dotnet-data
topic_type:
- apiref
api_name:
- System.Data.SqlTypes.SqlStreamChars.IsNull
- System.Data.SqlTypes.SqlStreamChars.get_IsNull
api_location:
- System.Data.dll
api_type:
- Assembly
ms.openlocfilehash: d80f653724b3ef0a1cadb69a5f72b1d9455597d6
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72395732"
---
# <a name="sqlstreamcharsisnull-property"></a>SqlStreamChars 属性

当在派生类中重写时，获取一个值，该值指示流是否 `null`。 包含此属性的程序集与 SQLAccess 具有友元关系。 它旨在 SQL Server 使用。 对于其他数据库，请使用该数据库提供的托管机制。

## <a name="syntax"></a>语法

```csharp
public abstract bool IsNull { get; }
```

## <a name="property-value"></a>属性值

<xref:System.Boolean>\
如果 `null`流，则 `true`;否则，`false`。

## <a name="remarks"></a>备注

> [!WARNING]
> `SqlStreamChars.IsNull` 属性是私有的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此属性。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Data.SqlTypes>

**程序集：** System.object （在 System.web 中）

**.NET framework 版本**：自 2.0 之后可用。
