---
title: ConnectionGroup 类
description: 阅读有关 ConnectionGroup 类的信息，该类在 ServicePoint 上下文中对连接进行分组，并用于维护 .NET 中网络资源的上下文。
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ConnectionGroup
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: 25c08217-fdeb-44b9-9cd6-1b4955d6e602
ms.openlocfilehash: 7121713b26880f2490b40d59d92d431a567519b3
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989814"
---
# <a name="connectiongroup-class"></a>ConnectionGroup 类

`ConnectionGroup`类对上下文内的连接列表进行分组 <xref:System.Net.ServicePoint> ，并用于维护网络资源（例如，代理和单独的客户端）的上下文。

## <a name="syntax"></a>语法
  
```csharp  
internal class ConnectionGroup
```

> [!WARNING]
> `ConnectionGroup`类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）

**.NET Framework 版本：** 自2.0 起可用。
