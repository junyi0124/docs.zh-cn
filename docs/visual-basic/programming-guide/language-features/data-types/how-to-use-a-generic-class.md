---
title: 如何：使用泛型类
ms.date: 07/20/2015
helpviewer_keywords:
- type parameters [Visual Basic], defining
- data type arguments [Visual Basic], defining
- arguments [Visual Basic], data types
- Of keyword [Visual Basic], using
- generic parameters
- data type parameters
- generics [Visual Basic], about generics
- data types [Visual Basic], as parameters
- data types [Visual Basic], as arguments
- parameters [Visual Basic], type
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- generics [Visual Basic], creating generic types
- data type arguments
- parameters [Visual Basic], data type
- data type parameters [Visual Basic], defining
- type arguments [Visual Basic], defining
- arguments [Visual Basic], type
ms.assetid: 242dd2a6-86c4-4ce7-83f2-f2661803f752
ms.openlocfilehash: 87ba5132afe9f5a7cd6fb33d716c670f4812c7e2
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077106"
---
# <a name="how-to-use-a-generic-class-visual-basic"></a>如何：使用泛型类 (Visual Basic)

采用 *类型参数* 的类称为 *泛型类*。 如果使用一个泛型类，则可以通过为每个形参提供 *类型实参* ，从该类生成 *构造类* 。 随后可以声明构造类类型的一个变量，可以创建构造类的实例并将它分配给该变量。  
  
 除了类之外，你还可以定义和使用泛型结构、接口、过程和委托。  
  
 下面的过程使用在 .NET Framework 中定义的泛型类，并从它创建一个实例。  
  
### <a name="to-use-a-class-that-takes-a-type-parameter"></a>使用采用类型参数的类  
  
1. 在源文件的开头，将 [Imports 语句 ( .Net 命名空间，并键入) ](../../../language-reference/statements/imports-statement-net-namespace-and-type.md) 以导入 <xref:System.Collections.Generic?displayProperty=nameWithType> 命名空间。 这使你可以引用 <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> 类，而不必完全限定它即可将它与其他队列类（如 <xref:System.Collections.Queue?displayProperty=nameWithType>）区分开来。  
  
2. 以正常方式创建对象，但是紧接在类名之后添加 `(Of type)` 。  
  
     下面的示例使用相同类 (<xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType>) 创建保存不同数据类型的项的两个队列对象。 它将项添加到每个队列末尾，然后从每个队列的前面删除并显示项。  
  
     [!code-vb[VbVbalrDataTypes#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>请参阅

- [数据类型](index.md)
- [Generic Types in Visual Basic](generic-types.md)
- [语言独立性和与语言无关的组件](../../../../standard/language-independence-and-language-independent-components.md)
- [个](../../../language-reference/statements/of-clause.md)
- [Imports 语句（.NET 命名空间和类型）](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [如何：定义可对不同数据类型提供相同功能的类](how-to-define-a-class-that-can-provide-identical-functionality.md)
- [迭代器](../../concepts/iterators.md)
