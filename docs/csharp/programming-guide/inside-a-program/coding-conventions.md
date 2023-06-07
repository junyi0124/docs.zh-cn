---
title: C# 编码约定 - C# 编程指南
description: 了解 C# 编码约定。 编码约定为代码创建一致的外观，并简化代码的复制、更改和维护过程。
ms.date: 07/20/2015
helpviewer_keywords:
- coding conventions, C#
- Visual C#, coding conventions
- C# language, coding conventions
ms.assetid: f4f60de9-d49b-4fb6-bab1-20e19ea24710
ms.openlocfilehash: 772aebff0b8c7aebe7c7d5c7634cd2931f4570b1
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301848"
---
# <a name="c-coding-conventions-c-programming-guide"></a>C# 编码约定（C# 编程指南）

编码约定可实现以下目的：  
  
- 它们为代码创建一致的外观，以确保读取器专注于内容而非布局。  
  
- 它们使得读取器可以通过基于之前的经验进行的假设更快地理解代码。  
  
- 它们便于复制、更改和维护代码。  
  
- 它们展示 C# 最佳做法。  

Microsoft 根据本文中的准则来开发样本和文档。  
  
## <a name="naming-conventions"></a>命名约定  
  
- 在不包括 [using 指令](../../language-reference/keywords/using-directive.md)的短示例中，使用命名空间限定。 如果你知道命名空间默认导入项目中，则不必完全限定来自该命名空间的名称。 如果对于单行来说过长，则可以在点 (.) 后中断限定名称，如下面的示例所示。  
  
     [!code-csharp[csProgGuideCodingConventions#1](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#1)]  
  
- 你不必更改通过使用 Visual Studio 设计器工具创建的对象的名称以使它们适合其他准则。  
  
## <a name="layout-conventions"></a>布局约定  

好的布局利用格式设置来强调代码的结构并使代码更便于阅读。 Microsoft 示例和样本符合以下约定：  
  
- 使用默认的代码编辑器设置（智能缩进、4 字符缩进、制表符保存为空格）。 有关详细信息，请参阅[选项、文本编辑器、C#、格式设置](/visualstudio/ide/reference/options-text-editor-csharp-formatting)。  
  
- 每行只写一条语句。  
  
- 每行只写一个声明。  
  
- 如果连续行未自动缩进，请将它们缩进一个制表符位（四个空格）。  
  
- 在方法定义与属性定义之间添加至少一个空白行。  
  
- 使用括号突出表达式中的子句，如下面的代码所示。  
  
     [!code-csharp[csProgGuideCodingConventions#2](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#2)]  
  
## <a name="commenting-conventions"></a>注释约定  
  
- 将注释放在单独的行上，而非代码行的末尾。  
  
- 以大写字母开始注释文本。  
  
- 以句点结束注释文本。  
  
- 在注释分隔符 (//) 与注释文本之间插入一个空格，如下面的示例所示。  
  
     [!code-csharp[csProgGuideCodingConventions#3](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#3)]  
  
- 不要在注释周围创建格式化的星号块。  
  
## <a name="language-guidelines"></a>语言准则  

以下各节介绍 C# 遵循以准备代码示例和样本的做法。  
  
### <a name="string-data-type"></a>String 数据类型  
  
- 使用[字符串内插](../../language-reference/tokens/interpolated.md)来连接短字符串，如下面的代码所示。  
  
     [!code-csharp[csProgGuideCodingConventions#6](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#6)]  
  
- 若要在循环中追加字符串，尤其是在使用大量文本时，请使用 <xref:System.Text.StringBuilder> 对象。  
  
     [!code-csharp[csProgGuideCodingConventions#7](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#7)]  
  
### <a name="implicitly-typed-local-variables"></a>隐式类型的局部变量  
  
- 当变量类型明显来自赋值的右侧时，或者当精度类型不重要时，请对本地变量进行[隐式类型化](../classes-and-structs/implicitly-typed-local-variables.md)。  
  
     [!code-csharp[csProgGuideCodingConventions#8](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#8)]  
  
- 当类型并非明显来自赋值的右侧时，请勿使用 [var](../../language-reference/keywords/var.md)。  
  
     [!code-csharp[csProgGuideCodingConventions#9](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#9)]  
  
- 请勿依靠变量名称来指定变量的类型。 它可能不正确。  
  
     [!code-csharp[csProgGuideCodingConventions#10](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#10)]  
  
- 避免使用 `var` 来代替 [dynamic](../../language-reference/builtin-types/reference-types.md)。  
  
- 使用隐式类型化来确定 [for](../../language-reference/keywords/for.md) 循环中循环变量的类型。  
  
     下面的示例在 `for` 语句中使用隐式类型化。  
  
     [!code-csharp[csProgGuideCodingConventions#7](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#7)]  

- 不要使用隐式类型化来确定 [foreach](../../language-reference/keywords/foreach-in.md) 循环中循环变量的类型。

     下面的示例在 `foreach` 语句中使用显式类型化。

     [!code-csharp[csProgGuideCodingConventions#12](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#12)]

     > [!NOTE]
     > 注意不要意外更改可迭代集合的元素类型。 例如，在 `foreach` 语句中从 <xref:System.Linq.IQueryable?displayProperty=nameWithType> 切换到 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> 很容易，这会更改查询的执行。

### <a name="unsigned-data-type"></a>无符号数据类型  
  
通常，使用 `int` 而非无符号类型。 `int` 的使用在整个 C# 中都很常见，并且当你使用 `int` 时，更易于与其他库交互。  
  
### <a name="arrays"></a>数组  
  
当在声明行上初始化数组时，请使用简洁的语法。  
  
[!code-csharp[csProgGuideCodingConventions#13](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#13)]  
  
### <a name="delegates"></a>委托  
  
使用简洁的语法来创建委托类型的实例。  
  
[!code-csharp[csProgGuideCodingConventions#14](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#14)]  
  
[!code-csharp[csProgGuideCodingConventions#15](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#15)]  
  
### <a name="try-catch-and-using-statements-in-exception-handling"></a>异常处理中的 try-catch 和 using 语句  
  
- 对大多数异常处理使用 [try-catch](../../language-reference/keywords/try-catch.md) 语句。  
  
     [!code-csharp[csProgGuideCodingConventions#16](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#16)]  
  
- 通过使用 C# [using 语句](../../language-reference/keywords/using-statement.md)简化你的代码。 如果具有 [try-finally](../../language-reference/keywords/try-finally.md) 语句（该语句中 `finally` 块的唯一代码是对 <xref:System.IDisposable.Dispose%2A> 方法的调用），请使用 `using` 语句代替。  
  
     [!code-csharp[csProgGuideCodingConventions#17](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#17)]  
  
### <a name="-and-124124-operators"></a>&& 和 || 运算符  
  
若要通过跳过不必要的比较来避免异常并提高性能，请在执行比较时使用 [&&](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-)（而不是 [&](../../language-reference/operators/boolean-logical-operators.md#logical-and-operator-)），使用 [||](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-) （而不是 [|](../../language-reference/operators/boolean-logical-operators.md#logical-or-operator-)），如下面的示例所示。  
  
[!code-csharp[csProgGuideCodingConventions#18](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#18)]  
  
### <a name="new-operator"></a>New 运算符  
  
- 隐式类型化时，请使用对象实例化的简洁形式，如下面的声明所示。  
  
     [!code-csharp[csProgGuideCodingConventions#19](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#19)]  
  
     上一行等同于下面的声明。  
  
     [!code-csharp[csProgGuideCodingConventions#20](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#20)]  
  
- 使用对象初始值设定项来简化对象创建。  
  
     [!code-csharp[csProgGuideCodingConventions#21](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#21)]  
  
### <a name="event-handling"></a>事件处理  
  
如果你正定义一个稍后不需要删除的事件处理程序，请使用 lambda 表达式。  
  
[!code-csharp[csProgGuideCodingConventions#22](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#22)]  
  
[!code-csharp[csProgGuideCodingConventions#23](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#23)]  
  
### <a name="static-members"></a>静态成员  
  
使用类名调用 [static](../../language-reference/keywords/static.md) 成员：ClassName.StaticMember。 这种做法通过明确静态访问使代码更易于阅读。  请勿使用派生类的名称限定基类中定义的静态成员。  编译该代码时，代码可读性具有误导性，如果向派生类添加具有相同名称的静态成员，代码可能会被破坏。  
  
### <a name="linq-queries"></a>LINQ 查询  
  
- 对查询变量使用有意义的名称。 下面的示例为位于西雅图的客户使用 `seattleCustomers`。  
  
     [!code-csharp[csProgGuideCodingConventions#25](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#25)]  
  
- 使用别名确保匿名类型的属性名称都使用 Pascal 大小写格式正确大写。  
  
     [!code-csharp[csProgGuideCodingConventions#26](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#26)]  
  
- 如果结果中的属性名称模棱两可，请对属性重命名。 例如，如果你的查询返回客户名称和分销商 ID，而不是在结果中将它们保留为 `Name` 和 `ID`，请对它们进行重命名以明确 `Name` 是客户的名称，`ID` 是分销商的 ID。  
  
     [!code-csharp[csProgGuideCodingConventions#27](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#27)]  
  
- 在查询变量和范围变量的声明中使用隐式类型化。  
  
     [!code-csharp[csProgGuideCodingConventions#25](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#25)]  
  
- 对齐 [from](../../language-reference/keywords/from-clause.md) 子句下的查询子句，如上面的示例所示。  
  
- 在其他查询子句之前使用 [where](../../language-reference/keywords/where-clause.md) 子句，以确保后面的查询子句作用于经过减少和筛选的数据集。  
  
     [!code-csharp[csProgGuideCodingConventions#29](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#29)]  
  
- 使用多行 `from` 子句代替 [join](../../language-reference/keywords/join-clause.md) 子句以访问内部集合。 例如，`Student` 对象的集合可能包含测验分数的集合。 当执行以下查询时，它返回高于 90 的分数，并返回得到该分数的学生的姓氏。  
  
     [!code-csharp[csProgGuideCodingConventions#30](../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs#30)]  
  
## <a name="security"></a>安全性  

请遵循[安全编码准则](../../../standard/security/secure-coding-guidelines.md)中的准则。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 编码约定](../../../visual-basic/programming-guide/program-structure/coding-conventions.md)
- [安全编码准则](../../../standard/security/secure-coding-guidelines.md)
