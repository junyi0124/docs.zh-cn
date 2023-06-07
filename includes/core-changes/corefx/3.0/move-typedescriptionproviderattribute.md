---
ms.openlocfilehash: 7a2617f27dfd6bb527ff6d408fae6382075f24ae
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032034"
---
### <a name="typedescriptionproviderattribute-moved-to-another-assembly"></a>TypeDescriptionProviderAttribute 已移到另一程序集

<xref:System.ComponentModel.TypeDescriptionProviderAttribute> 类已移动。

#### <a name="change-description"></a>更改描述

在 .NET Core 2.2 及更低版本中，<xref:System.ComponentModel.TypeDescriptionProviderAttribute> 位于 System.ComponentModel.TypeConverter 程序集中  。

而自 .NET Core 3.0 起，它位于 System.ObjectModel 程序集中  。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

此更改仅影响这样的应用程序，它们通过调用 <xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType> 等方法或调用假设类型位于特定程序集中的 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 的重载，使用反射过程来加载 <xref:System.ComponentModel.TypeDescriptionProviderAttribute> 类型。 若是如此，应更新在方法调用反射的程序集，以反射出类型的新程序集位置。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

无。

<!--

#### Affected APIs

- Not detectable via API analysis

-->
