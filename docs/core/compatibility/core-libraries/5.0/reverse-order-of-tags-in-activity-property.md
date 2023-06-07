---
title: 中断性变更：Activity.Tags 中的标记顺序是相反的
description: 了解核心 .NET 库中的以下 .NET 5.0 中断性变更：Activity.Tags 现在根据项目的添加顺序将它们存储在列表中。
ms.date: 11/01/2020
ms.openlocfilehash: 1ff14dc1a4f7a0bf8cf9e79f3750b819f4d4a5ca
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759150"
---
# <a name="order-of-tags-in-activitytags-is-reversed"></a>Activity.Tags 中的标记顺序是相反的

<xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> 现根据项目的添加顺序将其存储在列表中，也就是说，添加的第一个项目排在列表第一位。 此更改是为了与 [OpenTelemetry 属性规范](https://github.com/open-telemetry/opentelemetry-specification/blob/master/specification/common/common.md#attributes)一致。

## <a name="change-description"></a>更改描述

在以前的 .NET 版本中，<xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> 存储项目的顺序与项目的添加顺序相反。 也就是说，添加的第一个项目排在列表中的最后一位。 从 .NET 5.0 开始，项目的顺序是相反的，添加的第一项始终排在列表第一位。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="recommended-action"></a>建议操作

如果应用依赖于 <xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> 列表顺序，并且要升级到 .NET 5.0 或更高版本，则需要你更改代码的此部分。

## <a name="affected-apis"></a>受影响的 API

- <xref:System.Diagnostics.Activity.Tags?displayProperty=fullName>

<!--

#### Category

Core .NET libraries

### Affected APIs

- `P:System.Diagnostics.Activity.Tags`

-->
