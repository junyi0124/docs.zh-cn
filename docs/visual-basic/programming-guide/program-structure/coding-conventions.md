---
title: 编码约定
ms.date: 07/20/2015
helpviewer_keywords:
- coding conventions [Visual Basic], Visual Basic
- examples [Visual Basic], coding conventions
- Visual Basic code, conventions
ms.assetid: c1df130b-fec6-49a5-becf-0a7e494a1d0f
ms.openlocfilehash: eae283c757ddeb1290c15d82a41c8028a8941e63
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91059153"
---
# <a name="visual-basic-coding-conventions"></a>Visual Basic 编码约定

Microsoft 将按照本主题中的准则开发示例和文档。 如果遵循相同的编码约定，可能会获得以下好处：  
  
- 你的代码将具有一致的外观，以便读者可以更好地专注于内容而非布局。  
  
- 读者可以更快地了解你的代码，因为它们可以根据以前的经验做出假设。  
  
- 您可以更轻松地复制、更改和维护代码。  
  
- 您可以帮助确保您的代码演示 Visual Basic 的 "最佳实践"。  
  
## <a name="naming-conventions"></a>命名约定  
  
- 有关命名准则的信息，请参阅 [命名准则](../../../standard/design-guidelines/naming-guidelines.md) 主题。  
  
- 不要使用 "我的" 或 "我的" 作为变量名称的一部分。 这种做法会与 `My` 对象混淆。  
  
- 不需要在自动生成的代码中更改对象的名称，使其符合指导原则。  
  
## <a name="layout-conventions"></a>布局约定  
  
- 将制表符插入为空格，并使用具有四个空格缩进的智能缩进。  
  
- 使用 **整齐的列表 (** 重新设置代码) 格式，以在代码编辑器中重新设置代码的格式。 有关详细信息，请参阅 [选项、文本编辑器、基本 (Visual Basic) ](/visualstudio/ide/reference/options-text-editor-basic-visual-basic)。  
  
- 每行仅使用一条语句。 不要使用 Visual Basic 行分隔符 (： ) 。  
  
- 在语言允许的任何位置，避免使用显式行继续符 "_" 来取代隐式行继续符。  
  
- 每行仅使用一个声明。  
  
- 如果在很多情况下 **(重新格式化) 的代码** 不会自动设置延续行的格式，则手动将连续行缩进一个制表位。 但是，始终左对齐列表中的项。  
  
    ```vb  
    a As Integer,  
    b As Integer  
    ```  
  
- 在方法和属性定义之间添加至少一个空白行。  
  
## <a name="commenting-conventions"></a>注释约定  
  
- 将注释放在单独的行上，而不是放在代码行的末尾。  
  
- 以大写字母开始注释文本，并以句点结束注释文本。  
  
