---
title: QuotedPairReader 类（System.Net）
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.Mail.QuotedPairReader
- System.Net.Mail.QuotedPairReader.CountQuotedChars
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 898c6e9d2d5dd02f3d5f9c096ad470b5dd445d1d
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2020
ms.locfileid: "86051306"
---
# <a name="quotedpairreader-class"></a>QuotedPairReader 类

确定在带引号的字符串中用引号（转义）括起来的字符。 无法继承此类。

```csharp
internal static class QuotedPairReader
```

> [!WARNING]
> 此类是内部的，不应在代码中直接使用。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="countquotedchars-method"></a>CountQuotedChars 方法

计算指定字符串中连续带引号的字符数（包括多个前面带引号的反斜杠）。 例如，给定的字符串 `a\\\b` 和索引 `4` ，该方法返回 `4` ，因为带有引号， `b` 因此是前面三个反斜杠。

```csharp
internal static int CountQuotedChars(string data, int index, bool permitUnicodeEscaping)
```

### <a name="parameters"></a>参数

- `data` <xref:System.String>

  用于对连续带引号字符进行计数的数据字符串。

- `index` <xref:System.Int32>

  指定字符串中的位置，最多包含和包括应对哪些连续的带引号字符进行计数。

- `permitUnicodeEscaping` <xref:System.Boolean>

  `true`允许转义 Unicode 字符;否则为 `false` 。

### <a name="return-value"></a>返回值

<xref:System.Int32?displayProperty=nameWithType>

`0`如果指定索引处的字符未转义，则为; 否则为。否则，则为，最多包含中的字符（包括字符） `index` 。

### <a name="exceptions"></a>例外

<xref:System.FormatException?displayProperty=nameWithType>

找到了一个转义的 Unicode 字符，但不允许这样做。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** 系统（System.dll）
