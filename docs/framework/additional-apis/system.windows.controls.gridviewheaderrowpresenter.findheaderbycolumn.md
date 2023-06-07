---
title: 'FindHeaderByColumn 方法 (GridViewHeaderRowPresenter) '
description: 了解名为 FindHeaderByColumn 的专用 WPF 方法。
ms.date: 09/25/2020
ms.technology: dotnet-wpf
topic_type:
- apiref
api_name:
- System.Windows.Controls.GridViewHeaderRowPresenter.FindHeaderByColumn
api_location:
- PresentationFramework.dll
api_type:
- Assembly
ms.openlocfilehash: 88ed2928f86602d1078488354de6d5267c0311f5
ms.sourcegitcommit: eb7e87496f42361b1da98562dd75b516c9d58bbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877781"
---
# <a name="findheaderbycolumn-method"></a>FindHeaderByColumn 方法

在可视化树中查找指定列的列标题。

```csharp
private GridViewColumnHeader FindHeaderByColumn(GridViewColumn column)
```

> [!WARNING]
> 此方法是专用的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此方法。

## <a name="parameters"></a>参数

`column` <xref:System.Windows.Controls.GridViewColumn>\
应找到并返回其标头的列。

## <a name="return-value"></a>返回值

<xref:System.Windows.Controls.GridViewColumnHeader>\
指定列的标头， `null` 如果指定列不存在，则为。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Windows.Controls>

**程序集：** PresentationFramework.dll

## <a name="see-also"></a>请参阅

- <xref:System.Windows.Controls.GridViewHeaderRowPresenter>
