---
title: 中断性变更：向量<T> 引发 NotSupportedException
description: 了解核心 .NET 库中的以下 .NET 5.0 中断性变更：对于不支持的类型参数，Vector<T> 始终引发异常。
ms.date: 11/01/2020
ms.openlocfilehash: 63db7c6b720735b180ed11098227b31a14008f74
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759160"
---
# <a name="vectort-always-throws-notsupportedexception-for-unsupported-types"></a>对于不支持的类型，Vector\<T> 始终引发 NotSupportedException

现在，对于不支持的类型参数，<xref:System.Numerics.Vector%601?displayProperty=nameWithType> 始终引发 <xref:System.NotSupportedException>。

## <a name="change-description"></a>更改描述

以前，如果 `T` 是[不支持的类型](#unsupported-types)，<xref:System.Numerics.Vector%601> 的成员将不会始终引发 <xref:System.NotSupportedException>。 并非总是因支持硬件加速的代码路径而引发异常。 例如，`Vector<bool> + Vector<bool>` 返回了 `default`，而不是在没有硬件加速的平台（如 ARM32）上引发异常。 对于不支持的类型，<xref:System.Numerics.Vector%601> 成员在不同的平台和硬件配置中表现出不一致的行为。

从 .NET 5.0 开始，如果 `T` 不是受支持的类型，则 <xref:System.Numerics.Vector%601> 成员始终对所有硬件配置引发 <xref:System.NotSupportedException>。

### <a name="unsupported-types"></a>不支持的类型

<xref:System.Numerics.Vector%601> 的类型参数支持的类型如下：

- `byte`
- `sbyte`
- `short`
- `ushort`
- `int`
- `uint`
- `long`
- `ulong`
- `float`
- `double`

支持的类型并无变化，但将来可能会更改。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="recommended-action"></a>建议操作

不要对 <xref:System.Numerics.Vector%601> 的类型参数使用不受支持的类型。

## <a name="affected-apis"></a>受影响的 API

- <xref:System.Numerics.Vector%601?displayProperty=fullName> 及其所有成员

<!--

#### Category

Core .NET libraries

### Affected APIs

- ``T:System.Numerics.Vector`1``

-->
