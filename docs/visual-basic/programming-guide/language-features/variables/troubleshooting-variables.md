---
title: 变量疑难解答
ms.date: 07/20/2015
helpviewer_keywords:
- troubleshooting [Visual Basic], variables
- variables [Visual Basic], troubleshooting
ms.assetid: 928a2dc8-e565-4ae4-8ba3-80cc0cb50090
ms.openlocfilehash: e2bcf0b779d98ea4b109ea6d33b69a15110d423c
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91080161"
---
# <a name="troubleshooting-variables-in-visual-basic"></a>变量疑难解答 (Visual Basic)

此页列出了在使用 Visual Basic 中的变量时可能出现的一些常见问题。  
  
## <a name="unable-to-access-members-of-an-object"></a>无法访问对象的成员  

 如果你的代码尝试访问对象上的属性或方法，可能会出现两种错误结果：  
  
- 如果你声明对象变量为特定类型，然后引用不是由此类型定义的成员，则编译器可能生成错误消息。  
  
- 当分配给对象变量的对象不显示代码正尝试访问的成员时，将产生运行时 <xref:System.MemberAccessException> 。 当变量为 [Object Data Type](../../../language-reference/data-types/object-data-type.md)，如果成员不是 `Public`中的变量时可能出现的一些常见问题。 这是因为后期绑定只允许访问 `Public` 成员。  
  
 当 [Option Strict Statement](../../../language-reference/statements/option-strict-statement.md) 设置类型检查 `On`时，对象变量仅可访问声明它所用的类的方法和属性。 下面的示例对此进行了演示。  

 [!code-vb[VbVbalrVariables#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrVariables/VB/Class1.vb#2)]  
  
 在此示例中， `p` 仅可使用 <xref:System.Object> 类的成员，这不包括 `Left` 属性。 另一方面， `q` 已声明为 <xref:System.Windows.Forms.Label>类型，因此它可以使用 <xref:System.Windows.Forms.Label> 命名空间中 <xref:System.Windows.Forms> 类的所有方法和属性。  
  
### <a name="correct-approach"></a>正确方法  

 若要能够访问特定类的对象的所有成员，则尽可能将对象变量声明为该类的类型。 如果你不能这样做（例如如果你不知道编译时的对象类型），则必须将 `Option Strict` 设置为 `Off` ，并声明变量为 [Object Data Type](../../../language-reference/data-types/object-data-type.md)中的变量时可能出现的一些常见问题。 这将允许将任意类型的对象分配给该变量，你应该采取措施确保当前分配的对象是可接受的类型。 可以使用 [TypeOf 运算符](../../../language-reference/operators/typeof-operator.md) 来做出此决定。  
  
## <a name="other-components-cannot-access-your-variable"></a>其他组件不能访问你的变量  

 Visual Basic 名称不 *区分大小写*。 如果这两个名称只是字母大小写不同，编译器会将其解析为相同的名称。 例如，它认为 `ABC` 和 `abc` 指的是同一个声明的元素。  
  
 但是，公共语言运行时 (CLR) 使用 *区分大小写* 绑定。 因此，当你生成程序集或 DLL 并使其可供其他程序集使用时，则名称将不再不区分大小写。 例如，如果你用名为 `ABC`的元素定义类，而其他程序集通过公共语言运行时使用你的类，则元素必须指的是 `ABC`。 如果你以后重新编译你的类并将元素名称更改为 `abc`，则使用你的类的其他程序集将不能再访问此元素。 因此，发布程序集的更新版本时，不应该更改公共元素的字母大小写。  
  
 有关详细信息，请参阅 [Common Language Runtime](../../../../standard/clr.md)。  
  
### <a name="correct-approach"></a>正确方法  

 若要允许其他组件访问你的变量，将其名称视为区分大小写。 当测试类或模块时，确保其他程序集绑定到你所希望的变量。 发布组件后，请勿对现有变量名称进行修改，包括更改大小写。  
  
## <a name="wrong-variable-being-used"></a>正在使用的错误变量  

 如果有多个同名变量，则 Visual Basic 编译器会尝试解析对该名称的每个引用。 如果变量具有不同的范围，编译器将解析对范围最小的声明的引用。 如果它们具有相同的范围，解析将失败，编译器将引发错误。 有关详细信息，请参阅 [References to Declared Elements](../declared-elements/references-to-declared-elements.md)。  
  
### <a name="correct-approach"></a>正确方法  

 避免使用名称相同范围不同的变量。 如果你正在使用其他程序集或项目，尽量避免使用在那些外部组件中定义的任何名称。 如果你有名称相同的多个变量，确保限定对它的每个引用。 有关详细信息，请参阅 [References to Declared Elements](../declared-elements/references-to-declared-elements.md)。  
  
## <a name="see-also"></a>请参阅

- [变量](index.md)
- [变量声明](variable-declaration.md)
- [对象变量](object-variables.md)
- [对象变量声明](object-variable-declaration.md)
- [如何：访问对象的成员](how-to-access-members-of-an-object.md)
- [对象变量值](object-variable-values.md)
- [如何：确定对象变量引用的类型](how-to-determine-what-type-an-object-variable-refers-to.md)
- [References to Declared Elements](../declared-elements/references-to-declared-elements.md)
- [Declared Element Names](../declared-elements/declared-element-names.md)
