---
title: 中断性变更：FrameworkDescription 的值是 .NET 而不是 .NET Core
description: 了解核心 .NET 库中的以下 .NET 5.0 中断性变更：RuntimeInformation.FrameworkDescription 现在返回“.NET”而不是“.NET Core”。
ms.date: 11/01/2020
ms.openlocfilehash: 3925fb092135c26291e1e60b99f359974d21553c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759181"
---
# <a name="frameworkdescriptions-value-is-net-instead-of-net-core"></a>FrameworkDescription 的值是 .NET 而不是 .NET Core

<xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 现在返回“.NET”而不是“.NET Core”。

## <a name="change-description"></a>更改描述

在以前的 .NET 版本中，<xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 返回“.NET Core”作为描述字符串的一部分，例如 `.NET Core 3.1.1`。

从 .NET 5.0 开始，<xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 返回“.NET”作为描述字符串的一部分，例如 `.NET 5.0.0`。

## <a name="reason-for-change"></a>更改原因

对于 .NET 5，`netcoreapp` 替换为 `net`，作为简短的目标框架名字对象。 为了保持一致性，框架描述也进行了更新。 该更改只浮于表面，因为除了在 <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 属性中，`FrameworkName` 未在其他任何地方进行编码。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="recommended-action"></a>建议操作

更新搜索 <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription> 返回的字符串中“.NET Core”的所有代码。

## <a name="affected-apis"></a>受影响的 API

- <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription`

-->
