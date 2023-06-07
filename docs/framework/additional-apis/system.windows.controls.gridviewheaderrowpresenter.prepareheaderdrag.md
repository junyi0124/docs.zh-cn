---
title: 'PrepareHeaderDrag 方法 (GridViewHeaderRowPresenter) '
description: 了解名为 PrepareHeaderDrag 的专用 WPF 方法。
ms.date: 09/25/2020
ms.technology: dotnet-wpf
topic_type:
- apiref
api_name:
- System.Windows.Controls.GridViewHeaderRowPresenter.PrepareHeaderDrag
api_location:
- PresentationFramework.dll
api_type:
- Assembly
ms.openlocfilehash: 6d806b8a50de3234cd04198f4ab86621dcd6ede1
ms.sourcegitcommit: eb7e87496f42361b1da98562dd75b516c9d58bbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877778"
---
# <a name="prepareheaderdrag-method"></a>PrepareHeaderDrag 方法

准备要重新排序的指定列标题。

```csharp
private void PrepareHeaderDrag(GridViewColumnHeader header, Point pos, Point relativePos, bool cancelInvoke)
```

> [!WARNING]
> 此方法是专用的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此方法。

## <a name="parameters"></a>参数

`header` <xref:System.Windows.Controls.GridViewColumnHeader>\
准备重新排序的列标题。

`pos` <xref:System.Windows.Point>\
相对于的位置， <xref:System.Windows.Controls.GridViewHeaderRowPresenter> 拖动从此处开始。

`relativePos` <xref:System.Windows.Point>\
相对于的位置， `header` 拖动从此处开始。

`cancelInvoke` <xref:System.Boolean>\
`true` 如果取消重新排序，则为;否则为 `false` 。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Windows.Controls>

**程序集：** PresentationFramework.dll

## <a name="see-also"></a>请参阅

- <xref:System.Windows.Controls.GridViewHeaderRowPresenter>
