---
title: 如何确认字符串是有效的电子邮件格式
description: 阅读有关正则表达式如何在 .NET 中验证字符串是否为有效电子邮件格式的示例。
ms.date: 06/30/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, examples
- user input, examples
- Regex.IsMatch method
- regular expressions [.NET], examples
- examples [Visual Basic], strings
- IsValidEmail
- validation, email strings
- input, checking
- strings [.NET], examples [Visual Basic]
- email [.NET], validating
- IsMatch method
ms.assetid: 7536af08-4e86-4953-98a1-a8298623df92
ms.openlocfilehash: 88ff326e16ede6a422e9403b71905845014c4c25
ms.sourcegitcommit: 6d1ae17e60384f3b5953ca7b45ac859ec6d4c3a0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94982487"
---
# <a name="how-to-verify-that-strings-are-in-valid-email-format"></a>如何确认字符串是有效的电子邮件格式

下面的示例使用正则表达式来验证一个字符串是否为有效的电子邮件格式。

此正则表达式相对比实际上可用作电子邮件的表达式简单。 使用正则表达式来验证电子邮件，这对于确保电子邮件的结构正确是很有用的，但这不是验证电子邮件是否实际存在的替代方法。

✔️ 请使用小型正则表达式来检查电子邮件的有效结构。

✔️ 请将测试电子邮件发送到应用用户提供的地址。

❌ 请勿将正则表达式用作验证电子邮件的唯一方法。

如果尝试创建完美的正则表达式来验证电子邮件的结构是否正确，表达式会变得非常复杂，以至于很难进行调试或改进。 即使电子邮件的结构正确，正则表达式也无法验证其是否存在。 验证电子邮件的最佳方法是将测试电子邮件发送到该地址。

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="example"></a>示例

该示例定义 `IsValidEmail` 方法，如果字符串包含有效的电子邮件地址，则该方法返回 `true`；如果字符串不包含有效的电子邮件地址，但没有采取其他任何操作，该方法返回 `false`。

若要验证电子邮件地址是否有效，方法 `IsValidEmail` 将使用 <xref:System.Text.RegularExpressions.Regex.Replace%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.MatchEvaluator%29?displayProperty=nameWithType> 正则表达式模式调用 `(@)(.+)$` 方法将域名从电子邮件地址分离。 第三个参数是表示了处理和替换匹配文本的方法的 <xref:System.Text.RegularExpressions.MatchEvaluator> 委托。 正则表达式模式的解释如下。

| 模式 | 描述                                                                         |
|---------|-------------------------------------------------------------------------------------|
| `(@)`   | 匹配 @ 字符。 此部分是第一个捕获组。                           |
| `(.+)`  | 匹配任意字符的一个或多个匹配项。 此部分是第二个捕获组。 |
| `$`     | 在字符串的结尾结束匹配。                                             |

使用 @ 字符的域名已传递给 `DomainMapper` 方法，该方法使用 <xref:System.Globalization.IdnMapping> 类将 US-ASCII 字符范围外的 Unicode 字符转换为 Punycode。 如果 `invalid` 方法在域名中检测到任何无效字符，该方法还会将 `True` 标志设置为 <xref:System.Globalization.IdnMapping.GetAscii%2A?displayProperty=nameWithType> 。 该方法将冠以 @ 符号的 Punycode 域名返回给 `IsValidEmail` 方法。

> [!TIP]
> 建议使用简单的 `(@)(.+)$` 正则表达式模式对域进行规范化，然后返回一个值，指示该域是通过还是失败。 但本文中的示例介绍如何进一步使用正则表达式来验证电子邮件。 无论验证电子邮件的方式如何，你都应始终将测试电子邮件发送到该地址，以确保其存在。

然后 `IsValidEmail` 方法调用 <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%29?displayProperty=nameWithType> 方法验证该地址是否符合正则表达式模式。

`IsValidEmail` 方法只能确定电子邮件地址的格式是否有效，而不能验证电子邮件是否存在。 同样，`IsValidEmail` 方法不会验证顶级域名是否是 [IANA 根区域数据库](https://www.iana.org/domains/root/db)上列出的有效域名，这需执行查找操作。

:::code language="csharp" source="snippets/how-to-verify-that-strings-are-in-valid-email-format/csharp/RegexUtilities.cs":::

:::code language="vb" source="snippets/how-to-verify-that-strings-are-in-valid-email-format/vb/RegexUtilities.vb":::

在本例中，正则表达式模式 `^[^@\s]+@[^@\s]+\.[^@\s]+$` 按下表中的方式解释。 使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 标志编译正则表达式。

| 模式   | 描述                                                                              |
|-----------|------------------------------------------------------------------------------------------|
| `^`       | 从字符串的开头部分开始匹配。                                              |
| `[^@\s]+` | 匹配一次或多次出现的任何字符，@ 字符或空格除外。 |
| `@`       | 匹配 @ 字符。                                                                   |
| `[^@\s]+` | 匹配一次或多次出现的任何字符，@ 字符或空格除外。 |
| `\.`      | 匹配一个句点字符。                                                         |
| `[^@\s]+` | 匹配一次或多次出现的任何字符，@ 字符或空格除外。 |
| `$`       | 在字符串的结尾结束匹配。                                                  |

> [!IMPORTANT]
> 此正则表达式没有涵盖有效电子邮件地址的所有字符。 它作为示例提供，你可以按需对其进行扩展。

## <a name="see-also"></a>请参阅

- [.NET 正则表达式](regular-expressions.md)
- [对电子邮件地址进行验证的频率应为多久一次？](https://softwareengineering.stackexchange.com/questions/78353/how-far-should-one-take-e-mail-address-validation#78363)
