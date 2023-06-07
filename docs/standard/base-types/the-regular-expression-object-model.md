---
title: 正则表达式对象模型
description: 查看 .NET 中的正则表达式对象模型。 使用与匹配、分组和捕获相关的正则表达式引擎、对象和集合。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- searching with regular expressions, backreferences
- Regex class
- Match class
- pattern-matching with regular expressions, backreferences
- .NET regular expressions, classes
- CaptureCollection class
- Group class
- characters [.NET], backreferences
- substrings
- .NET regular expressions, backreferences
- searching with regular expressions, classes
- backreferences
- Capture class
- repeating groups of characters
- MatchCollection class
- parsing text with regular expressions, backreferences
- regular expressions [.NET]
- characters [.NET], regular expressions
- classes [.NET], regular expression
- regular expressions [.NET], classes
- characters [.NET], metacharacters
- metacharacters, regular expression classes
- metacharacters, backreferences
- parsing text with regular expressions, classes
- regular expressions [.NET], backreferences
- strings [.NET], regular expressions
- pattern-matching with regular expressions, classes
- GroupCollection class
ms.assetid: 49a21470-64ca-4b5a-a889-8e24e3c0af7e
ms.openlocfilehash: 996a8cca8222e3de6517ee6fa7cef3c4f44fc5a9
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734176"
---
# <a name="the-regular-expression-object-model"></a>正则表达式对象模型

<a name="introduction"></a> 本主题介绍了处理 .NET 正则表达式时使用的对象模型。 它包含下列部分：  
  
