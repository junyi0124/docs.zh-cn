---
title: 分析 .NET 中的其他字符串
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Char data type, parsing strings
- enumerations [.NET], parsing strings
- base types, parsing strings
- parsing strings, other strings
- Boolean data type, parsing strings
ms.assetid: d139bc00-3c4e-4d78-ac9a-5c951b258d28
ms.openlocfilehash: 352bd426c889996daa5f420ccc384947896dc118
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683740"
---
# <a name="parsing-other-strings-in-net"></a>分析 .NET 中的其他字符串

除了数字和 <xref:System.DateTime> 字符串之外，还可以将表示类型 <xref:System.Char>、<xref:System.Boolean> 和 <xref:System.Enum> 的字符串分析为数据类型。  
  
## <a name="char"></a>Char  

 与 **Char** 数据类型关联的静态分析方法 可用于将包含单个字符的字符串转换为其 Unicode 值。 下面的代码示例将字符串分析为 Unicode 字符。  
  
 [!code-cpp[Conceptual.String.Parse#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.parse/cpp/parse.cpp#2)]
 [!code-csharp[Conceptual.String.Parse#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.parse/cs/parse.cs#2)]
 [!code-vb[Conceptual.String.Parse#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.parse/vb/parse.vb#2)]  
  
## <a name="boolean"></a>Boolean  

 Boolean  数据类型包含 Parse  方法，可用于将表示 Boolean 值的字符串转换为实际 Boolean  类型。 此方法不区分大小写，可以成功分析包含“True”或“False”的字符串。 与 Boolean 类型关联的 Parse 方法还可以分析两端是空格的字符串。 如果传递的是其他任何字符串，<xref:System.FormatException> 就会抛出。  
  
 下面的代码示例使用 Parse  方法，将字符串转换为 Boolean 值。  
  
 [!code-cpp[Conceptual.String.Parse#3](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.parse/cpp/parse.cpp#3)]
 [!code-csharp[Conceptual.String.Parse#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.parse/cs/parse.cs#3)]
 [!code-vb[Conceptual.String.Parse#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.parse/vb/parse.vb#3)]  
  
## <a name="enumeration"></a>枚举  

 可以使用静态 **Parse** 方法将枚举类型初始化为字符串的值。 此方法接受要分析的枚举类型、要分析的字符串和可选 Boolean 标志（指明分析是否区分大小写）。 所分析的字符串可以包含用逗号分隔的多个值，这些值前面或后面可以是一个或多个空白（也称为空格）。 当字符串包含多个值时，返回的对象的值是所有指定值通过按位 OR 运算组合的值。  
  
 下面的示例使用 Parse  方法，将字符串表示形式转换为枚举值。 <xref:System.DayOfWeek> 枚举从字符串初始化为 Thursday  。  
  
 [!code-cpp[Conceptual.String.Parse#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.parse/cpp/parse.cpp#4)]
 [!code-csharp[Conceptual.String.Parse#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.parse/cs/parse.cs#4)]
 [!code-vb[Conceptual.String.Parse#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.parse/vb/parse.vb#4)]  
  
## <a name="see-also"></a>另请参阅

- [Parsing Strings](parsing-strings.md)
- [格式设置类型](formatting-types.md)
- [.NET 中的类型转换](type-conversion.md)
