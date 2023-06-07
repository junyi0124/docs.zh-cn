---
ms.openlocfilehash: 298cb441bf9fe7daddb30c85f9d7366dc972628c
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031980"
---
### <a name="replacing-ill-formed-utf-8-byte-sequences-follows-unicode-guidelines"></a>替换格式错误的 UTF-8 字节序列将遵循 Unicode 准则

<xref:System.Text.UTF8Encoding> 类在字节到字符转码操作期间遇到格式错误的 UTF-8 字节序列时，它将在输出字符串中用“�”（U+FFFD 替换字符）替换该序列。 .NET Core 3.0 与以前版本的 .NET Core 和 .NET Framework 的不同之处在于，在转码操作期间按照 Unicode 最佳做法执行此替换。

这是在整个 .NET 中改进 UTF-8 处理的较大工作量（包括通过新的 <xref:System.Text.Unicode.Utf8?displayProperty=nameWithType> 和 <xref:System.Text.Rune?displayProperty=nameWithType> 类型）的一部分。 为 <xref:System.Text.UTF8Encoding> 类型提供了改进的错误处理机制，以便生成与新引入的类型一致的输出。

#### <a name="change-description"></a>更改描述

从 .NET Core 3.0 开始，当将字节转码为字符时，<xref:System.Text.UTF8Encoding> 类会根据 Unicode 最佳做法执行字符替换。 [Unicode 标准 12.0 版第 3.9 节 (PDF)](https://www.unicode.org/versions/Unicode12.0.0/ch03.pdf) 的  “U+FFFD 替换最大子部分”标题中描述了使用的替换机制。

仅当  输入字节序列包含格式错误的 UTF-8 数据时，此行为才适用。 此外，如果已通过 `throwOnInvalidBytes: true` 构造了 <xref:System.Text.UTF8Encoding> 实例，则 `UTF8Encoding` 实例将继续对无效输入引发，而不是执行 U+FFFD 替换。 有关 `UTF8Encoding` 构造函数的详细信息，请参阅 <xref:System.Text.UTF8Encoding.%23ctor(System.Boolean,System.Boolean)>。

下表通过无效的 3 字节输入说明了此更改的影响：

| 格式无效的 3 字节输入 | .NET Core 3.0 之前的输出          | 从 .NET Core 3.0 开始的输出        |
|-------------------------|--------------------------------------|-------------------------------------------|
| `[ ED A0 90 ]`          | `[ FFFD FFFD ]`（2 字符输出） | `[ FFFD FFFD FFFD ]`（3 字符输出） |

根据以前链接的 Unicode 标准 PDF 的表 3-9  ，此 3 字符输出为首选输出。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

开发人员一方不需要执行任何操作。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Text.UTF8Encoding.GetCharCount%2A?displayProperty=nameWithType>
- <xref:System.Text.UTF8Encoding.GetChars%2A?displayProperty=nameWithType>
- <xref:System.Text.UTF8Encoding.GetString(System.Byte[],System.Int32,System.Int32)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Text.UTF8Encoding.GetCharCount`
- `Overload:System.Text.UTF8Encoding.GetChars`
- `M:System.Text.UTF8Encoding.GetString(System.Byte[],System.Int32,System.Int32)`

-->
