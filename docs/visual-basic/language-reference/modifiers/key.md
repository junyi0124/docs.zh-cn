---
title: 密钥
ms.date: 07/20/2015
f1_keywords:
- vb.AnonymousKey
helpviewer_keywords:
- anonymous types [Visual Basic], key
- Key [Visual Basic]
- Key keyword [Visual Basic]
ms.assetid: 7697a928-7d14-4430-a72a-c9e96e8d6c11
ms.openlocfilehash: 582ed5bb67b9c7504e736710aa4649cffb12ef45
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867998"
---
# <a name="key-visual-basic"></a>Key (Visual Basic)

关键字可用于 `Key` 指定匿名类型的属性的行为。 只有指定为键属性的属性才会在匿名类型实例与哈希代码值的计算之间进行相等性测试。 键属性的值不能更改。  
  
 将匿名类型的属性指定为键属性，方法是在 `Key` 初始化列表中将关键字置于其声明之前。 在下面的示例中， `Airline` 和 `FlightNo` 是键属性，但 `Gate` 不是。  
  
 [!code-vb[VbVbalrAnonymousTypes#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#26)]  
  
 创建新的匿名类型时，它会直接从继承 <xref:System.Object> 。 编译器会重写三个继承成员： <xref:System.Object.Equals%2A> 、 <xref:System.Object.GetHashCode%2A> 和 <xref:System.Object.ToString%2A> 。 为和生成的替代代码 <xref:System.Object.Equals%2A> <xref:System.Object.GetHashCode%2A> 基于键属性。 如果类型中没有键属性，则不会 <xref:System.Object.GetHashCode%2A> <xref:System.Object.Equals%2A> 重写这些属性。  
  
## <a name="equality"></a>等式  

 如果两个匿名类型实例是同一类型的实例，并且它们的键属性的值相等，则这两个实例相等。 在下面的示例中，与 `flight2` `flight1` 上一示例中的相等，因为它们是相同匿名类型的实例，并且它们的键属性具有匹配的值。 但是， `flight3` 不等于， `flight1` 因为它的键属性具有不同的值 `FlightNo` 。 实例 `flight4` 的类型 `flight1` 不同，因为它们将不同的属性指定为键属性。  
  
 [!code-vb[VbVbalrAnonymousTypes#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#27)]  
  
 如果仅用非键属性声明了两个实例，则在名称、类型、顺序和值上相同，这两个实例不相等。 没有键属性的实例仅与自身相等。  
  
 有关这两个匿名类型实例是同一匿名类型的实例的详细信息，请参阅 [匿名类型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)。  
  
## <a name="hash-code-calculation"></a>哈希代码计算  

 与一样 <xref:System.Object.Equals%2A> ，在中为匿名类型定义的哈希函数 <xref:System.Object.GetHashCode%2A> 基于该类型的键属性。 下面的示例演示键属性和哈希代码值之间的交互。  
  
 与所有键属性具有相同值的匿名类型的实例具有相同的哈希代码值，即使非键属性没有匹配的值也是如此。 下面的语句将返回 `True`。  
  
 [!code-vb[VbVbalrAnonymousTypes#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#37)]  
  
 对于一个或多个键属性具有不同值的匿名类型的实例具有不同的哈希代码值。 下面的语句将返回 `False`。  
  
 [!code-vb[VbVbalrAnonymousTypes#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#38)]  
  
 将不同属性指定为键属性的匿名类型的实例不是相同类型的实例。 即使所有属性的名称和值相同，它们也具有不同的哈希代码值。 下面的语句将返回 `False`。  
  
 [!code-vb[VbVbalrAnonymousTypes#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#39)]  
  
## <a name="read-only-values"></a>只读值  

 键属性的值不能更改。 例如，在 `flight1` 前面的示例中， `Airline` 和 `FlightNo` 字段是只读的，但 `Gate` 可以更改。  
  
 [!code-vb[VbVbalrAnonymousTypes#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class2.vb#28)]  
  
## <a name="see-also"></a>另请参阅

- [匿名类型定义](../../programming-guide/language-features/objects-and-classes/anonymous-type-definition.md)
- [如何：推断匿名类型声明中的属性名和类型](../../programming-guide/language-features/objects-and-classes/how-to-infer-property-names-and-types-in-anonymous-type-declarations.md)
- [匿名类型](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)
