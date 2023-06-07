---
title: 名称“<name>”未声明
ms.date: 10/10/2018
f1_keywords:
- bc30451
- vbc30451
helpviewer_keywords:
- BC30451
ms.assetid: 765f099b-e21e-47c6-a906-a065444e56b3
ms.openlocfilehash: 0a2c2a90b99017fcd9bb594acc6f2dbcf29d9536
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160192"
---
# <a name="bc30451-name-name-is-not-declared"></a>BC30451：名称 " \<name> " 未声明

语句引用编程元素，但编译器找不到具有该确切名称的元素。

 **错误 ID：** BC30451

## <a name="to-correct-this-error"></a>更正此错误

1. 检查引用语句中名称的拼写。 Visual Basic 不区分大小写，但拼写中的任何其他变体被视为完全不同的名称。 请注意，下划线 (`_`) 是名称的一部分，因此也是拼写的一部分。

2. 检查是否有成员访问运算符在 `.` 对象及其成员之间 () 。 例如，如果你拥有名为 <xref:System.Windows.Forms.TextBox> 的 `TextBox1`控件，则要键入 <xref:System.Windows.Forms.TextBoxBase.Text%2A> 才可访问其 `TextBox1.Text`。 如果你转而键入 `TextBox1Text`，则会创建不同的名称。

3. 如果拼写正确并且任何对象成员访问的语法正确，请验证元素是否已声明。 有关详细信息，请参阅已 [声明元素](../../programming-guide/language-features/declared-elements/index.md)。

4. 如果已声明编程元素，请检查它是否在范围内。 如果引用语句位于声明编程元素的区域外，则可能需要限定元素名称。 有关详细信息，请参阅 [Scope in Visual Basic](../../programming-guide/language-features/declared-elements/scope.md)。

5. 如果未使用完全限定的类型或类型和成员名称 (例如，你的代码将属性视为 `MethodInfo.Name` 而不是 `System.Reflection.MethodInfo.Name`) ，请添加 [Imports 语句](../statements/imports-statement-net-namespace-and-type.md)。

6. 如果尝试使用 .vbproj 文件（以行) 开头的文件）编译 SDK 样式 (项目， \* `<Project Sdk="Microsoft.NET.Sdk">` 并且错误消息引用 Microsoft.VisualBasic.dll 程序集中的类型或成员，则将应用程序配置为使用对 Visual Basic 运行库的引用进行编译。 默认情况下，库的一个子集嵌入 SDK 样式项目中的程序集。

   例如，下面的示例无法编译，因为 <xref:Microsoft.VisualBasic.CompilerServices.Conversions.ChangeType%2A?displayProperty=fullName> 找不到该方法。 它不会嵌入到应用程序附带的 Visual Basic 运行时的子集中。

   [!code-vb[BC30451](~/samples/snippets/visualbasic/language-reference/error-messages/bc30451/program1.vb?highlight=7)]

   若要解决此错误，请将 `<VBRuntime>Default</VBRuntime>` 元素添加到 "项目" `<PropertyGroup>` 部分，如以下 Visual Basic 项目文件所示。

   [!code-xml[BC30451](~/samples/snippets/visualbasic/language-reference/error-messages/bc30451/vbruntime.vbproj?highlight=6)]

## <a name="see-also"></a>另请参阅

- [声明和常量摘要](../keywords/declarations-and-constants-summary.md)
- [Visual Basic 命名约定](../../programming-guide/program-structure/naming-conventions.md)
- [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)
- [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
