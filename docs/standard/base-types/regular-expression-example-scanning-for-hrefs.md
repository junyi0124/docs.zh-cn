---
title: 正则表达式示例：扫描 HREF
description: 请参见 .NET 中的正则表达式示例。 此示例搜索输入字符串并显示所有 href 属性值和它们的位置。
ms.date: 06/30/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- searching with regular expressions, examples
- parsing text with regular expressions, examples
- regular expressions, examples
- .NET regular expressions, examples
- regular expressions [.NET], examples
- pattern-matching with regular expressions, examples
ms.assetid: fae2c15b-7adf-4b15-b118-58eb3906994f
ms.openlocfilehash: aceccc019542bb1afe3082881626cfc32740a338
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733641"
---
# <a name="regular-expression-example-scanning-for-hrefs"></a>正则表达式示例：扫描 HREF

下面的示例搜索输入字符串并显示所有 href="…" 的值和它们在字符串中的位置。  

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="the-regex-object"></a>Regex 对象

 因为可以通过用户代码多次调用 `DumpHRefs` 方法，所以它使用 `static`（Visual Basic 中的 `Shared`）<xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 方法。 这样一来，正则表达式引擎不仅可以缓存正则表达式，还杜绝了每次调用方法时实例化新 <xref:System.Text.RegularExpressions.Regex> 对象产生的开销。 随后使用 <xref:System.Text.RegularExpressions.Match> 对象循环访问字符串中的所有匹配。  
  
 [!code-csharp[RegularExpressions.Examples.HREF#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.HREF/cs/example.cs#1)]
 [!code-vb[RegularExpressions.Examples.HREF#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.HREF/vb/example.vb#1)]  
  
 下面的示例演示对 `DumpHRefs` 方法的调用。  
  
 [!code-csharp[RegularExpressions.Examples.HREF#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.HREF/cs/example.cs#2)]
 [!code-vb[RegularExpressions.Examples.HREF#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.HREF/vb/example.vb#2)]  
  
 正则表达式模式 `href\s*=\s*(?:["'](?<1>[^"']*)["']|(?<1>\S+))` 的含义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`href`|匹配文本字符串“href”。 匹配不区分大小写。|  
|`\s*`|匹配零个或多个空白字符。|  
|`=`|匹配等于号。|  
|`\s*`|匹配零个或多个空白字符。|  
|`(?:\["'\](?<1>\[^"'\]*)["']|(?<1>\S+))`|匹配以下项之一，而不将结果分配到捕获组：<br /> <ul><li><p>一个引号或单引号，后跟零个或多个引号或单引号以外的任意字符，然后再后跟一个引号或单引号。 名为 `1` 的组包含在此模式。</p></li><li><p>一个或多个非空格字符。 名为 `1` 的组包含在此模式。</p></li></ul>|  
|`(?<1>[^"']*)`|将零个或多个引号或单引号以外的任意字符分配给名为 `1` 的捕获组。|  
|`(?<1>\S+)`|将一个或多个非空白字符分配给名为 `1` 的捕获组。|  
  
## <a name="match-result-class"></a>匹配结果类  

 搜索结果存储在 <xref:System.Text.RegularExpressions.Match> 类中，此类可访问搜索提取的所有子字符串。 它还会记住搜索的字符串和使用的正则表达式，因此可以调用 <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> 方法，从上一次搜索结束的位置开始执行另一次搜索。  
  
## <a name="explicitly-named-captures"></a>显式命名的捕获  

 在传统正则表达式中，捕获圆括号会自动按顺序编号。 这会导致两个问题。 首先，如果通过插入或删除一组圆括号修改正则表达式，则必须重新编写引用带编号捕获的所有代码才能反映新编号。 其次，由于不同的圆括号组通常用于为可接受的匹配项提供两个替代表达式，则可能难以确定这两个表达式中的哪个表达式实际返回了结果。  
  
 为了解决这些问题，<xref:System.Text.RegularExpressions.Regex> 类支持语法 `(?<name>…)`，以便将匹配捕获到指定槽中（可以使用字符串或整数命名槽；如果使用整数命名，可以更快地召回）。 因此，相同字符串的替代匹配全都可以定向到相同位置。 发生冲突时，放入槽中的最后一个匹配项是成功的匹配项。 （但是，提供了适用于单个槽的多个匹配项的完整列表。 有关详细信息，请参阅 <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> 集合。）  
  
## <a name="see-also"></a>请参阅

- [.NET 正则表达式](regular-expressions.md)
