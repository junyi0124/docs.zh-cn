---
title: .NET 正则表达式中的反向引用构造
description: 了解如何通过在正则表达式中使用向后引用构造来识别重复的文本元素。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- backreferences
- constructs, backreference
- .NET regular expressions, backreference constructs
- regular expressions, backreference constructs
ms.assetid: 567a4b8d-0e79-49dc-8df9-f4b1aa376a2a
ms.openlocfilehash: 79702f266e7233c96fef6b6aa32a7e756589f49c
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94825255"
---
# <a name="backreference-constructs-in-regular-expressions"></a>正则表达式中的反向引用构造

反向引用提供了标识字符串中的重复字符或子字符串的方便途径。 例如，如果输入字符串包含某任意子字符串的多个匹配项，可以使用捕获组匹配第一个出现的子字符串，然后使用反向引用匹配后面出现的子字符串。

> [!NOTE]
> 单独语法用于引用替换字符串中命名的和带编号的捕获组。 有关更多信息，请参见 [替代](substitutions-in-regular-expressions.md)。

.NET 定义引用编号和命名捕获组的单独语言元素。 若要详细了解捕获组，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

## <a name="numbered-backreferences"></a>带编号的反向引用

带编号的反向引用使用以下语法：

`\` *number*

其中 *number* 是正则表达式中捕获组的序号位置。 例如，`\4` 匹配第四个捕获组的内容。 如果正则表达式模式中未定义 number  ，将会发生分析错误，并且正则表达式引擎会抛出 <xref:System.ArgumentException>。 例如，正则表达式 `\b(\w+)\s\1` 有效，因为 `(\w+)` 是表达式中的第一个也是唯一一个捕获组。 `\b(\w+)\s\2` 无效，该表达式会因为没有捕获组编号 `\2` 而引发自变量异常。 此外，如果 number 标识特定序号位置中的捕获组，但该捕获组已被分配了一个不同于其序号位置的数字名称，则正则表达式分析器还会引发 <xref:System.ArgumentException>。

请注意八进制转义代码（如 `\16`）和使用相同表示法的 `\`number  反向引用之间的不明确问题。 这种多义性可通过如下方式解决：

- 表达式 `\1` 到 `\9` 始终解释为反向应用，而不是八进制代码。

- 如果多位表达式的第一个数字是 8 或 9（如 `\80` 或 `\91`），该表达式将解释为文本。

- 对于编号为 `\10` 或更大值的表达式，如果存在与该编号对应的反向引用，则将该表达式视为反向引用；否则，将这些表达式解释为八进制代码。

- 如果正则表达式包含对未定义的组成员的反向引用，将会发生分析错误，并且正则表达式引擎会抛出 <xref:System.ArgumentException>。

如果存在不明确问题，可以使用 `\k<`name  `>` 表示法，此表示法非常明确，不会与八进制字符代码混淆。 同样，诸如 `\xdd` 的十六进制代码也是明确的，不会与反向引用混淆。

下面的示例查找字符串中双写的单词字符。 它定义一个由下列元素组成的正则表达式 `(\w)\1`。

|元素|说明|
|-------------|-----------------|
|`(\w)`|匹配单词字符，并将其分配给第一个捕获组。|
|`\1`|匹配值与第一捕获组相同的下一个字符。|

[!code-csharp[RegularExpressions.Language.Backreferences#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference1.cs#1)]
[!code-vb[RegularExpressions.Language.Backreferences#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference1.vb#1)]

## <a name="named-backreferences"></a>命名的反向引用

使用以下语法定义命名的反向引用：

`\k<` *name* `>`

或：

`\k'` *name* `'`

其中，*name* 是正则表达式模式中定义的捕获组的名称。 如果正则表达式模式中未定义 name  ，将会发生分析错误，并且正则表达式引擎会抛出 <xref:System.ArgumentException>。

下面的示例查找字符串中双写的单词字符。 它定义一个由下列元素组成的正则表达式 `(?<char>\w)\k<char>`。

|元素|说明|
|-------------|-----------------|
|`(?<char>\w)`|匹配字词字符，并将结果分配到 `char` 捕获组。|
|`\k<char>`|匹配下一个与 `char` 捕获组的值相同的字符。|

[!code-csharp[RegularExpressions.Language.Backreferences#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference2.cs#2)]
[!code-vb[RegularExpressions.Language.Backreferences#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference2.vb#2)]

## <a name="named-numeric-backreferences"></a>已命名数值的反向引用

在具有 `\k` 的已命名反向引用中，name 也可以是 number 的字符串表示形式  。 例如，下面的示例使用正则表达式 `(?<2>\w)\k<2>` 查找字符串中双写的单词字符。 在此情况下，该示例定义了显式命名为“2”的捕获组，反向引用相应地命名为“2”。

[!code-csharp[RegularExpressions.Language.Backreferences#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference3.cs#3)]
[!code-vb[RegularExpressions.Language.Backreferences#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference3.vb#3)]

如果 name 是 number 的字符串表示形式，且没有捕获组具有该名称，`\k<` name `>` 与反向引用 `\`number 相同，其中 number 是捕获的序号位置。 在以下示例中，有名为 `char` 的单个捕获组。 反向引用构造将其称为 `\k<1>`。 正如示例中的输出所示，由于 `char` 是第一个捕获组，所以对 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 的调用成功。

[!code-csharp[Ordinal.Backreference](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference6.cs)]
[!code-vb[Ordinal.BackReference](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference6.vb)]

但是，如果 name 是 number 的字符串表示形式，并且已向该位置中的捕获组明确分配了数字名称，正则表达式分析器无法通过其序号位置识别捕获组  。 相反，它会引发 <xref:System.ArgumentException>。 以下示例中的唯一捕获组名为“2”。 由于 `\k` 结构用于定义名为“1”的反向引用，因此正则表达式分析器无法识别第一个捕获组并引发异常。

[!code-csharp[Ordinal.Backreference](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference7.cs)]
[!code-vb[Ordinal.BackReference](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference7.vb)]

## <a name="what-backreferences-match"></a>反向引用匹配什么内容

反向引用引用组的最新定义（从左向右匹配时，最靠近左侧的定义）。 当组建立多个捕获时，反向引用会引用最新的捕获。

下面的示例包含正则表达式模式 `(?<1>a)(?<1>\1b)*`，该模式重新定义 \1 命名组。 下表描述了正则表达式中的每个模式。

|模式|说明|
|-------------|-----------------|
|`(?<1>a)`|匹配字符“a”，并将结果分配到 `1` 捕获组。|
|`(?<1>\1b)*`|匹配 `1` 组的 0 更大发生次数以及“b”，并将结果分配到 `1` 捕获组。|

[!code-csharp[RegularExpressions.Language.Backreferences#4](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference4.cs#4)]
[!code-vb[RegularExpressions.Language.Backreferences#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference4.vb#4)]

在比较正则表达式与输入字符串（“aababb”）时，正则表达式引擎执行以下操作：

1. 从该字符串的开头开始，成功将“a”与表达式 `(?<1>a)` 匹配。 此时，`1` 组的值为“a”。

2. 继续匹配第二个字符，成功将字符串“ab”与表达式 `\1b` 或“ab”匹配。 然后，将结果“ab”分配到 `\1`。

3. 继续匹配第四个字符。 表达式 `(?<1>\1b)*` 要匹配零次或多次，因此会成功将字符串“abb”与表达式 `\1b` 匹配。 然后，将结果“abb”分配回到 `\1`。

在本示例中，`*` 是循环限定符 -- 它将被重复计算，直到正则表达式引擎不能与它定义的模式匹配为止。 循环限定符不会清除组定义。

如果某个组尚未捕获任何子字符串，则对该组的反向引用是不确定的，永远不会匹配。 下面展示了正则表达式模式 `\b(\p{Lu}{2})(\d{2})?(\p{Lu}{2})\b` 的定义：

|模式|说明|
|-------------|-----------------|
|`\b`|在单词边界处开始匹配。|
|`(\p{Lu}{2})`|匹配两个大写字母。 这是第一个捕获组。|
|`(\d{2})?`|匹配两个十进制数的零个或一个匹配项。 这是第二个捕获组。|
|`(\p{Lu}{2})`|匹配两个大写字母。 这是第三个捕获组。|
|`\b`|在单词边界处结束匹配。|

输入字符串可以匹配此正则表达式，即使第二个捕获组定义的两个十进制数字都不存在。 下面的示例显示了即使匹配成功，也仍会在两个成功的捕获组之间找到空捕获组。

[!code-csharp[RegularExpressions.Language.Backreferences#5](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference5.cs#5)]
[!code-vb[RegularExpressions.Language.Backreferences#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference5.vb#5)]

## <a name="see-also"></a>另请参阅

- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
