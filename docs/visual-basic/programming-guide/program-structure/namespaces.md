---
title: 命名空间
ms.date: 07/20/2015
f1_keywords:
- vb.global
helpviewer_keywords:
- assemblies [Visual Basic], namespaces
- name collisions
- Global keyword [Visual Basic]
- namespace pollution
- names [Visual Basic], avoiding conflicts
- naming conflicts [Visual Basic], namespaces
- namespaces [Visual Basic], assemblies
- Visual Basic code, namespaces
- fully qualified names [Visual Basic]
- naming conventions [Visual Basic], naming conflicts
- namespaces
ms.assetid: cffac744-ab8c-4f1f-ba50-732c22ab4b88
ms.openlocfilehash: f4521fa10c3bb9e8e121e3c228a23061becd1741
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072192"
---
# <a name="namespaces-in-visual-basic"></a>Visual Basic 中的命名空间

命名空间组织程序集中定义的对象。 程序集可以包含多个命名空间，命名空间又可以包含其他命名空间。 使用大组对象（比如类库）时，命名空间可以避免多义性和简化引用。  
  
 例如，.NET Framework 在 <xref:System.Windows.Forms.ListBox> 命名空间中定义类 <xref:System.Windows.Forms?displayProperty=nameWithType> 。 以下代码段演示如何使用此类的完全限定名称声明变量：  
  
 [!code-vb[VbVbalrApplication#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#6)]  
  
## <a name="avoiding-name-collisions"></a>避免名称冲突  

 .NET Framework 命名空间解决有时被称为 *命名空间污染*的问题，在这种情况下，类库的开发人员会受到另一个库中使用类似名称的阻碍。 这些冲突及其现有组件有时被称为 *名称冲突*。  
  
 例如，如果创建一个名为 `ListBox`的新类，你可以在项目中不加限定地使用它。 但是，如果您想要 <xref:System.Windows.Forms.ListBox> 在同一项目中使用 .NET Framework 类，则必须使用完全限定引用来使引用唯一。 如果引用不唯一，Visual Basic 将产生一个错误，指出名称不明确。 下面的代码示例演示如何声明这些对象：  
  
 [!code-vb[VbVbalrApplication#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#7)]  
  
 下图显示了两个命名空间层次结构，它们都包含一个名为的对象 `ListBox` ：  
  
 ![显示两个命名空间层次结构的屏幕截图。](./media/namespaces/visual-basic-namespace-hierarchy.gif)  
  
 默认情况下，使用 Visual Basic 创建的每个可执行文件都包含与项目具有相同名称的命名空间。 例如，如果在一个名为 `ListBoxProject`的项目中定义对象，则可执行文件 ListBoxProject.exe 包含一个名为 `ListBoxProject`的命名空间。  
  
 多个程序集可以使用相同的命名空间。 Visual Basic 将它们视为一组名称。 例如，你可以为名为 `SomeNameSpace` 的程序集中名为 `Assemb1`的命名空间定义多个类，并为名为 `Assemb2`的程序集中相同的命名空间定义其他类。  
  
## <a name="fully-qualified-names"></a>完全限定名  

 完全限定名是以在其中定义对象的命名空间的名称为前缀的对象引用。 如果创建对类的引用（通过选择“项目” **** 菜单中的“添加引用” **** ），然后为代码中的对象使用完全限定名，则可以使用在其他项目中定义的对象。 以下代码段演示如何为来自另一个项目命名空间的对象使用完全限定名：  
  
 [!code-vb[VbVbalrApplication#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#8)]  
  
 完全限定名避免命名冲突，因为它们可以使编译器确定哪些对象正在被使用。 但是，名称本身可能会变得冗长繁杂。 为避免这一问题，可以使用 `Imports` 语句来定义 *别名*- 可用于代替完全限定名的缩略名。 例如，以下代码示例为两个完全限定名创建别名，并使用这些别名来定义两个对象。  
  
 [!code-vb[VbVbalrApplication#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#9)]  
  
 [!code-vb[VbVbalrApplication#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#10)]  
  
 如果不带别名使用 `Imports` 语句，可以使用命名空间中的所有名称而无需限定，只要这些名称唯一。 如果项目包含同名的项的命名空间使用的 `Imports` 语句，则使用时必须完全限定该名称。 例如，假设项目包含以下两个 `Imports` 语句：  
  
 [!code-vb[VbVbalrApplication#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/Class1.vb#11)]  
  
 如果尝试使用 `Class1` 而不对其完全限定，Visual Basic 会生成一个错误，指出名称 `Class1` 不明确。  
  
## <a name="namespace-level-statements"></a>命名空间级别语句  

 在命名空间中，可以定义项，如模块、接口、类、委托、枚举、结构和其他命名空间。 无法在命名空间级别定义属性、过程、变量和事件等项。 这些项必须在模块、结构或类等容器内部进行声明。  
  
## <a name="global-keyword-in-fully-qualified-names"></a>完全限定名中的全局关键字  

 如果定义了命名空间的嵌套层次结构，该层次结构内的代码可能会被阻止访问 .NET Framework 的 <xref:System?displayProperty=nameWithType> 命名空间。 下面的示例阐释了 `SpecialSpace.System` 命名空间阻止访问 <xref:System?displayProperty=nameWithType>的层次结构。  
  
```vb  
Namespace SpecialSpace  
    Namespace System  
        Class abc  
            Function getValue() As System.Int32  
                Dim n As System.Int32  
                Return n  
            End Function  
        End Class  
    End Namespace  
End Namespace  
```  
  
 结果是，Visual Basic 编译器无法成功地解析对 <xref:System.Int32?displayProperty=nameWithType>的引用，因为 `SpecialSpace.System` 没有定义 `Int32`。 可以使用 `Global` 关键字在.NET Framework 类库的最外层级别上启动限定链。 这将允许你指定类库中的 <xref:System?displayProperty=nameWithType> 命名空间或任何其他命名空间。 下面的示例对此进行了演示。  
  
```vb  
Namespace SpecialSpace  
    Namespace System  
        Class abc  
            Function getValue() As Global.System.Int32  
                Dim n As Global.System.Int32  
                Return n  
            End Function  
        End Class  
    End Namespace  
End Namespace  
```  
  
 可以使用 `Global` 访问其他根级别的命名空间，如 <xref:Microsoft.VisualBasic?displayProperty=nameWithType>和与项目相关联的任何命名空间。  
  
## <a name="global-keyword-in-namespace-statements"></a>命名空间语句中的全局关键字  

 还可以使用 `Global` 中的 [Global](../../language-reference/statements/namespace-statement.md)。 这将允许你定义项目根命名空间之外的命名空间。  
  
 项目中的所有命名空间均基于项目的根命名空间。  Visual Studio 针对项目中的所有代码，将项目名称指定为默认根命名空间。 例如，如果项目命名为 `ConsoleApplication1`，则其编程元素属于命名空间 `ConsoleApplication1`。 如果声明 `Namespace Magnetosphere`，项目中对 `Magnetosphere` 的引用将访问 `ConsoleApplication1.Magnetosphere`。  
  
 以下示例使用 `Global` 关键字来为项目声明根命名空间之外的命名空间。  
  
 [!code-vb[VbVbalrApplication#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/module1.vb#22)]  
  
 在命名空间声明中， `Global` 不能嵌套于另一个命名空间中。  
  
 可以使用 [Application Page, Project Designer (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic) 查看和修改项目的“根命名空间” **** 。  对于新项目，“根命名空间” **** 默认为该项目的名称。 若要使 `Global` 成为顶级命名空间，可以清除“根命名空间” **** 条目，以便此框为空。 清除“根命名空间” **** 移除了命名空间声明中需要的 `Global` 关键字。  
  
 如果 `Namespace` 语句声明一个名称，而该名称也是.NET Framework 中的命名空间，则如果 `Global` 关键字未在完全限定名中使用，.NET Framework 命名空间会变得不可用。 在不使用 `Global` 关键字的情况下，若要启用对该 .NET Framework 命名空间的访问，可以将 `Global` 关键字包括到 `Namespace` 语句中。  
  
 以下示例在 `Global` 命名空间声明中具有 `System.Text` 关键字。  
  
 如果命名空间声明中不存在 `Global` 关键字，若不指定 <xref:System.Text.StringBuilder> 将无法访问 `Global.System.Text.StringBuilder`。 对于名为 `ConsoleApplication1`的项目，如果未使用 `System.Text` 关键字，对 `ConsoleApplication1.System.Text` 的引用会访问 `Global` 。  
  
 [!code-vb[VbVbalrApplication#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrApplication/VB/module1.vb#21)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Windows.Forms.ListBox>
- <xref:System.Windows.Forms?displayProperty=nameWithType>
- [.NET 中的程序集](../../../standard/assembly/index.md)
- [引用和 Imports 语句](references-and-the-imports-statement.md)
- [Imports 语句（.NET 命名空间和类型）](../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [在 Office 解决方案中编写代码](/visualstudio/vsto/writing-code-in-office-solutions)
