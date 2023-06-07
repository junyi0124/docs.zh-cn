---
title: 中断性变更：WinForms 属性现在引发 ArgumentOutOfRangeException
description: 了解 .NET 5.0 中的以下中断性变更：某些 Windows 窗体属性现在将针对无效参数引发 ArgumentOutOfRangeException。
ms.date: 06/18/2020
ms.openlocfilehash: 94593047d16304ce401b23993ad4ca173c10812b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759243"
---
# <a name="winforms-properties-now-throw-argumentoutofrangeexception"></a>WinForms 属性现在引发 ArgumentOutOfRangeException

某些 Windows 窗体属性现在将针对无效参数引发 <xref:System.ArgumentOutOfRangeException>，之前不会这样。

## <a name="change-description"></a>更改描述

以前，当传递超出范围的参数时，这些属性将引发各种异常，如 <xref:System.NullReferenceException>、<xref:System.IndexOutOfRangeException> 或 <xref:System.ArgumentException>。 从 .NET 5.0 开始，如果传递的参数超出范围，则这些属性现在将引发 <xref:System.ArgumentOutOfRangeException>。

引发 <xref:System.ArgumentOutOfRangeException> 符合 .NET 运行时的行为。 它还通过清楚地指示具体的无效参数来改进调试体验。

## <a name="version-introduced"></a>引入的版本

.NET 5.0

## <a name="recommended-action"></a>建议操作

- 更新代码以防止传递无效参数。
- 如有必要，请在设置属性时处理 <xref:System.ArgumentOutOfRangeException>。

## <a name="affected-apis"></a>受影响的 API

下表列出了受影响的方法和参数：

> [!div class="mx-tdBreakAll"]
> | Property | 参数名称 | 新增的版本 |
> |-|-|-|
> | <xref:System.Windows.Forms.ListBox.IntegerCollection.Item(System.Int32)?displayProperty=nameWithType> | `index` | 5.0 预览版 5 |
> | <xref:System.Windows.Forms.TreeNode.ImageIndex?displayProperty=nameWithType> | `value` | 5.0 预览版 6 |
> | <xref:System.Windows.Forms.TreeNode.SelectedImageIndex?displayProperty=nameWithType> | `value` | 5.0 预览版 6 |

<!--

### Affected APIs

- `P:System.Windows.Forms.ListBox.IntegerCollection.Item(System.Int32)`
- `P:System.Windows.Forms.TreeNode.ImageIndex`
- `P:System.Windows.Forms.TreeNode.SelectedImageIndex`

### Category

Windows Forms

-->
