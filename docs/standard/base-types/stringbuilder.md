---
title: 使用 .NET 中的 StringBuilder 类
description: 了解如何使用 .NET 中的 StringBuilder 类。 无需创建新对象即可使用此类来修改字符串。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Remove method
- strings [.NET], capacities
- StringBuilder object
- Replace method
- AppendFormat method
- Append method
- Insert method
- strings [.NET], StringBuilder object
ms.assetid: 5c14867c-9a99-45bc-ae7f-2686700d377a
ms.openlocfilehash: 54878f737faaedf4c9a176719f4ff83813a80201
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734213"
---
# <a name="using-the-stringbuilder-class-in-net"></a>使用 .NET 中的 StringBuilder 类

<xref:System.String> 对象不可变。 每次使用 <xref:System.String?displayProperty=nameWithType> 类中的方法之一，都要在内存中新建字符串对象，这就需要为新对象分配新空间。 在需要重复修改字符串的情况下，与新建 <xref:System.String> 对象关联的开销可能会非常大。 若要修改字符串（而不新建对象），可以使用 <xref:System.Text.StringBuilder?displayProperty=nameWithType> 类。 例如，如果在循环中将许多字符串连接在一起，使用 <xref:System.Text.StringBuilder> 类可以提升性能。  
  
## <a name="importing-the-systemtext-namespace"></a>导入 System.Text 命名空间  

 <xref:System.Text.StringBuilder> 类位于 <xref:System.Text> 命名空间中。  为了避免必须在代码中提供完全限定的类型名称，可以导入 <xref:System.Text> 命名空间：  
  
 [!code-cpp[Conceptual.StringBuilder#11](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#11)]
 [!code-csharp[Conceptual.StringBuilder#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#11)]
 [!code-vb[Conceptual.StringBuilder#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#11)]  
  
## <a name="instantiating-a-stringbuilder-object"></a>实例化 StringBuilder 对象  

 通过使用重载的构造函数方法之一初始化变量，可以新建 <xref:System.Text.StringBuilder> 类的实例，如下面的示例所示。  
  
 [!code-cpp[Conceptual.StringBuilder#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#1)]
 [!code-csharp[Conceptual.StringBuilder#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#1)]
 [!code-vb[Conceptual.StringBuilder#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#1)]  
  
## <a name="setting-the-capacity-and-length"></a>设置容量和长度  

 虽然 <xref:System.Text.StringBuilder> 是动态对象，支持扩展它封装的字符串中的字符数，但可以指定值，作为对象可保留的字符数上限。 此值称为“对象容量”，不得将它与当前 <xref:System.Text.StringBuilder> 保留的字符串长度相混淆。 例如，可以使用长度为 5 的字符串“Hello”新建 <xref:System.Text.StringBuilder> 类的实例，同时可以指定此对象的最大容量为 25。 修改 <xref:System.Text.StringBuilder> 时，除非达到容量，否则对象不会为自己重新分配空间。 当达到容量时，将自动分配新的空间且容量翻倍。 可以使用重载的构造函数之一，指定 <xref:System.Text.StringBuilder> 类的容量。 下面的示例指定可以将 `myStringBuilder` 对象增加到最多 25 个空间。  
  
 [!code-cpp[Conceptual.StringBuilder#2](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#2)]
 [!code-csharp[Conceptual.StringBuilder#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#2)]
 [!code-vb[Conceptual.StringBuilder#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#2)]  
  
 另外，还可以使用读/写 <xref:System.Text.StringBuilder.Capacity%2A> 属性，设置对象的长度上限。 下面的示例使用 **Capacity** 属性来定义对象的最大长度。  
  
 [!code-cpp[Conceptual.StringBuilder#3](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#3)]
 [!code-csharp[Conceptual.StringBuilder#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#3)]
 [!code-vb[Conceptual.StringBuilder#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#3)]  
  
 <xref:System.Text.StringBuilder.EnsureCapacity%2A> 方法可用于检查当前 StringBuilder 的容量。 如果容量大于传递的值，则不进行任何更改；但是，如果容量小于传递的值，则会更改当前的容量以使其与传递的值匹配。  
  
 也可以查看或设置 <xref:System.Text.StringBuilder.Length%2A> 属性。 如果将 **Length** 属性设置为大于 **Capacity** 属性的值，则自动将 **Capacity** 属性更改为与 **Length** 属性相同的值。 如果将 **Length** 属性设置为小于当前 **StringBuilder** 对象内的字符串长度的值，则会缩短该字符串。  
  
## <a name="modifying-the-stringbuilder-string"></a>修改 StringBuilder 字符串  

 下表列出了可用于修改 **StringBuilder** 内容的方法。  
  
|方法名称|使用|  
|-----------------|---------|  
|<xref:System.Text.StringBuilder.Append%2A?displayProperty=nameWithType>|将信息追加到当前 **StringBuilder** 的末尾。|  
|<xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType>|用带格式文本替换字符串中传递的格式说明符。|  
|<xref:System.Text.StringBuilder.Insert%2A?displayProperty=nameWithType>|将字符串或对象插入到当前 **StringBuilder** 的指定索引中。|  
|<xref:System.Text.StringBuilder.Remove%2A?displayProperty=nameWithType>|从当前 **StringBuilder** 中删除指定数量的字符。|  
|<xref:System.Text.StringBuilder.Replace%2A?displayProperty=nameWithType>|将当前 StringBuilder 中出现的所有指定字符或字符串替换为其他的指定字符或字符串。|  
  
### <a name="append"></a>追加  

 Append 方法可用于将对象的文本或字符串表示形式添加到当前 StringBuilder 表示的字符串末尾。 下面的示例将 **StringBuilder** 对象初始化为“Hello World”，然后将一些文本追加到该对象的末尾。 将根据需要自动分配空间。  
  
 [!code-cpp[Conceptual.StringBuilder#4](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#4)]
 [!code-csharp[Conceptual.StringBuilder#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#4)]
 [!code-vb[Conceptual.StringBuilder#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#4)]  
  
### <a name="appendformat"></a>AppendFormat  

 <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> 方法将文本添加到 <xref:System.Text.StringBuilder> 对象末尾。 它调用要设置格式的一个或多个对象的 <xref:System.IFormattable> 实现，支持复合格式功能（有关详细信息，请参阅[复合格式](composite-formatting.md)）。 因此，它接受数字、日期和时间以及枚举值的标准格式字符串、数字以及日期和时间值的自定义格式字符串，以及为自定义类型定义的格式字符串。 （有关格式化的信息，请参阅[格式设置类型](formatting-types.md)。）此方法可用于自定义变量格式，并将这些值追加到 <xref:System.Text.StringBuilder>。 下面的示例使用 <xref:System.Text.StringBuilder.AppendFormat%2A> 方法，将格式为货币值的整数值添加到 <xref:System.Text.StringBuilder> 对象末尾。  
  
 [!code-cpp[Conceptual.StringBuilder#5](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#5)]
 [!code-csharp[Conceptual.StringBuilder#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#5)]
 [!code-vb[Conceptual.StringBuilder#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#5)]  
  
### <a name="insert"></a>Insert  

 <xref:System.Text.StringBuilder.Insert%2A> 方法将字符串或对象添加到当前 <xref:System.Text.StringBuilder> 对象中的指定位置。 下面的示例使用此方法，将字词插入 <xref:System.Text.StringBuilder> 对象的第六个位置。  
  
 [!code-cpp[Conceptual.StringBuilder#6](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#6)]
 [!code-csharp[Conceptual.StringBuilder#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#6)]
 [!code-vb[Conceptual.StringBuilder#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#6)]  
  
### <a name="remove"></a>删除  

 可以使用 Remove方法，从当前 <xref:System.Text.StringBuilder> 对象中指定索引（从零开始编制）处开始删除指定数量的字符。 下面的示例使用 Remove 方法缩短 <xref:System.Text.StringBuilder> 对象。  
  
 [!code-cpp[Conceptual.StringBuilder#7](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#7)]
 [!code-csharp[Conceptual.StringBuilder#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#7)]
 [!code-vb[Conceptual.StringBuilder#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#7)]  
  
### <a name="replace"></a>替换  

 Replace 方法可用于将 <xref:System.Text.StringBuilder> 对象内的字符替换为另一个指定的字符。 下面的示例使用 Replace 方法，在 <xref:System.Text.StringBuilder> 对象中搜索感叹号字符 (!) 的所有实例，并将它们替换为问号字符 (?)。  
  
 [!code-cpp[Conceptual.StringBuilder#8](../../../samples/snippets/cpp/VS_Snippets_CLR/Conceptual.StringBuilder/cpp/example.cpp#8)]
 [!code-csharp[Conceptual.StringBuilder#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/Example.cs#8)]
 [!code-vb[Conceptual.StringBuilder#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/Example.vb#8)]  
  
## <a name="converting-a-stringbuilder-object-to-a-string"></a>将 StringBuilder 对象转换为字符串  

 必须先将 <xref:System.Text.StringBuilder> 对象转换为 <xref:System.String> 对象，然后才能将 <xref:System.Text.StringBuilder> 对象表示的字符串传递给包含 <xref:System.String> 参数的方法，或在用户界面中显示它。 可通过调用 <xref:System.Text.StringBuilder.ToString%2A?displayProperty=nameWithType> 方法来执行此转换。 下面的示例先调用许多 <xref:System.Text.StringBuilder> 方法，再调用 <xref:System.Text.StringBuilder.ToString?displayProperty=nameWithType> 方法来显示字符串。  
  
 [!code-csharp[Conceptual.StringBuilder#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.StringBuilder/cs/tostringexample1.cs#10)]
 [!code-vb[Conceptual.StringBuilder#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.StringBuilder/vb/tostringexample1.vb#10)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Text.StringBuilder?displayProperty=nameWithType>
- [基本字符串操作](basic-string-operations.md)
- [格式设置类型](formatting-types.md)