- [正则表达式引擎](#Engine)  
  
- [MatchCollection 和 Match 对象](#Match_and_MCollection)  
  
- [组集合](#GroupCollection)  
  
- [捕获组](#the_captured_group)  
  
- [捕获集合](#CaptureCollection)  
  
- [单个捕获](#the_individual_capture)  
  
<a name="Engine"></a>

## <a name="the-regular-expression-engine"></a>正则表达式引擎  

 .NET 中的正则表达式引擎由 <xref:System.Text.RegularExpressions.Regex> 类表示。 正则表达式引擎负责分析和编译正则表达式，并执行用于将正则表达式模式与输入字符串相匹配的操作。 此引擎是 .NET 正则表达式对象模型中的主要组件。  
  
 可以通过以下两种方式之一使用正则表达式引擎：  
  
- 通过调用 <xref:System.Text.RegularExpressions.Regex> 类的静态方法。 方法参数包含输入字符串和正则表达式模式。 正则表达式引擎会缓存静态方法调用中使用的正则表达式，这样一来，重复调用使用同一正则表达式的静态正则表达式方法将提供相对良好的性能。  
  
- 通过实例化 <xref:System.Text.RegularExpressions.Regex> 对象，采用的方式是将一个正则表达式传递给类构造函数。 在此情况下，<xref:System.Text.RegularExpressions.Regex> 对象是不可变的（只读），它表示一个与单个正则表达式紧密耦合的正则表达式引擎。 由于未对 <xref:System.Text.RegularExpressions.Regex> 实例使用的正则表达式进行缓存，因此不应使用同一正则表达式实例化 <xref:System.Text.RegularExpressions.Regex> 对象多次。  
  
 可以调用 <xref:System.Text.RegularExpressions.Regex> 类的方法来执行下列操作：  
  
- 确定字符串是否与正则表达式模式匹配。  
  
- 提取单个匹配项或第一个匹配项。  
  
- 提取所有匹配项。  
  
- 替换匹配的子字符串。  
  
- 将单个字符串拆分成一个字符串数组。  
  
 以下各部分对这些操作进行了描述。  
  
### <a name="matching-a-regular-expression-pattern"></a>匹配正则表达式模式  

 如果字符串与此模式匹配，则 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法返回 `true`；如果字符串与此模式不匹配，则该方法返回 `false`。 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A> 方法通常用于验证字符串输入。 例如，下面的代码将确保字符串与有效的美国社会保障号匹配。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/validate1.cs#1)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/validate1.vb#1)]  
  
 正则表达式模式 `^\d{3}-\d{2}-\d{4}$` 的含义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`^`|匹配输入字符串的开头部分。|  
|`\d{3}`|匹配三个十进制数字。|  
|`-`|匹配连字符。|  
|`\d{2}`|匹配两个十进制数字。|  
|`-`|匹配连字符。|  
|`\d{4}`|匹配四个十进制数字。|  
|`$`|匹配输入字符串的末尾部分。|  
  
### <a name="extracting-a-single-match-or-the-first-match"></a>提取单个匹配项或第一个匹配项  

 <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> 方法返回一个 <xref:System.Text.RegularExpressions.Match> 对象，该对象包含有关与正则表达式模式匹配的第一个子字符串的信息。 如果 `Match.Success` 属性返回 `true`，则表示已找到一个匹配项，可以通过调用 <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法来检索有关后续匹配项的信息。 这些方法调用可以继续进行，直到 `Match.Success` 属性返回 `false`。 例如，下面的代码使用 <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType> 方法查找重复的单词在字符串中的第一个匹配项。 然后，此代码调用 <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法查找任何其他匹配项。 该示例将在每次调用方法后检查 `Match.Success` 属性以确定当前匹配是否成功，并确定是否应接着调用 <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match1.cs#2)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match1.vb#2)]  
  
 正则表达式模式 `\b(\w+)\W+(\1)\b` 的含义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`(\w+)`|匹配一个或多个单词字符。 这是第一个捕获组。|  
|`\W+`|匹配一个或多个非单词字符。|  
|`(\1)`|与第一个捕获的字符串匹配。 这是第二个捕获组。|  
|`\b`|在单词边界处结束匹配。|  
  
### <a name="extracting-all-matches"></a>提取所有匹配项  

 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> 方法返回一个 <xref:System.Text.RegularExpressions.MatchCollection> 对象，该对象包含有关正则表达式引擎在输入字符串中找到的所有匹配项的信息。 例如，可重写上一示例以调用 <xref:System.Text.RegularExpressions.Regex.Matches%2A> 方法，而不是调用 <xref:System.Text.RegularExpressions.Regex.Match%2A> 和 <xref:System.Text.RegularExpressions.Match.NextMatch%2A> 方法。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/matches1.cs#3)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/matches1.vb#3)]  
  
### <a name="replacing-a-matched-substring"></a>替换匹配的子字符串  

 <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> 方法会将与正则表达式模式匹配的每个子字符串替换为指定的字符串或正则表达式模式，并返回进行了替换的整个输入字符串。 例如，下面的代码在字符串的十进制数字前添加了美国货币符号。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/replace1.cs#4)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/replace1.vb#4)]  
  
 正则表达式模式 `\b\d+\.\d{2}\b` 的含义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`\d+`|匹配一个或多个十进制数字。|  
|`\.`|匹配句点。|  
|`\d{2}`|匹配两个十进制数字。|  
|`\b`|在单词边界处结束匹配。|  
  
 替换模式 `$$$&` 的含义如下表所示。  
  
|模式|替换字符串|  
|-------------|------------------------|  
|`$$`|美元符号 ($) 字符。|  
|`$&`|整个匹配的子字符串。|  
  
### <a name="splitting-a-single-string-into-an-array-of-strings"></a>将单个字符串拆分成一个字符串数组  

 <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> 方法在由正则表达式匹配项定义的位置拆分输入字符串。 例如，下面的代码将编号列表中的项置于字符串数组中。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/split1.cs#5)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/split1.vb#5)]  
  
 正则表达式模式 `\b\d{1,2}\.\s` 的含义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`\d{1,2}`|匹配一个或两个十进制数字。|  
|`\.`|匹配句点。|  
|`\s`|与空白字符匹配。|  
  
<a name="Match_and_MCollection"></a>

## <a name="the-matchcollection-and-match-objects"></a>MatchCollection 和 Match 对象  

 Regex 方法返回作为正则表达式对象模型的一部分的两个对象：<xref:System.Text.RegularExpressions.MatchCollection> 对象和 <xref:System.Text.RegularExpressions.Match> 对象。  
  
<a name="the_match_collection"></a>

### <a name="the-match-collection"></a>Match 集合  

 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> 方法返回一个 <xref:System.Text.RegularExpressions.MatchCollection> 对象，该对象包含多个 <xref:System.Text.RegularExpressions.Match> 对象，这些对象表示正则表达式引擎在输入字符串中找到的所有匹配项（其顺序为这些匹配项在输入字符串中的显示顺序）。 如果没有匹配项，则此方法将返回一个不包含任何成员的 <xref:System.Text.RegularExpressions.MatchCollection> 对象。 利用 <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> 属性，你可以按照索引（从零到将 <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType> 属性的值减 1 所得的值）访问集合中的各个成员。 <xref:System.Text.RegularExpressions.MatchCollection.Item%2A> 是集合的索引器（在 C# 中）和默认属性（在 Visual Basic 中）。  
  
 默认情况下，调用 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> 方法会使用延迟计算来填充 <xref:System.Text.RegularExpressions.MatchCollection> 对象。 访问需要完全填充的集合的属性（如 <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType> 和 <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> 属性）可能会降低性能。 因此，建议你使用由 <xref:System.Collections.IEnumerator> 方法返回的 <xref:System.Text.RegularExpressions.MatchCollection.GetEnumerator%2A?displayProperty=nameWithType> 对象访问该集合。 各种语言都提供了用于包装集合的 <xref:System.Collections.IEnumerator> 接口的构造（如 Visual Basic 中的 `For Each` 和 C# 中的 `foreach`）。  
  
 下面的示例使用 <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%29?displayProperty=nameWithType> 方法将在输入字符串中找到的所有匹配项填充到 <xref:System.Text.RegularExpressions.MatchCollection> 对象中。 此示例枚举了该集合，将匹配项复制到字符串数组并将字符位置记录在整数数组中。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/matchcollection1.cs#6)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/matchcollection1.vb#6)]  
  
<a name="the_match"></a>

### <a name="the-match"></a>Match 类  

 <xref:System.Text.RegularExpressions.Match> 类表示单个正则表达式匹配项的结果。 可以通过两种方式访问 <xref:System.Text.RegularExpressions.Match> 对象：  
  
- 通过从 <xref:System.Text.RegularExpressions.MatchCollection> 方法返回的 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> 对象检索这些对象。 若要检索各个 <xref:System.Text.RegularExpressions.Match> 对象，请通过使用 `foreach`（在 C# 中）或 `For Each`...`Next`（在 Visual Basic 中）构造循环访问集合；或者使用 <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> 属性以按索引或名称检索特定的 <xref:System.Text.RegularExpressions.Match> 对象。 也可以通过按索引（从零到将集合中的对象数减去 1 所得的值）循环访问集合来检索集合中的各个 <xref:System.Text.RegularExpressions.Match> 对象。 但是，此方法不使用延迟计算，因为它将访问 <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType> 属性。  
  
     下面的示例通过使用 <xref:System.Text.RegularExpressions.Match> 或 <xref:System.Text.RegularExpressions.MatchCollection>...`foreach` 构造循环访问集合，来从 `For Each` 对象中检索各个 `Next` 对象。 正则表达式只是与输入字符串中的字符串“abc”匹配。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match2.cs#7)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match2.vb#7)]  
  
- 通过调用 <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> 方法，此方法返回一个 <xref:System.Text.RegularExpressions.Match> 对象，该对象表示字符串中的第一个匹配项或字符串的一部分。 可以通过检索 `Match.Success` 属性的值确定是否已找到匹配项。 若要检索表示后续匹配项的 <xref:System.Text.RegularExpressions.Match> 对象，请重复调用 <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法，直到返回的 `Success` 对象的 <xref:System.Text.RegularExpressions.Match> 属性为 `false`。  
  
     下面的示例使用 <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType> 和 <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法来匹配输入字符串中的字符串“abc”。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match3.cs#8)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match3.vb#8)]  
  
 <xref:System.Text.RegularExpressions.Match> 类的以下两个属性都将返回集合对象：  
  
- <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性返回一个 <xref:System.Text.RegularExpressions.GroupCollection> 对象，该对象包含有关与正则表达式模式中的捕获组匹配的子字符串的信息。  
  
- `Match.Captures` 属性返回一个 <xref:System.Text.RegularExpressions.CaptureCollection> 对象，该对象的使用是有限制的。 不会为其 `Success` 属性为 `false` 的 <xref:System.Text.RegularExpressions.Match> 的对象填充集合。 否则，它将包含一个 <xref:System.Text.RegularExpressions.Capture> 对象，该对象具有的信息与 <xref:System.Text.RegularExpressions.Match> 对象具有的信息相同。  
  
 有关这些对象的更多信息，请参阅本主题后面的[组集合](#GroupCollection)和[捕获集合](#CaptureCollection)部分。  
  
 <xref:System.Text.RegularExpressions.Match> 类的另外两个属性提供了有关匹配项的信息。 `Match.Value` 属性返回输入字符串中与正则表达式模式匹配的子字符串。 `Match.Index` 属性返回输入字符串中匹配的字符串的起始位置（从零开始）。  
  
 <xref:System.Text.RegularExpressions.Match> 类还具有两个模式匹配方法：  
  
- <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法查找位于由当前的 <xref:System.Text.RegularExpressions.Match> 对象表示的匹配项之后的匹配项，并返回表示该匹配项的 <xref:System.Text.RegularExpressions.Match> 对象。  
  
- <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 方法对匹配的字符串执行指定的替换操作并返回相应结果。  
  
 下面的示例使用 <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 方法在每个包含两个小数位的数字前预置一个 $ 符号和一个空格。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/result1.cs#9)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/result1.vb#9)]  
  
 正则表达式模式 `\b\d+(,\d{3})*\.\d{2}\b` 的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`\d+`|匹配一个或多个十进制数字。|  
|`(,\d{3})*`|匹配零个或多个以下模式：一个逗号后跟三个十进制数字。|  
|`\.`|匹配小数点字符。|  
|`\d{2}`|匹配两个十进制数字。|  
|`\b`|在单词边界处结束匹配。|  
  
 替换模式 `$$ $&` 指示匹配的子字符串应由美元符号 ($)（`$$` 模式）、空格和匹配项的值（`$&` 模式）替换。  
  
 [返回页首](#introduction)  
  
<a name="GroupCollection"></a>

## <a name="the-group-collection"></a>组集合  

 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性返回一个 <xref:System.Text.RegularExpressions.GroupCollection> 对象，该对象包含多个 <xref:System.Text.RegularExpressions.Group> 对象，这些对象表示单个匹配项中的捕获的组。 集合中的第一个 <xref:System.Text.RegularExpressions.Group> 对象（位于索引 0 处）表示整个匹配项。 此对象后面的每个对象均表示一个捕获组的结果。  
  
 可以使用 <xref:System.Text.RegularExpressions.Group> 属性检索集合中的各个 <xref:System.Text.RegularExpressions.GroupCollection.Item%2A?displayProperty=nameWithType> 对象。 可以在集合中按未命名组的序号位置来检索未命名组，也可以按命名组的名称或序号位置来检索命名组。 未命名捕获将首先在集合中显示，并将按照未命名捕获在正则表达式模式中出现的顺序从左至右对它们进行索引。 在对未命名捕获进行索引后，将按照命名捕获在正则表达式模式中出现的顺序从左至右对它们进行索引。 若要确定在特定的正则表达式匹配方法返回的集合中哪些编号的组可用，可以调用实例 <xref:System.Text.RegularExpressions.Regex.GetGroupNumbers%2A?displayProperty=nameWithType> 方法。 若要确定集合中哪些命名的组可用，可以调用实例 <xref:System.Text.RegularExpressions.Regex.GetGroupNames%2A?displayProperty=nameWithType> 方法。 这两种方法在分析通过任何正则表达式找到的匹配的常规用途例程中都特别有用。  
  
 <xref:System.Text.RegularExpressions.GroupCollection.Item%2A?displayProperty=nameWithType> 属性是集合的索引器（在 C# 中）和集合对象的默认属性（在 Visual Basic 中）。 这表示可以按索引（对于命名组，可以按名称）访问各个 <xref:System.Text.RegularExpressions.Group> 对象，如下所示：  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/groupsyntax1.cs#13)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/groupsyntax1.vb#13)]  
  
 下面的示例定义一个正则表达式，该表达式使用分组构造捕获日期的年、月和日部分。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/groupcollection1.cs#10)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/groupcollection1.vb#10)]  
  
 正则表达式模式 `\b(\w+)\s(\d{1,2}),\s(\d{4})\b` 的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始匹配。|  
|`(\w+)`|匹配一个或多个单词字符。 这是第一个捕获组。|  
|`\s`|与空白字符匹配。|  
|`(\d{1,2})`|匹配一个或两个十进制数字。 这是第二个捕获组。|  
|`,`|匹配逗号。|  
|`\s`|与空白字符匹配。|  
|`(\d{4})`|匹配四个十进制数字。 这是第三个捕获组。|  
|`\b`|在单词边界处结束匹配。|  
  
 [返回页首](#introduction)  
  
<a name="the_captured_group"></a>

## <a name="the-captured-group"></a>捕获的组  

 <xref:System.Text.RegularExpressions.Group> 类表示来自单个捕获组的结果。 表示正则表达式中定义的捕获组的组对象由 <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> 属性所返回的 <xref:System.Text.RegularExpressions.GroupCollection> 对象的 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性返回。 <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> 属性是索引器（在 C# 中）和 <xref:System.Text.RegularExpressions.Group> 类的默认属性（在 Visual Basic 中）。 也可以使用 `foreach` 或 `For Each` 构造循环访问集合，从而检索各个成员。 有关示例，请参见上一部分。  
  
 下面的示例使用嵌套的分组构造来将子字符串捕获到组中。 正则表达式模式 `(a(b))c` 将匹配字符串“abc”。 它会将子字符串“ab”分配给第一个捕获组，并将子字符串“b”分配给第二个捕获组。  
  
 [!code-csharp[RegularExpressions.Classes#6](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#6)]
 [!code-vb[RegularExpressions.Classes#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#6)]  
  
 下面的示例使用命名的分组构造，从包含“DATANAME:VALUE”格式的数据的字符串中捕获子字符串，正则表达式通过冒号 (:) 拆分数据。  
  
 [!code-csharp[RegularExpressions.Classes#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#8)]
 [!code-vb[RegularExpressions.Classes#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#8)]  
  
 正则表达式模式 `^(?<name>\w+):(?<value>\w+)` 的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`^`|从输入字符串的开头部分开始匹配。|  
|`(?<name>\w+)`|匹配一个或多个单词字符。 此捕获组的名称为 `name`。|  
|`:`|匹配冒号。|  
|`(?<value>\w+)`|匹配一个或多个单词字符。 此捕获组的名称为 `value`。|  
  
 <xref:System.Text.RegularExpressions.Group> 类的属性提供有关捕获组的信息：`Group.Value` 属性包含捕获子字符串，`Group.Index` 属性在输入文本中指示捕获组的起始位置，`Group.Length` 属性包含捕获文本的长度，`Group.Success` 属性指示子字符串是否与捕获组所定义的模式匹配。  
  
 通过对组应用量符（有关详细信息，请参阅[量符](quantifiers-in-regular-expressions.md)），可以每捕获组修改一个捕获的关系，具体方式分为以下两种：  
  
- 如果对组应用 `*` 或 `*?` 限定符（将指定零个或多个匹配项），则捕获组在输入字符串中可能没有匹配项。 在没有捕获的文本时，将如下表所示设置 <xref:System.Text.RegularExpressions.Group> 对象的属性。  
  
    |组属性|“值”|  
    |--------------------|-----------|  
    |`Success`|`false`|  
    |`Value`|<xref:System.String.Empty?displayProperty=nameWithType>|  
    |`Length`|0|  
  
     下面的示例进行了这方面的演示。 在正则表达式模式 `aaa(bbb)*ccc` 中，可以匹配第一个捕获组（子字符串“bbb”）零次或多次。 由于输入字符串“aaaccc”与此模式匹配，因此该捕获组没有匹配项。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/nocapture1.cs#11)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/nocapture1.vb#11)]  
  
- 限定符可以匹配由捕获组定义的模式的多个匹配项。 在此情况下，`Value` 对象的 `Length` 和 <xref:System.Text.RegularExpressions.Group> 属性仅包含有关最后捕获的子字符串的信息。 例如，下面的正则表达式匹配以句点结束的单个句子。 此表达式使用两个分组构造：第一个分组构造捕获单个单词和空白字符；第二个分组构造捕获单个单词。 如示例中的输出所示，虽然正则表达式成功捕获整个句子，但第二个捕获组仅捕获了最后一个单词。  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/lastcapture1.cs#12)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/lastcapture1.vb#12)]  
  
 [返回页首](#introduction)  
  
<a name="CaptureCollection"></a>

## <a name="the-capture-collection"></a>捕获集合  

 <xref:System.Text.RegularExpressions.Group> 对象仅包含有关最后一个捕获的信息。 但仍可从 <xref:System.Text.RegularExpressions.CaptureCollection> 属性返回的 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 对象中获取由捕获组生成的整个捕获集。 集合中的每个成员均为一个表示由该捕获组生成的捕获的 <xref:System.Text.RegularExpressions.Capture> 对象，这些对象按被捕获的顺序排列（因而也就是遵循在输入字符串中按从左至右匹配捕获的字符串的顺序）。 可以通过以下两种方式之一来检索集合中的各个 <xref:System.Text.RegularExpressions.Capture> 对象：  
  
- 通过使用构造循环访问集合，如 `foreach` 构造（在 C# 中）或 `For Each` 构造（在 Visual Basic 中）。  
  
- 通过使用 <xref:System.Text.RegularExpressions.CaptureCollection.Item%2A?displayProperty=nameWithType> 属性按索引检索特定对象。 <xref:System.Text.RegularExpressions.CaptureCollection.Item%2A> 属性是 <xref:System.Text.RegularExpressions.CaptureCollection> 对象的默认属性（在 Visual Basic 中）或索引器（在 C# 中）。  
  
 如果未对捕获组应用限定符，则 <xref:System.Text.RegularExpressions.CaptureCollection> 对象将包含一个 <xref:System.Text.RegularExpressions.Capture> 对象，但该对象的作用不大，因为它提供的是有关与其 <xref:System.Text.RegularExpressions.Group> 对象相同的匹配项的信息。 如果对一个捕获组应用限定符，则 <xref:System.Text.RegularExpressions.CaptureCollection> 对象将包含该捕获组所生成的所有捕获，并且集合的最后一个成员将表示与 <xref:System.Text.RegularExpressions.Group> 对象相同的捕获。  
  
 例如，如果使用正则表达式模式 `((a(b))c)+`（其中，+ 限定符指定一个或多个匹配项）捕获字符串“abcabcabc”中的匹配项，则每个 <xref:System.Text.RegularExpressions.CaptureCollection> 对象的 <xref:System.Text.RegularExpressions.Group> 对象都将包含三个成员。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/capturecollection1.cs#14)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/capturecollection1.vb#14)]  
  
 下面的示例使用正则表达式 `(Abc)+` 来在字符串“XYZAbcAbcAbcXYZAbcAb”中查找字符串“Abc”的一个或多个连续匹配项。 该示例演示了使用 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 属性来返回多组捕获的子字符串。  
  
 [!code-csharp[RegularExpressions.Classes#5](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#5)]
 [!code-vb[RegularExpressions.Classes#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#5)]  
  
 [返回页首](#introduction)  
  
<a name="the_individual_capture"></a>

## <a name="the-individual-capture"></a>单个捕获  

 <xref:System.Text.RegularExpressions.Capture> 类包含来自单个子表达式捕获的结果。 <xref:System.Text.RegularExpressions.Capture.Value%2A?displayProperty=nameWithType> 属性包含匹配的文本，而 <xref:System.Text.RegularExpressions.Capture.Index%2A?displayProperty=nameWithType> 属性指示匹配的子字符串在输入字符串中的起始位置（从零开始）。  
  
 下面的示例分析针对选定城市的温度的输入字符串。 逗号（“,”）用于将城市与其温度分隔开，而分号（“;”）用于将每个城市的数据分隔开。 整个输入字符串表示一个匹配项。 在用于分析字符串的正则表达式模式 `((\w+(\s\w+)*),(\d+);)+` 中，城市名称将分配给第二个捕获组，而温度将分配到第四个捕获组。  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/capture1.cs#16)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/capture1.vb#16)]  
  
 该正则表达式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\w+`|匹配一个或多个单词字符。|  
|`(\s\w+)*`|匹配零个或多个以下模式：一个空白字符后跟一个或多个单词字符。 此模式匹配包含多个单词的城市名称。 这是第三个捕获组。|  
|`(\w+(\s\w+)*)`|匹配以下模式：一个或多个单词字符，后跟零个或多个一个空白字符与一个或多个单词字符的组合。 这是第二个捕获组。|  
|`,`|匹配逗号。|  
|`(\d+)`|匹配一个或多个数字。 这是第四个捕获组。|  
|`;`|匹配分号。|  
|`((\w+(\s\w+)*),(\d+);)+`|匹配一个或多个以下模式：一个单词后跟任何其他单词，后跟一个逗号、一个或多个数字和一个分号。 这是第一个捕获组。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Text.RegularExpressions>
- [.NET 正则表达式](regular-expressions.md)
- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
