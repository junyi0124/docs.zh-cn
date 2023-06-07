---
title: 字符串 - C# 编程指南
description: 了解 C# 编程中的字符串。 请参阅有关声明和初始化字符串、字符串对象不可变性和字符串转义序列的信息。
ms.date: 06/27/2019
helpviewer_keywords:
- C# language, strings
- strings [C#]
ms.assetid: 21580405-cb25-4541-89d5-037846a38b07
ms.openlocfilehash: ba2bd6c90fa28624d52e7ef2e341b43da7ea19a2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176209"
---
# <a name="strings-c-programming-guide"></a>字符串（C# 编程指南）

字符串是值为文本的 <xref:System.String> 类型对象。 文本在内部存储为 <xref:System.Char> 对象的依序只读集合。 在 C# 字符串末尾没有 null 终止字符；因此，一个 C# 字符串可以包含任何数量的嵌入的 null 字符 ('\0')。 字符串的 <xref:System.String.Length%2A> 属性表示其包含的 `Char` 对象数量，而非 Unicode 字符数。 若要访问字符串中的各个 Unicode 码位，请使用 <xref:System.Globalization.StringInfo> 对象。  
  
## <a name="string-vs-systemstring"></a>string 与System.String  

 在 C# 中，`string` 关键字是 <xref:System.String> 的别名。 因此，`String` 和 `string` 是等效的，你可以使用你所喜欢的任何一种命名约定。 `String` 类提供了安全创建、操作和比较字符串的多种方法。 此外，C# 语言重载了部分运算符，以简化常见字符串操作。 有关关键字的详细信息，请参阅 [string](../../language-reference/builtin-types/reference-types.md)。 有关类型及其方法的详细信息，请参阅 <xref:System.String>。  
  
## <a name="declaring-and-initializing-strings"></a>声明和初始化字符串  

 可以使用各种方法声明和初始化字符串，如以下示例中所示：  
  
 [!code-csharp[csProgGuideStrings#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#1)]  
  
 请注意，不要使用 [new](../../language-reference/operators/new-operator.md) 运算符创建字符串对象，除非使用字符数组初始化字符串。  
  
 使用 <xref:System.String.Empty> 常量值初始化字符串，以新建字符串长度为零的 <xref:System.String> 对象。 长度为零的字符串文本表示法是“”。 通过使用 <xref:System.String.Empty> 值（而不是 [null](../../language-reference/keywords/null.md)）初始化字符串，可以减少 <xref:System.NullReferenceException> 发生的可能性。 尝试访问字符串前，先使用静态 <xref:System.String.IsNullOrEmpty%28System.String%29> 方法验证字符串的值。  
  
## <a name="immutability-of-string-objects"></a>字符串对象的不可变性  

 字符串对象是“不可变的”：它们在创建后无法更改。 看起来是在修改字符串的所有 <xref:System.String> 方法和 C# 运算符实际上都是在新的字符串对象中返回结果。 在下面的示例中，当 `s1` 和 `s2` 的内容被串联在一起以形成单个字符串时，两个原始字符串没有被修改。 `+=` 运算符创建一个新的字符串，其中包含组合的内容。 这个新对象被分配给变量 `s1`，而分配给 `s1` 的原始对象被释放，以供垃圾回收，因为没有任何其他变量包含对它的引用。  
  
 [!code-csharp[csProgGuideStrings#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#2)]  
  
 由于字符串“modification”实际上是一个新创建的字符串，因此，必须在创建对字符串的引用时使用警告。 如果创建了字符串的引用，然后“修改”了原始字符串，则该引用将继续指向原始对象，而非指向修改字符串时所创建的新对象。 以下代码阐释了此行为：  
  
 [!code-csharp[csProgGuideStrings#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#25)]  
  
 有关如何创建基于修改的新字符串的详细信息，例如原始字符串上的搜索和替换操作，请参阅[如何修改字符串内容](../../how-to/modify-string-contents.md)。  
  
## <a name="regular-and-verbatim-string-literals"></a>常规和逐字字符串文本  

 在必须嵌入 C# 提供的转义字符时，使用常规字符串文本，如以下示例所示：  
  
 [!code-csharp[csProgGuideStrings#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#3)]  
  
 当字符串文本包含反斜杠字符（例如在文件路径中）时，出于便捷性和更强的可读性的考虑，使用逐字字符串。 由于逐字字符串将新的行字符作为字符串文本的一部分保留，因此可将其用于初始化多行字符串。 使用双引号在逐字字符串内部嵌入引号。 下面的示例演示逐字字符串的一些常见用法：  
  
 [!code-csharp[csProgGuideStrings#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#4)]  
  
## <a name="string-escape-sequences"></a>字符串转义序列  
  
|转义序列|字符名称|Unicode 编码|  
|---------------------|--------------------|----------------------|  
|\\'|单引号|0x0027|  
|\\"|双引号|0x0022|  
|\\\\ |反斜杠|0x005C|  
|\0|null|0x0000|  
|\a|警报|0x0007|  
|\b|Backspace|0x0008|  
|\f|换页|0x000C|  
|\n|换行|0x000A|  
|\r|回车|0x000D|  
|\t|水平制表符|0x0009|  
|\v|垂直制表符|0x000B|  
|\u|Unicode 转义序列 (UTF-16)|`\uHHHH`（范围：0000 - FFFF；示例：`\u00E7` =“ç”）|  
|\U|Unicode 转义序列 (UTF-32)|`\U00HHHHHH`（范围：000000 - 10FFFF；示例：`\U0001F47D` =“&#x1F47D;”)|  
|\x|除长度可变外，Unicode 转义序列与“\u”类似|`\xH[H][H][H]`（范围：0 - FFFF；示例：`\x00E7`、`\x0E7` 或 `\xE7` =“ç”）|  
  
> [!WARNING]
> 使用 `\x` 转义序列且指定的位数小于 4 个十六进制数字时，如果紧跟在转义序列后面的字符是有效的十六进制数字（即 0-9、A-F 和 a-f），则这些字符将被解释为转义序列的一部分。 例如，`\xA1` 会生成“&#161;”，即码位 U+00A1。 但是，如果下一个字符是“A”或“a”，则转义序列将转而被解释为 `\xA1A` 并生成“&#x0A1A;”（即码位 U+0A1A）。 在此类情况下，如果指定全部 4 个十六进制数字（例如 `\x00A1`），则可能导致解释出错。  
  
> [!NOTE]
> 在编译时，逐字字符串被转换为普通字符串，并具有所有相同的转义序列。 因此，如果在调试器监视窗口中查看逐字字符串，将看到由编译器添加的转义字符，而不是来自你的源代码的逐字字符串版本。 例如，原义字符串 `@"C:\files.txt"` 在监视窗口中显示为“C:\\\files.txt”。  
  
## <a name="format-strings"></a>格式字符串  

 格式字符串是在运行时以动态方式确定其内容的字符串。 格式字符串是通过将内插表达式或占位符嵌入字符串大括号内创建的。 大括号 (`{...}`) 中的所有内容都将解析为一个值，并在运行时以格式化字符串的形式输出。 有两种方法创建格式字符串：字符串内插和复合格式。

### <a name="string-interpolation"></a>字符串内插

在 C# 6.0 及更高版本中提供，[内插字符串](../../language-reference/tokens/interpolated.md)由 `$` 特殊字符标识，并在大括号中包含内插表达式。 如果不熟悉字符串内插，请参阅[字符串内插 - C# 交互式教程](../../tutorials/exploration/interpolated-strings.yml)快速概览。

使用字符串内插来改善代码的可读性和可维护性。 字符串内插可实现与 `String.Format` 方法相同的结果，但提高了易用性和内联清晰度。

[!code-csharp[csProgGuideFormatStrings](~/samples/snippets/csharp/programming-guide/strings/Strings_1.cs#StringInterpolation)]

### <a name="composite-formatting"></a>复合格式设置

<xref:System.String.Format%2A?displayProperty=nameWithType> 利用大括号中的占位符创建格式字符串。 此示例生成与上面使用的字符串内插方法类似的输出。
  
[!code-csharp[csProgGuideFormatStrings](~/samples/snippets/csharp/programming-guide/strings/Strings_1.cs#StringFormat)]

有关设置 .NET 类型格式的详细信息，请参阅 [.NET 中的格式设置类型](../../../standard/base-types/formatting-types.md)。
  
## <a name="substrings"></a>子字符串  

 子字符串是包含在字符串中的任何字符序列。 使用 <xref:System.String.Substring%2A> 方法可以通过原始字符串的一部分新建字符串。 可以使用 <xref:System.String.IndexOf%2A> 方法搜索一次或多次出现的子字符串。 使用 <xref:System.String.Replace%2A> 方法可以将出现的所有指定子字符串替换为新字符串。 与 <xref:System.String.Substring%2A> 方法一样，<xref:System.String.Replace%2A> 实际返回的是新字符串，且不修改原始字符串。 有关详细信息，请参阅[如何搜索字符串](../../how-to/search-strings.md)和[如何修改字符串内容](../../how-to/modify-string-contents.md)。
  
 [!code-csharp[csProgGuideStrings#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#7)]  
  
## <a name="accessing-individual-characters"></a>访问单个字符  

 可以使用包含索引值的数组表示法来获取对单个字符的只读访问权限，如下面的示例中所示：  
  
 [!code-csharp[csProgGuideStrings#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#8)]  
  
 如果 <xref:System.String> 方法不提供修改字符串中的各个字符所需的功能，可以使用 <xref:System.Text.StringBuilder> 对象“就地”修改各个字符，再新建字符串来使用 <xref:System.Text.StringBuilder> 方法存储结果。 在下面的示例中，假定必须以特定方式修改原始字符串，然后存储结果以供未来使用：  
  
 [!code-csharp[csProgGuideStrings#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#27)]  
  
## <a name="null-strings-and-empty-strings"></a>Null 字符串和空字符串  

 空字符串是包含零个字符的 <xref:System.String?displayProperty=nameWithType> 对象实例。 空字符串常用在各种编程方案中，表示空文本字段。 可以对空字符串调用方法，因为它们是有效的 <xref:System.String?displayProperty=nameWithType> 对象。 对空字符串进行了初始化，如下所示：  
  
```csharp  
string s = String.Empty;  
```  
  
 相比较而言，null 字符串并不指 <xref:System.String?displayProperty=nameWithType> 对象实例，只要尝试对 null 字符串调用方法，都会引发 <xref:System.NullReferenceException>。 但是，可以在串联和与其他字符串的比较操作中使用 null 字符串。 以下示例说明了对 null 字符串的引用会引发和不会引发意外的某些情况：  
  
 [!code-csharp[csProgGuideStrings#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#20)]  
  
## <a name="using-stringbuilder-for-fast-string-creation"></a>使用 StringBuilder 快速创建字符串  

 .NET 中的字符串操作进行了高度的优化，在大多数情况下不会显著影响性能。 但是，在某些情况下（例如，执行数百次或数千次的紧密循环），字符串操作可能影响性能。 <xref:System.Text.StringBuilder> 类创建字符串缓冲区，用于在程序执行多个字符串操控时提升性能。 使用 <xref:System.Text.StringBuilder> 字符串，还可以重新分配各个字符，而内置字符串数据类型则不支持这样做。 例如，此代码更改字符串的内容，而无需创建新的字符串：  
  
 [!code-csharp[csProgGuideStrings#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#15)]  
  
 在以下示例中，<xref:System.Text.StringBuilder> 对象用于通过一组数字类型创建字符串：  
  
 [!code-csharp[TestStringBuilder#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/TestStringBuilder.cs)]
  
## <a name="strings-extension-methods-and-linq"></a>字符串、扩展方法和 LINQ  

 由于 <xref:System.String> 类型实现 <xref:System.Collections.Generic.IEnumerable%601>，因此可以对字符串使用 <xref:System.Linq.Enumerable> 类中定义的扩展方法。 为了避免视觉干扰，这些方法已从 <xref:System.String> 类型的 IntelliSense 中排除，但它们仍然可用。 还可以使用字符串上的 LINQ 查询表达式。 有关详细信息，请参阅 [LINQ 和字符串](../concepts/linq/linq-and-strings.md)。  
  
## <a name="related-topics"></a>相关主题  
  
|主题|描述|  
|-----------|-----------------|  
|[如何修改字符串内容](../../how-to/modify-string-contents.md)|阐明转换字符串并修改字符串内容的方法。|  
|[如何比较字符串](../../how-to/compare-strings.md)|演示如何对字符串执行序号和特定于区域性的比较。|  
|[如何连接多个字符串](../../how-to/concatenate-multiple-strings.md)|演示将多个字符串联接成一个字符串的多种方式。|
|[如何使用 String.Split 分析字符串](../../how-to/parse-strings-using-split.md)|包含代码示例，演示了如何使用 `String.Split` 方法来分析字符串。|  
|[如何搜索字符串](../../how-to/search-strings.md)|说明如何在字符串中使用搜索来搜索特定的文本或模式。|  
|[如何确定字符串是否表示数值](./how-to-determine-whether-a-string-represents-a-numeric-value.md)|演示如何安全地分析一个字符串，以查看其是否具有有效的数值。|  
|[字符串内插](../../language-reference/tokens/interpolated.md)|介绍字符串内插功能，它提供了一种方便的语法来格式化字符串。|
|[基本字符串操作](../../../standard/base-types/basic-string-operations.md)|收录了介绍如何使用 <xref:System.String?displayProperty=nameWithType> 和 <xref:System.Text.StringBuilder?displayProperty=nameWithType> 方法执行基本字符串操作的主题链接。|  
|[分析字符串](../../../standard/base-types/parsing-strings.md)|介绍如何将 .NET 基类型的字符串表示形式转换为相应类型的实例。|  
|[分析 .NET 中的日期和时间字符串](../../../standard/base-types/parsing-datetime.md)|展示了如何将字符串（如“01/24/2008”）转换为 <xref:System.DateTime?displayProperty=nameWithType> 对象。|  
|[比较字符串](../../../standard/base-types/comparing.md)|包括有关如何比较字符串的信息，并提供 C# 和 Visual Basic 中的示例。|  
|[使用 StringBuilder 类](../../../standard/base-types/stringbuilder.md)|介绍了如何使用 <xref:System.Text.StringBuilder> 类创建和修改动态字符串对象。|  
|[LINQ 和字符串](../concepts/linq/linq-and-strings.md)|提供有关如何使用 LINQ 查询来执行各种字符串操作的信息。|  
|[C# 编程指南](../index.md)|提供介绍在 C# 中编程构造的主题的链接。|  
