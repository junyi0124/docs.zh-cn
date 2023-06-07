---
title: References to Declared Elements
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic]
- references [Visual Basic], declared elements
- qualified names [Visual Basic]
ms.assetid: d6301709-f4cc-4b7a-b8ba-80898f14ab46
ms.openlocfilehash: af5be47335b6d48bd6c0bccc30b8db15c9912807
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085875"
---
# <a name="references-to-declared-elements-visual-basic"></a>对已声明元素的引用 (Visual Basic)

当代码引用已声明的元素时，Visual Basic 编译器会将引用中的名称与该名称的相应声明进行匹配。 如果用相同的名称声明了多个元素，则可以通过 *限定* 其名称来控制要引用的元素。  
  
 编译器尝试将名称引用与最 *窄的范围*进行匹配。 这意味着，它从发出引用的代码开始，并通过连续的包含元素级别进行处理。  
  
 下面的示例显示对两个名称相同的变量的引用。 该示例在 `totalCount` 模块的不同范围内声明了两个变量，每个变量都命名为 `container` 。 如果过程 `showCount` 显示时 `totalCount` 未进行限定，则 Visual Basic 编译器会将引用解析为范围最窄的声明，即内的本地声明 `showCount` 。 当它符合 `totalCount` 包含模块时 `container` ，编译器会将引用解析为范围更广的声明。  
  
```vb  
' Assume these two modules are both in the same assembly.  
Module container  
    Public totalCount As Integer = 1  
    Public Sub showCount()  
        Dim totalCount As Integer = 6000  
        ' The following statement displays the local totalCount (6000).  
        MsgBox("Unqualified totalCount is " & CStr(totalCount))  
        ' The following statement displays the module's totalCount (1).  
        MsgBox("container.totalCount is " & CStr(container.totalCount))  
    End Sub  
End Module  
Module callingModule  
    Public Sub displayCount()  
        container.showCount()  
        ' The following statement displays the containing module's totalCount (1).  
        MsgBox("container.totalCount is " & CStr(container.totalCount))  
    End Sub  
End Module  
```  
  
## <a name="qualifying-an-element-name"></a>限定元素名称  

 如果要重写此搜索过程并指定在更大范围内声明的名称，则必须使用范围更广的包含元素 *限定* 该名称。 在某些情况下，你可能还必须限定包含元素。  
  
 限定名称表示在源语句中使用标识目标元素定义位置的信息。 此信息称为 *限定字符串*。 它可以包含一个或多个命名空间以及模块、类或结构。  
  
 限定字符串应明确指定包含目标元素的模块、类或结构。 容器可能又位于另一个包含元素中，通常是一个命名空间。 你可能需要在限定字符串中包含多个包含元素。  
  
#### <a name="to-access-a-declared-element-by-qualifying-its-name"></a>通过限定其名称访问声明的元素  
  
1. 确定定义元素的位置。 这可能包括命名空间，甚至命名空间的层次结构。 在最低级别的命名空间中，元素必须包含在模块、类或结构中。  
  
    ```vb  
    ' Assume the following hierarchy exists outside your code.  
    Namespace outerSpace  
        Namespace innerSpace  
            Module holdsTotals  
                Public Structure totals  
                    Public thisTotal As Integer  
                    Public Shared grandTotal As Long  
                End Structure  
            End Module  
        End Namespace  
    End Namespace  
    ```  
  
2. 根据目标元素的位置确定限定路径。 从最高层命名空间开始，转到最低级别命名空间，并以包含目标元素的模块、类或结构结尾。 路径中的每个元素都必须包含其后的元素。  
  
     `outerSpace` → `innerSpace` → `holdsTotals` → `totals`  
  
3. 准备目标元素的限定字符串。 将一个句点 (在 `.` 路径中的每个元素之后) 。 应用程序必须有权访问限定字符串中的每个元素。  
  
    ```vb  
    outerSpace.innerSpace.holdsTotals.totals.  
    ```  
  
4. 以正常方式编写引用目标元素的表达式或赋值语句。  
  
    ```vb  
    grandTotal = 9000  
    ```  
  
5. 在目标元素名称之前加上限定字符串。 该名称应紧跟 `.` 在包含元素的模块、类或结构 () 时间段之后。  
  
    ```vb  
    ' Assume the following module is part of your code.  
    Module accessGrandTotal  
        Public Sub setGrandTotal()  
            outerSpace.innerSpace.holdsTotals.totals.grandTotal = 9000  
        End Sub  
    End Module  
    ```  
  
6. 编译器使用限定字符串来查找可与目标元素引用匹配的明确、明确的声明。  
  
 如果你的应用程序有权访问多个具有相同名称的编程元素，则可能还必须限定名称引用。 例如， <xref:System.Windows.Forms> 和 <xref:System.Web.UI.WebControls> 命名空间都包含一个 `Label` 类 (<xref:System.Windows.Forms.Label?displayProperty=nameWithType> 和 <xref:System.Web.UI.WebControls.Label?displayProperty=nameWithType>) 。 如果你的应用程序同时使用这两个，或者它定义自己的 `Label` 类，则必须区分不同的 `Label` 对象。 在变量声明中包含命名空间或导入别名。 下面的示例使用了导入别名。  
  
```vb  
' The following statement must precede all your declarations.  
Imports win = System.Windows.Forms, web = System.Web.UI.WebControls  
' The following statement references the Windows.Forms.Label class.  
Dim winLabel As New win.Label()  
```  
  
