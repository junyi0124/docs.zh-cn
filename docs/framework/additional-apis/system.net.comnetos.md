---
title: ComNetOS 类（System.Net）
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.ComNetOS
- System.Net.ComNetOS.IsWin7orLater
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: ed2b970d07df2c338870b386e75c1688703f1d68
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990456"
---
# <a name="comnetos-class"></a>ComNetOS 类

提供有关当前操作系统的信息，如其版本和安装类型（客户端或服务器）。 无法继承此类。
  
```csharp  
internal static class ComNetOS
```

> [!WARNING]
> 此类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="iswin7orlater-field"></a>IsWin7orLater 字段

指定操作系统版本是否为 Windows 7 或更高版本。

```csharp
internal static readonly bool IsWin7orLater
```

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）
