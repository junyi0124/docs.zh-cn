---
ms.openlocfilehash: 78678b4b352bb063d1521e9aee3492c0cee059b8
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031979"
---
### <a name="invalidasynchronousstateexception-moved-to-another-assembly"></a>InvalidAsynchronousStateException 已移到另一程序集

<xref:System.ComponentModel.InvalidAsynchronousStateException> 类已移动。

#### <a name="change-description"></a>更改描述

在 .NET Core 2.2 及更低版本中，<xref:System.ComponentModel.InvalidAsynchronousStateException> 位于 System.ComponentModel.TypeConverter 程序集中  。

而自 .NET Core 3.0 起，它位于 System.ComponentModel.Primitives 程序集中  。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

此更改仅影响这样的应用程序，它们通过调用 <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> 等方法或调用假设类型位于特定程序集中的 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 的重载，使用反射过程来加载 <xref:System.ComponentModel.InvalidAsynchronousStateException>。 如果是这种情况，可以更新在方法调用中引用的程序集，以反映出类型的新程序集位置。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

无。

<!--

#### Affected APIs

- Not detectable via API analysis

-->
