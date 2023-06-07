---
title: 如何：从 URL 中提取协议和端口号
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
ms.assetid: ab7f62b3-6d2c-4efb-8ac6-28600df5fd5c
ms.openlocfilehash: ba512fbe4ebc7ec35ca590541fe5bf94d07c465d
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734512"
---
# <a name="how-to-extract-a-protocol-and-port-number-from-a-url"></a>如何：从 URL 中提取协议和端口号

下面的示例从 URL 中提取协议和端口号。  

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="example"></a>示例  

 此示例使用 <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 方法返回协议，后面依次跟的是冒号和端口号。  
  
 [!code-csharp[RegularExpressions.Examples.Protocol#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/cs/Example.cs#1)]
 [!code-vb[RegularExpressions.Examples.Protocol#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/vb/Example.vb#1)]  
  
 正则表达式模式 `^(?<proto>\w+)://[^/]+?(?<port>:\d+)?/` 可按下表中的方式解释。  
  
|模式|说明|  
|-------------|-----------------|  
|`^`|从字符串的开头部分开始匹配。|  
|`(?<proto>\w+)`|匹配一个或多个单词字符。 将此组命名为 `proto`。|  
|`://`|匹配后跟两个正斜线的冒号。|  
|`[^/]+?`|匹配正斜线以外的任何字符的一次或多次出现（但尽可能少）。|  
|`(?<port>:\d+)?`|匹配后跟一个或多个数字字符的冒号的零次或一次出现。 将此组命名为 `port`。|  
|`/`|匹配正斜线。|  
  
 <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 方法扩展 `${proto}${port}` 替换序列，以连接在正则表达式模式中捕获的两个命名组的值。 便捷的替换方法是，显式连接从 <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> 属性返回的集合对象检索到的字符串。  
  
 此示例使用有两处替换（`${proto}` 和 `${port}`）的 <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> 方法，在输出字符串中添加捕获组。 可以改为从匹配的 <xref:System.Text.RegularExpressions.GroupCollection> 对象检索捕获组，如下面的代码所示。  
  
 [!code-csharp[RegularExpressions.Examples.Protocol#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/cs/example2.cs#2)]
 [!code-vb[RegularExpressions.Examples.Protocol#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/vb/example2.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [.NET 正则表达式](regular-expressions.md)
