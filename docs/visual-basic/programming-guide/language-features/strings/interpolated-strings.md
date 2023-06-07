---
title: 内插字符串
ms.date: 10/31/2017
ms.openlocfilehash: c427b48ce58a59ff3878f24f1989db6ac8c8239a
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2020
ms.locfileid: "91805273"
---
# <a name="interpolated-strings-visual-basic-reference"></a>Visual Basic 引用 (的内插字符串) 

用于构造字符串。  内插字符串类似于包含内插表达式** 的模板字符串。  内插字符串返回的字符串可替换内插表达式并且包含其字符串表示形式。 此功能在 Visual Basic 14 及更高版本中可用。

与[复合格式字符串](../../../../standard/base-types/composite-formatting.md#composite-format-string)相比，内插字符串的参数更易于理解。  例如，内插字符串

```vb
Console.WriteLine($"Name = {name}, hours = {hours:hh}")
```

包含 2 个内插表达式：“{name}”和“{hours:hh}”。 等效复合格式字符串为：

```vb
Console.WriteLine("Name = {0}, hours = {1:hh}", name, hours)
```

内插字符串的结构为：

```vb
$"<text> {<interpolated-expression> [,<field-width>] [:<format-string>] } <text> ..."
```

其中：

- *field-width* 是一个带符号整数，表示字段中的字符数。 如果为正数，则字段右对齐；如果为负数，则左对齐。

- “格式字符串”** 是适合正在设置格式的对象类型的格式字符串。 例如，对于 <xref:System.DateTime> 值，它可以是 [标准日期和时间格式字符串](../../../../standard/base-types/standard-date-and-time-format-strings.md) ，例如 "d" 或 "d"。

> [!IMPORTANT]
> 字符串开头的 `$` 和 `"` 之间不能有任何空格。 这样做会导致编译器错误。

可以在可使用字符串的任何位置使用内插字符串。  每次执行带内插字符串的代码时，都会计算内插字符串。 这有助于分隔内插字符串的定义和计算结果。

若要在内插字符串中包含大括号（“{”或“}”），请使用两个大括号，即“{{”或“}}”。  请参阅“隐式转换”部分以获取详细信息。

如果内插字符串包含在内插字符串中具有特殊意义的其他字符，例如引号 (")、冒号 (:) 或逗号 (,)，则出现在文本中时应转义这些字符；如果它们是内插表达式中随附的语言元素，则应将其包括在由括号分隔的表达式内。 下面的示例对引号进行转义，以将它们包含在结果字符串中：

[!code-vb[interpolated-strings](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings4.vb)]

## <a name="implicit-conversions"></a>隐式转换

内插字符串有 3 种隐式类型转换：

1. 将内插字符串转换为 <xref:System.String>。 下例返回一个字符串，其内插字符串表达式已替换为字符串表示形式。 例如： 。

   [!code-vb[interpolated-strings1](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings1.vb)]

   这是字符串解释的最终结果。 出现的所有双大括号（“{{”和“}}”）都转换为单大括号。

2. 将内插字符串转换为 <xref:System.IFormattable> 变量，使用此变量可通过单个 <xref:System.IFormattable> 实例创建多个包含区域性特定内容的结果字符串。 对于包括诸如各区域性的正确数字和日期格式之类的内容，这种转换很有用。  出现的所有双大括号（“{{”和“}}”）仍保留为双大括号，直至通过显式或隐式调用 <xref:System.Object.ToString> 方法格式化字符串。  所有包含的内插表达式都转换为 {0} 、 {1} 等。

   以下示例使用反射来显示内插字符串中所创建的 <xref:System.IFormattable> 变量的成员以及字段和属性值。 并将 <xref:System.IFormattable> 变量传递给 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法。

   [!code-vb[interpolated-strings2](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings2.vb)]

   请注意，只能通过使用反射来检查内插字符串。 如果将其传递给字符串格式设置方法，例如 <xref:System.Console.WriteLine(System.String)>，则会解析其格式项并返回结果字符串。

3. 将内插字符串转换为 <xref:System.FormattableString> 表示复合格式字符串的变量。 例如，检查复合格式字符串及其呈现为结果字符串的方式可能有助于在生成查询时防止注入攻击。 <xref:System.FormattableString>还包括：

      - <xref:System.FormattableString.ToString> 重载，生成 <xref:System.Globalization.CultureInfo.CurrentCulture> 的结果字符串。

      - 为 <xref:System.FormattableString.Invariant%2A> 生成字符串的方法 <xref:System.Globalization.CultureInfo.InvariantCulture> 。

      - <xref:System.FormattableString.ToString(System.IFormatProvider)> 方法，生成特定区域性的结果字符串。

    出现的所有双大括号 ( "{{" 和 "}}" ) 将保留为双大括号，直至进行格式化。  所有包含的内插表达式都转换为 {0} 、 {1} 等。

   [!code-vb[interpolated-strings3](../../../../../samples/snippets/visualbasic/programming-guide/language-features/strings/interpolated-strings3.vb)]

## <a name="see-also"></a>请参阅

- <xref:System.IFormattable?displayProperty=nameWithType>
- <xref:System.FormattableString?displayProperty=nameWithType>
- [Visual Basic 语言参考](index.md)
