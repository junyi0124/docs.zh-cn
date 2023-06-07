---
title: SmiOrderProperty.Item 属性 (Microsoft.SqlServer.Server)
author: stevestein
ms.author: sstein
ms.date: 12/20/2018
ms.technology: dotnet-data
topic_type:
- apiref
api_name:
- Microsoft.SqlServer.Server.SmiOrderProperty.Item
- Microsoft.SqlServer.Server.SmiOrderProperty.get_Item
api_location:
- System.Data.dll
api_type:
- Assembly
ms.openlocfilehash: e2d8788f610d80c30baf51bff0131f0834d59fcd
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2019
ms.locfileid: "65634576"
---
# <a name="smiorderpropertyitem-property"></a>SmiOrderProperty.Item 属性

获取实体的列顺序。 包含此属性的程序集具有与 SQLAccess.dll 友元关系。 它被用于 SQL server 上。 对于其他数据库，使用提供该数据库的宿主机制。

## <a name="syntax"></a>语法

```csharp
internal SmiColumnOrder Item { get; }
```

## <a name="property-value"></a>属性值

列的顺序。

## <a name="remarks"></a>备注

> [!WARNING]
> `SmiOrderProperty.Item`属性内部使用并且不应在代码中直接使用。
>
> Microsoft 不支持在生产应用程序在任何情况下使用此属性。

## <a name="requirements"></a>要求

**Namespace**：<xref:Microsoft.SqlServer.Server>

**程序集：** System.Data （在 System.Data.dll 中)

**.NET framework 版本：** 自 2.0 之后可用。
