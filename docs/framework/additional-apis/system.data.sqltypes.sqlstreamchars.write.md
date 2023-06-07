---
title: SqlStreamChars （Char []，Int32，Int32）方法（SqlTypes）
author: stevestein
ms.author: sstein
ms.date: 12/20/2018
ms.technology: dotnet-data
topic_type:
- apiref
api_name:
- System.Data.SqlTypes.SqlStreamChars.Write
api_location:
- System.Data.dll
api_type:
- Assembly
ms.openlocfilehash: 9d952041122ceb3824712bd81cab7ce4789c9db8
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72395577"
---
# <a name="sqlstreamcharswritechar-int32-int32-method"></a>SqlStreamChars （Char []，Int32，Int32）方法

当在派生类中重写时，将字符序列写入当前流，并将此流中的当前位置提升写入的字符数。 包含此方法的程序集与 SQLAccess 具有友元关系。 它旨在 SQL Server 使用。 对于其他数据库，请使用该数据库提供的托管机制。

```csharp
public abstract void Write (char[] buffer, int offset, int count);
```

## <a name="parameters"></a>参数

`buffer`  
要写入的字符数组。

`offset`  
相对于原点的偏移量。

`count`  
要写入当前流的字符数。

## <a name="remarks"></a>备注

> [!WARNING]
> @No__t-0 方法是私有的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此方法。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Data.SqlTypes>

**程序集：** System.object （在 System.web 中）

**.NET Framework 版本：** 自2.0 起可用。
