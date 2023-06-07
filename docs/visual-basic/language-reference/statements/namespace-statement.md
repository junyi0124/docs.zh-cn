---
title: Namespace 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Namespace
helpviewer_keywords:
- namespaces [Visual Basic], root
- Namespace statement [Visual Basic]
- namespaces [Visual Basic], nested
- declaring namespaces [Visual Basic], syntax
- namespaces [Visual Basic], declaring
- root namespaces
- declarations [Visual Basic], namespaces
ms.assetid: a31fbd95-9ace-4c3d-bbb1-51222a2272b2
ms.openlocfilehash: cef339a66458ee9657dc1706082c3c5328746dc6
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875211"
---
# <a name="namespace-statement"></a>Namespace 语句

声明命名空间的名称，并导致在该命名空间中编译跟在声明后的源代码。  
  
## <a name="syntax"></a>语法  
  
```vb  
Namespace [Global.] { name | name.name }  
    [ componenttypes ]  
End Namespace  
```  
  
## <a name="parts"></a>组成部分  

 全球  
 可选。 允许你在项目的根命名空间外定义命名空间。 请参阅 [Visual Basic 中的命名空间](../../programming-guide/program-structure/namespaces.md)。  
  
 `name`  
 必需。 标识命名空间的唯一名称。 必须是有效的 Visual Basic 标识符。 有关详细信息，请参阅已 [声明的元素名称](../../programming-guide/language-features/declared-elements/declared-element-names.md)。  
  
 `componenttypes`  
 可选。 构成命名空间的元素。 其中包括但不限于枚举、结构、接口、类、模块、委托和其他命名空间。  
  
 `End Namespace`  
 终止 `Namespace` 块。  
  
## <a name="remarks"></a>备注  

 命名空间用作组织系统。 它们提供了一种方法来分类和显示向其他程序和应用程序公开的编程元素。 请注意，在类或结构为的意义上，命名空间不是 *类型* ，不能将编程元素声明为具有命名空间的数据类型。  
  
 语句后声明的所有编程元素都 `Namespace` 属于该命名空间。 Visual Basic 继续将元素编译到最后一个声明的命名空间中，直到它遇到 `End Namespace` 语句或另一个 `Namespace` 语句。  
  
 如果命名空间已定义，即使是项目外部的命名空间，也可以向其中添加编程元素。 为此，请使用 `Namespace` 语句来指示 Visual Basic 将元素编译到该命名空间中。  
  
 只能 `Namespace` 在文件或命名空间级别使用语句。 这意味着命名空间的 *声明上下文* 必须是源文件或其他命名空间，不能是类、结构、模块、接口或过程。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。  
  
 可以在另一个命名空间中声明一个命名空间。 您可以声明的嵌套级别没有严格限制，但是请记住，如果其他代码访问在最内层命名空间中声明的元素，则必须使用包含嵌套层次结构中所有命名空间名称的限定字符串。  
  
## <a name="access-level"></a>访问级别  

 命名空间将被视为具有 `Public` 访问级别。 可以从同一项目中的任何位置、引用该项目的其他项目以及从项目生成的任何程序集访问该命名空间。  
  
 在命名空间级别声明的编程元素（在命名空间中，而不是在任何其他元素内）可以具有 `Public` 或 `Friend` 访问。 如果未指定，则默认情况下，此类元素的访问级别使用 `Friend` 。 可在命名空间级别声明的元素包括类、结构、模块、接口、枚举和委托。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。  
  
## <a name="root-namespace"></a>根命名空间  

 项目中的所有命名空间名称都基于 *根命名空间*。 Visual Studio 针对项目中的所有代码，将项目名称指定为默认根命名空间。 例如，如果项目命名为 `Payroll`，则其编程元素属于命名空间 `Payroll`。 如果声明 `Namespace funding` ，则该命名空间的完整名称为 `Payroll.funding` 。  
  
 如果要在语句中指定现有命名空间（ `Namespace` 如泛型列表类示例中所示），可以将根命名空间设置为 null 值。 为此，请单击 "**项目**" 菜单中的 "**项目属性**"，然后清除 "**根命名空间**" 条目，以便此框为空。 如果未在泛型列表类示例中执行此操作，则 Visual Basic 编译器会将 `System.Collections.Generic` 项目中的新命名空间作为的 `Payroll` 完整名称 `Payroll.System.Collections.Generic` 。  
  
 或者，可以使用 `Global` 关键字来引用在项目外部定义的命名空间的元素。 这样做可以使项目名称保留为根命名空间。 这可以减少无意中将您的编程元素与现有命名空间的合并。 有关详细信息，请参阅 [Visual Basic 中命名空间](../../programming-guide/program-structure/namespaces.md)的 "完全限定名称中的全局关键字" 部分。  
  
 `Global`关键字还可以在命名空间语句中使用。 这将允许你定义项目根命名空间之外的命名空间。 有关详细信息，请参阅 [Visual Basic 的命名空间](../../programming-guide/program-structure/namespaces.md)中的 "命名空间语句中的全局关键字" 部分。  
  
 **有关.** 根命名空间可能会导致命名空间名称出现意外的串联。 如果你引用在项目外部定义的命名空间，则 Visual Basic 编译器可以将它们作为根命名空间中的嵌套命名空间 construe。 在这种情况下，编译器不能识别已在外部命名空间中定义的任何类型。 若要避免这种情况，请将根命名空间设置为空值（如 "根命名空间" 中所述）或使用 `Global` 关键字访问外部命名空间的元素。  
  
## <a name="attributes-and-modifiers"></a>特性和修饰符  

 不能将属性应用到命名空间。 特性向程序集的元数据提供信息，这对于源分类器（如命名空间）没有意义。  
  
 不能将任何访问或过程修饰符或任何其他修饰符应用到命名空间。 由于这不是类型，因此这些修饰符没有意义。  
  
## <a name="example"></a>示例  

 下面的示例声明了两个命名空间，一个嵌套在另一个。  
  
 [!code-vb[VbVbalrStatements#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#43)]  
  
## <a name="example"></a>示例  

 下面的示例在一行上声明多个嵌套命名空间，并且它等效于前面的示例。  
  
 [!code-vb[VbVbalrStatements#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#41)]  
  
## <a name="example"></a>示例  

 以下示例访问在前面的示例中定义的类。  
  
 [!code-vb[VbVbalrStatements#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#42)]  
  
## <a name="example"></a>示例  

 下面的示例定义了一个新的泛型列表类的主干，并将其添加到了 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间。  
  
```vb  
Namespace System.Collections.Generic  
    Class specialSortedList(Of T)  
        Inherits List(Of T)  
        ' Insert code to define the special generic list class.  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>另请参阅

- [Imports 语句（.NET 命名空间和类型）](imports-statement-net-namespace-and-type.md)
- [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)
- [Visual Basic 中的命名空间](../../programming-guide/program-structure/namespaces.md)
