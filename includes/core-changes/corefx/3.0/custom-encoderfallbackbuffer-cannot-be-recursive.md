---
ms.openlocfilehash: 54ef49755dc0b9d1b821ae7999ab218626d455e1
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031976"
---
### <a name="custom-encoderfallbackbuffer-instances-cannot-fall-back-recursively"></a>自定义 EncoderFallbackBuffer 实例无法递归回退

自定义 <xref:System.Text.EncoderFallbackBuffer> 实例无法以递归方式回退。 <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> 的实现必须生成一个可转换为目标编码的字符序列。 否则会发生异常。

#### <a name="change-description"></a>更改描述

在字符到字节的转码操作期间，运行时将检测格式不正确或不可转换的 UTF-16 序列，并将这些字符提供给 <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> 方法。 `Fallback` 方法确定应将哪些字符替换为原始不可转换数据，并通过在循环中调用 <xref:System.Text.EncoderFallbackBuffer.GetNextChar%2A?displayProperty=nameWithType> 来释放这些字符。

然后，运行时尝试将这些替换字符转码为目标编码。 如果此操作成功，则运行时继续从原始输入字符串中的中断位置进行转码。

之前，<xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> 的自定义实现可以返回无法转换为目标编码的字符序列。 如果替换字符无法转码为目标编码，则运行时将使用替换字符再调用一次 <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> 方法，并要求 <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> 方法返回新的替换序列。 此过程将一直继续，直到运行时最终看到格式正确的、可转换的替换，或直到达到最大递归计数。

从 .NET Core 3.0 开始，<xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType> 的自定义实现必须返回可转换为目标编码的字符序列。 如果替换字符无法转码为目标编码，则引发 <xref:System.ArgumentException>。 运行时将不再对 <xref:System.Text.EncoderFallbackBuffer> 实例进行递归调用。

仅当满足以下所有三个条件时，此行为才适用：

- 运行时检测格式不正确的 UTF-16 序列或无法转换为目标编码的 UTF-16 序列。
- 已指定自定义 <xref:System.Text.EncoderFallback>。
- 自定义 <xref:System.Text.EncoderFallback> 尝试替换新的格式不正确的或无法转换的 UTF-16 序列。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

大多数开发人员都不需要执行任何操作。

如果应用程序使用自定义 <xref:System.Text.EncoderFallback> 和 <xref:System.Text.EncoderFallbackBuffer> 类，请确保 <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType> 的实现使用格式正确的 UTF-16 数据（该数据在运行时第一次调用 <xref:System.Text.EncoderFallbackBuffer.Fallback%2A> 方法时可直接转换为目标编码）填充回退缓冲区。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Text.EncoderFallbackBuffer.Fallback%2A?displayProperty=nameWithType>
- <xref:System.Text.EncoderFallbackBuffer.GetNextChar?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Text.EncoderFallbackBuffer.Fallback`
- `M:System.Text.EncoderFallbackBuffer.GetNextChar`

-->
