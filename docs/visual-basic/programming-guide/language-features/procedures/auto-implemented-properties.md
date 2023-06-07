---
title: 自动实现的属性
ms.date: 07/20/2015
f1_keywords:
- vb.AutoProperty
- vb.AutoImplementedProperty
helpviewer_keywords:
- properties [Visual Basic], auto-implemented
- auto-implemented properties [Visual Basic]
ms.assetid: 5c669f0b-cf95-4b4e-ae84-9cc55212ca87
ms.openlocfilehash: f50b1f40ef9843391c6622561bfd8a8eaae6fc17
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91090049"
---
# <a name="auto-implemented-properties-visual-basic"></a>自动实现的属性 (Visual Basic)

使用*自动实现的属性*，可以快速指定类的属性，而无需向 `Get` 和属性写入代码 `Set` 。 为自动实现的属性编写代码时，Visual Basic 编译器会自动创建私有字段以存储属性变量，并且会创建关联的 `Get` 和 `Set` 过程。  
  
 使用自动实现的属性，可以在单行中声明一个属性（包括默认值）。 下面的示例演示三个属性声明。  
  
 [!code-vb[VbVbalrAutoImplementedProperties#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#1)]  
  
 自动实现的属性等效于其属性值存储在私有字段中的属性。 下面的代码示例演示一个自动实现的属性。  
  
 [!code-vb[VbVbalrAutoImplementedProperties#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#5)]  
  
 下面的代码示例演示上面自动实现的属性示例的等效代码。  
  
 [!code-vb[VbVbalrAutoImplementedProperties#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#2)]  
  
 以下代码演示如何实现只读属性：  
  
```vb  
Class Customer  
   Public ReadOnly Property Tags As New List(Of String)  
   Public ReadOnly Property Name As String = ""  
   Public ReadOnly Property File As String  
  
   Sub New(file As String)  
      Me.File = file  
   End Sub  
End Class  
```  
  
 可以使用初始化表达式分配给属性（如示例中所示），也可以在包含类型的构造函数中分配给属性。  可以随时分配给只读属性的支持字段。  
  
## <a name="backing-field"></a>支持字段  

 当你声明自动实现的属性时，Visual Basic 会自动创建一个名为 " *支持" 字段* 的隐藏私有字段来包含属性值。 支持字段名是前面带下划线 (_) 的自动实现的属性名。 例如，如果声明一个名为 `ID` 的自动实现的属性，则支持字段名为 `_ID`。 如果包含一个也名为 `_ID` 的类成员，则会形成命名冲突，Visual Basic 会报告编译器错误。  
  
 支持字段还具有下列特征：  
  
- 支持字段的访问修饰符始终是 `Private`，即使在属性本身具有不同访问级别（如 `Public`）时也是如此。  
  
- 如果属性标记为 `Shared`，则支持字段也进行共享。  
  
- 为属性指定的属性不适用于支持字段。  
  
- 可以从类中的代码以及从调试工具（如监视窗口）访问支持字段。 但是，支持字段不显示在 IntelliSense 文字自动完成列表中。  
  
## <a name="initializing-an-auto-implemented-property"></a>初始化自动实现的属性  

 任何可以用于实例化字段的表达式对于初始化自动实现的属性都是有效的。 初始化自动实现的属性时，表达式会进行计算并传递给属性的 `Set` 过程。 下面的代码示例演示一些包含初始值的自动实现的属性。  
  
 [!code-vb[VbVbalrAutoImplementedProperties#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#3)]  
  
 无法初始化作为 `Interface` 的成员或标记为 `MustOverride` 的自动实现的属性。  
  
 将自动实现的属性声明为 `Structure` 的成员时，如果自动实现的属性标记为 `Shared`，则只能初始化它。  
  
 将自动实现的属性声明为数组时，不能指定显式数组边界。 但是，可以使用数组初始值设定项提供值，如下面的示例所示。  
  
 [!code-vb[VbVbalrAutoImplementedProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrautoimplementedproperties/vb/module1.vb#4)]  
  
## <a name="property-definitions-that-require-standard-syntax"></a>需要标准语法的属性定义  

 自动实现的属性十分方便，支持很多编程方案。 但是，在某些情况下，你不能使用自动实现的属性，而是必须使用标准或 *扩展*的属性语法。  
  
 如果要执行以下任一操作，则必须使用扩展属性定义语法：  
  
- 向属性的 `Get` 或 `Set` 过程添加代码，如用于在 `Set` 过程中验证传入值的代码。 例如，你可能要在设置属性值之前验证表示电话号码的字符串是否包含所需数量的数字。  
  
- 为 `Get` 和 `Set` 过程指定不同的可访问性。 例如，你可能要使 `Set` 过程是 `Private`，要使 `Get` 过程是 `Public`。  
  
- 创建作为 `WriteOnly` 的属性。  
  
- 使用参数化的属性（包括 `Default` 属性）。 必须声明扩展属性才能为属性指定参数或是为 `Set` 过程指定附加参数。  
  
- 将属性置于支持字段上，或更改支持字段的访问级别。  
  
- 为支持字段提供 XML 注释。  
  
## <a name="expanding-an-auto-implemented-property"></a>扩展自动实现的属性  

 如果需要将自动实现的属性转换为包含 `Get` 或 `Set` 过程的展开属性，则 Visual Basic 代码编辑器可以为属性自动生成 `Get` 和 `Set` 过程以及 `End Property` 语句。 如果将光标置于语句后面的空行上，则会生成代码 `Property` ， `G` 为 `Get`) 或)  (键入一个 (， `S` `Set` 然后按 enter。 在 `Property` 语句末尾按 Enter 时，Visual Basic 代码编辑器会为只读和只写属性自动生成 `Get` 或 `Set` 过程。  
  
## <a name="see-also"></a>请参阅

- [如何：在 Visual Basic 中声明和调用默认属性](./how-to-declare-and-call-a-default-property.md)
- [如何：声明具有混合访问级别的属性](./how-to-declare-a-property-with-mixed-access-levels.md)
- [Property Statement](../../../language-reference/statements/property-statement.md)
- [ReadOnly](../../../language-reference/modifiers/readonly.md)
- [WriteOnly](../../../language-reference/modifiers/writeonly.md)
- [对象和类](../objects-and-classes/index.md)
