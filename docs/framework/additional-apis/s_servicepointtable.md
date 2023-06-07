---
title: ServicePointManager 字段 s_ServicePointTable
description: 阅读有关 .NET 中的 s_ServicePointTable ServicePointManager 字段的信息。 此哈希表字段包含 AppDomain 中的活动 HTTP 连接（ServicePoints）。
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ServicePointManager.s_ServicePointTable
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: 24459679-291c-401a-9def-e42b29466fcf
ms.openlocfilehash: 9462ae10125dd37706f786a1f2cef78e62fbabcc
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989544"
---
# <a name="servicepointmanagers_servicepointtable-field"></a>ServicePointManager \_ ServicePointTable 字段

`ServicePointManager.s_ServicePointTable`一个 <xref:System.Collections.Hashtable> ，它包含中的活动 HTTP 连接的列表 <xref:System.Net.ServicePoint> <xref:System.AppDomain> 。

## <a name="syntax"></a>语法
  
```csharp  
private static Hashtable s_ServicePointTable
```

> [!WARNING]
> 此 `ServicePointManager.s_ServicePointTable` 字段是专用的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此字段。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）

**.NET Framework 版本：** 自2.0 起可用。