- 在注释分隔符 ( ") 与注释文本之间插入一个空格。  
  
     [!code-vb[VbVbalrGuidelines#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#2)]  
  
- 不要在带格式的星号块中环绕注释。  
  
## <a name="program-structure"></a>程序结构  
  
- 使用 `Main` 方法时，请为新的控制台应用程序使用默认构造，并使用 `My` 作为命令行参数。  
  
     [!code-vb[VbVbalrGuidelines#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#3)]  
  
## <a name="language-guidelines"></a>语言准则  
  
### <a name="string-data-type"></a>String 数据类型  
  
- 使用[字符串内插](../language-features/strings/interpolated-strings.md)来连接短字符串，如下面的代码所示。
  
     ```vb
     MsgBox($"hello{vbCrLf}goodbye")
     ```
  
- 若要在循环中追加字符串，请使用 <xref:System.Text.StringBuilder> 对象。  
  
     [!code-vb[VbVbalrGuidelines#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#5)]  
  
### <a name="relaxed-delegates-in-event-handlers"></a>事件处理程序中的宽松委托  

 不要向事件处理程序 (对象和 EventArgs) 显式限定参数。 如果未使用传递给事件的事件参数 (例如，发送方为 Object，e 作为 EventArgs) ，请使用宽松委托，并在代码中留下事件参数：  
  
 [!code-vb[VbVbalrGuidelines#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#7)]  
  
### <a name="unsigned-data-type"></a>无符号数据类型  
  
- 使用 `Integer` 而不是无符号类型，但它们是必需的。  
  
### <a name="arrays"></a>数组  
  
- 在声明行上初始化数组时，请使用短语法。 例如，使用以下语法。  
  
     [!code-vb[VbVbalrGuidelines#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#8)]  
  
     不要使用以下语法。  
  
     [!code-vb[VbVbalrGuidelines#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#9)]  
  
- 将数组指示符置于类型上，而不是变量上。 例如，使用以下语法：  
  
     [!code-vb[VbVbalrGuidelines#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#11)]  
  
     不要使用以下语法：  
  
     [!code-vb[VbVbalrGuidelines#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#10)]  
  
- 声明和初始化基本数据类型的数组时，请使用 {} 语法。 例如，使用以下语法：  
  
     [!code-vb[VbVbalrGuidelines#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#12)]  
  
     不要使用以下语法：  
  
     [!code-vb[VbVbalrGuidelines#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#13)]  
  
### <a name="use-the-with-keyword"></a>使用 With 关键字  

 对一个对象进行一系列调用时，请考虑使用 `With` 关键字：  
  
 [!code-vb[VbVbalrGuidelines#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#15)]  
  
### <a name="use-the-trycatch-and-using-statements-when-you-use-exception-handling"></a>使用 Try .。。使用异常处理时捕获和使用语句  

 请勿使用 `On Error Goto`。  
  
### <a name="use-the-isnot-keyword"></a>使用 IsNot 关键字  

 使用 `IsNot` 关键字而不是 `Not...Is Nothing` 。  
  
### <a name="new-keyword"></a>New 关键字  
  
- 使用短实例化。 例如，使用以下语法：  
  
     [!code-vb[VbVbalrGuidelines#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#21)]  
  
     前面的行等效于：  
  
     [!code-vb[VbVbalrGuidelines#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#22)]  
  
- 为新对象使用对象初始值设定项，而不使用无参数构造函数：  
  
     [!code-vb[VbVbalrGuidelines#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#23)]  
  
### <a name="event-handling"></a>事件处理  
  
- 使用 `Handles` 而不是 `AddHandler` ：  
  
     [!code-vb[VbVbalrGuidelines#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#24)]  
  
- 使用 `AddressOf` ，并且不显式实例化委托：  
  
     [!code-vb[VbVbalrGuidelines#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#25)]  
  
- 定义事件时，请使用 short 语法，并让编译器定义委托：  
  
     [!code-vb[VbVbalrGuidelines#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#26)]  
  
- 在调用方法之前，不要验证事件是否为 `Nothing` null)  (`RaiseEvent` 。 `RaiseEvent``Nothing`在引发事件之前进行检查。  
  
### <a name="using-shared-members"></a>使用共享成员  

 `Shared`使用类名称（而不是从实例变量）调用成员。  
  
### <a name="use-xml-literals"></a>使用 XML 文本  

 XML 文本简化了在处理 XML (时遇到的最常见任务，例如，加载、查询和转换) 。 当你用 XML 开发时，请遵循以下准则：  
  
- 使用 XML 文本来创建 XML 文档和片段，而不是直接调用 XML Api。  
  
- 在文件或项目级别导入 XML 命名空间，以利用 XML 文本的性能优化。  
  
- 使用 XML 轴属性可以访问 XML 文档中的元素和属性。  
  
- 使用嵌入的表达式包含值和从现有值创建 XML，而不是使用 API 调用（如 `Add` 方法）：  
  
     [!code-vb[VbVbalrGuidelines#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#27)]  
  
### <a name="linq-queries"></a>LINQ 查询  
  
- 对查询变量使用有意义的名称：  
  
     [!code-vb[VbVbalrGuidelines#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#28)]  
  
- 为查询中的元素提供名称，以确保匿名类型的属性名称使用 Pascal 大小写正确地大写：  
  
     [!code-vb[VbVbalrGuidelines#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#29)]  
  
- 如果结果中的属性名称模棱两可，请对属性重命名。 例如，如果你的查询返回客户名称和订单 ID，请将它们重命名，而不是 `Name` `ID` 在结果中保留它们：  
  
     [!code-vb[VbVbalrGuidelines#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#30)]  
  
- 在查询变量和范围变量的声明中使用类型推理：  
  
     [!code-vb[VbVbalrGuidelines#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#31)]  
  
- 对齐语句下的查询子句 `From` ：  
  
     [!code-vb[VbVbalrGuidelines#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#32)]  
  
- 在 `Where` 其他查询子句之前使用子句，以便后面的查询子句对筛选的数据集执行操作：  
  
     [!code-vb[VbVbalrGuidelines#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#33)]  
  
- 使用 `Join` 子句显式定义联接运算，而不是使用 `Where` 子句隐式定义联接运算：  
  
     [!code-vb[VbVbalrGuidelines#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrGuidelines/VB/Class1.vb#34)]  
  
## <a name="see-also"></a>另请参阅

- [安全编码准则](../../../standard/security/secure-coding-guidelines.md)
