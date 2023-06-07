---
title: ServicePoint 字段 m_ConnectionGroupList
description: 了解 "m_ConnectionGroupList ServicePoint" 字段，它是一个连接组哈希表，其中每个连接组都在 .NET 中保存 ServicePoint URI 的连接。
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ServicePoint.m_ConnectionGroupList
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: df8afb59-f0f6-4ddc-b3c1-839b9fc601d8
ms.openlocfilehash: 0ebfeb782147f21abfde536b8053fa15b1e1a602
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989712"
---
# <a name="servicepointm_connectiongrouplist-field"></a>ServicePoint \_ ConnectionGroupList 字段

`ServicePoint.m_ConnectionGroupList`是一个 <xref:System.Collections.Hashtable> 连接组，每个组都有一个用于的 URI 的连接 <xref:System.Net.ServicePoint> 。

## <a name="syntax"></a>语法
  
```csharp  
private Hashtable m_ConnectionGroupList
```

> [!WARNING]
> 此 `ServicePoint.m_ConnectionGroupList` 字段是专用的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此字段。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）

**.NET Framework 版本：** 自2.0 起可用。