## <a name="members-of-other-containing-elements"></a>其他包含元素的成员  

 如果使用其他类或结构的非共享成员，则必须先使用指向类或结构的实例的变量或表达式来限定成员名称。 在下面的示例中， `demoClass` 是名为的类的实例 `class1` 。  
  
```vb  
Dim demoClass As class1 = New class1()  
demoClass.someSub[(argumentlist)]  
```  
  
 不能使用类名本身来限定未 [共享](../../../language-reference/modifiers/shared.md)的成员。 在此示例中，您必须首先在对象 (变量中创建实例 `demoClass`) 然后通过变量名称引用它。  
  
 如果某个类或结构具有 `Shared` 成员，则可以使用类或结构名称或指向实例的变量或表达式来限定该成员。  
  
 模块没有任何单独的实例，并且默认情况下其所有成员均为 `Shared` 。 因此，您可以使用模块名称限定模块成员。  
  
 下面的示例演示对模块成员过程的限定引用。 该示例 `Sub` `perform` 在项目的不同模块中声明了两个名为的过程。 可以在其自身的模块中无需限定地指定每个项，但在其他任何地方引用时必须进行限定。 因为中的最后一个引用 `module3` 不符合条件 `perform` ，所以编译器无法解析该引用。  
  
```vb  
' Assume these three modules are all in the same assembly.  
Module module1  
    Public Sub perform()  
        MsgBox("module1.perform() now returning")  
    End Sub  
End Module  
Module module2  
    Public Sub perform()  
        MsgBox("module2.perform() now returning")  
    End Sub  
    Public Sub doSomething()  
        ' The following statement calls perform in module2, the active module.  
        perform()  
        ' The following statement calls perform in module1.  
        module1.perform()  
    End Sub  
End Module  
Module module3  
    Public Sub callPerform()  
        ' The following statement calls perform in module1.  
        module1.perform()  
        ' The following statement makes an unresolvable name reference  
        ' and therefore generates a COMPILER ERROR.  
        perform() ' INVALID statement  
    End Sub  
End Module  
```  
  
## <a name="references-to-projects"></a>对项目的引用  

 若要使用另一个项目中定义的 [公共](../../../language-reference/modifiers/public.md) 元素，必须首先设置对该项目的程序集或类型库的 *引用* 。 若要设置引用，请在 "**项目**" 菜单上单击 "**添加引用**"，或使用[引用 (Visual Basic) ](../../../reference/command-line-compiler/reference.md)命令行编译器选项。  
  
 例如，可以使用 .NET Framework 的 XML 对象模型。 如果设置了对 <xref:System.Xml> 命名空间的引用，则可以声明并使用它的任何类，如 <xref:System.Xml.XmlDocument> 。 下面的示例使用 <xref:System.Xml.XmlDocument>。  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As System.Xml.XmlDocument  
```  
  
## <a name="importing-containing-elements"></a>导入包含元素  

 您可以使用 import [语句 ( .Net 命名空间，并键入) ](../../../language-reference/statements/imports-statement-net-namespace-and-type.md) 以 *导入* 包含您要使用的模块或类的命名空间。 这使您可以引用在导入的命名空间中定义的元素，而无需完全限定其名称。 下面的示例重写上一示例以导入 <xref:System.Xml> 命名空间。  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement must precede all your declarations.  
Imports System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As XmlDocument  
```  
  
 此外， `Imports` 语句可以为每个导入的命名空间定义 *导入别名* 。 这会使源代码更短且更易于阅读。 下面的示例重写上一个示例，以 `xD` 用作 <xref:System.Xml> 命名空间的别名。  
  
```vb  
' Assume this project has a reference to System.Xml  
' The following statement must precede all your declarations.  
Imports xD = System.Xml  
' The following statement creates xDoc as an XML document object.  
Dim xDoc As xD.XmlDocument  
```  
  
 `Imports`语句不会使其他项目中的元素可用于你的应用程序。 也就是说，它不会取代设置引用。 导入命名空间只是不再要求限定命名空间中定义的名称。  
  
 你还可以使用 `Imports` 语句来导入模块、类、结构和枚举。 然后，可以使用此类导入元素的成员而无需进行限定。 但是，必须始终使用计算结果为类或结构的实例的变量或表达式来限定类和结构的非共享成员。  
  
## <a name="naming-guidelines"></a>命名准则  

 在定义两个或多个具有相同名称的编程元素时，当编译器尝试解析对该名称的引用时，可能会导致 *名称不明确* 。 如果范围内有多个定义，或者如果没有定义在范围内，则引用为不能纠正。 有关示例，请参阅此帮助页上的 "限定引用示例"。  
  
 可以通过提供所有元素的唯一名称来避免名称不明确。 然后，可以引用任何元素，而无需使用命名空间、模块或类限定其名称。 您还可以减少意外引用错误元素的几率。  
  
## <a name="shadowing"></a>阴影操作  

 当两个编程元素共享同一名称时，其中一个元素可以 *隐藏或隐藏*另一个。 隐藏的元素不可用于引用;相反，当代码使用隐藏的元素名称时，Visual Basic 编译器会将其解析为隐藏元素。 有关示例的更详细说明，请参阅 [Visual Basic 中的隐藏](shadowing.md)。  
  
## <a name="see-also"></a>请参阅

- [Declared Element Names](declared-element-names.md)
- [已声明元素的特性](declared-element-characteristics.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
- [变量](../variables/index.md)
- [Imports 语句（.NET 命名空间和类型）](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [New 运算符](../../../language-reference/operators/new-operator.md)
- [公共](../../../language-reference/modifiers/public.md)
