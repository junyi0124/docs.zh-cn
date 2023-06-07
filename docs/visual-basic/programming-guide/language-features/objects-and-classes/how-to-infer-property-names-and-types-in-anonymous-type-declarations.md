---
title: 如何：推断匿名类型声明中的属性名和类型
ms.date: 07/20/2015
helpviewer_keywords:
- inferring property names [Visual Basic]
- anonymous types [Visual Basic], inferring property names and types
- inferring property types [Visual Basic]
ms.assetid: 7c748b22-913f-4d9d-b747-6b7bf296a0bc
ms.openlocfilehash: 9ebbbe9d2e6d36f5ab2bc7f7c916d18c9240a06d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404883"
---
# <a name="how-to-infer-property-names-and-types-in-anonymous-type-declarations-visual-basic"></a>如何：推断匿名类型声明中的属性名和类型 (Visual Basic)

匿名类型不提供直接指定属性的数据类型的机制。 所有属性的类型都是推断出来的。 下面的示例从用于初始化属性的值，直接推断 `Name` 和 `Price` 属性的类型。

[!code-vb[VbVbalrAnonymousTypes#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#1)]

匿名类型还可以从其他来源推断属性名和类型。 以下各部分提供了可以进行推断的情况列表，以及不能进行推断的情况的示例。

## <a name="successful-inference"></a>成功推理

#### <a name="anonymous-types-can-infer-property-names-and-types-from-the-following-sources"></a>匿名类型可以从以下来源推断属性名和类型：

- 从变量名称。 匿名类型 `anonProduct` 将具有两个属性： `productName` 和 `productPrice`。 它们的数据类型将是原始变量的数据类型，分别为 `String` 和 `Double`。

  [!code-vb[VbVbalrAnonymousTypes#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#11)]

- 从其他对象的属性或字段名称。 例如，考虑包含 `car` 和 `CarClass` 属性的 `Name` 类型的 `ID` 对象。 若要创建一个匿名类型实例 `car1`，并且其 `Name` 和 `ID` 属性使用 `car` 对象的值进行初始化，则可以编写下面的代码：

  [!code-vb[VbVbalrAnonymousTypes#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#34)]

  以上声明等效于定义匿名类型 `car2`的一行较长的代码。

  [!code-vb[VbVbalrAnonymousTypes#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#35)]

- 从 XML 成员名称。

  [!code-vb[VbVbalrAnonymousTypes#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#12)]

  为 `anon` 产生的类型将具有一个属性 `Book`，其类型为 <xref:System.Collections.IEnumerable>(Of XElement)。

- 从没有参数的函数，例如下面示例中的 `SomeFunction` 。

  ```vb
  Dim sc As New SomeClass
  Dim anon1 = New With {Key sc.SomeFunction()}
  ```

  下面代码中的变量 `anon2` 是具有一个属性（一个名为 `First`的字符）的匿名类型。 这段代码将显示函数 <xref:System.Linq.Enumerable.First%2A>返回的字母“E”。

  [!code-vb[VbVbalrAnonymousTypes#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#13)]

## <a name="inference-failures"></a>推理失败

#### <a name="name-inference-will-fail-in-many-circumstances-including-the-following"></a>名称推理在许多情况下将失败，包括：

- 推理派生自对方法、构造函数或需要参数的参数化属性的调用。 如果 `anon1` 具有一个或多个参数，则上面的 `someFunction` 声明将失败。

  ```vb
  ' Not valid.
  ' Dim anon3 = New With {Key sc.someFunction(someArg)}
  ```

  向新属性名赋值可以解决这一问题。

  ```vb
  ' Valid.
  Dim anon4 = New With {Key .FunResult = sc.someFunction(someArg)}
  ```

- 推理派生自复杂表达式。

  ```vb
  Dim aString As String = "Act "
  ' Not valid.
  ' Dim label = New With {Key aString & "IV"}
  ```

  将表达式的结果赋给属性名可以消除此错误。

  [!code-vb[VbVbalrAnonymousTypes#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#14)]

- 对多个属性的推理会产生两个或更多的同名属性。 回到前面示例中的声明，在那些声明中，不能将 `product.Name` 和 `car1.Name` 同时列为同一匿名类型的属性。 这是因为这些属性的推断标识符都是 `Name`。

  ```vb
  ' Not valid.
  ' Dim anon5 = New With {Key product.Name, Key car1.Name}
  ```

  通过为不同属性名赋值可以解决此问题。

  [!code-vb[VbVbalrAnonymousTypes#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#36)]

  请注意，更改大小写（在大写和小写字母之间切换）不能区分两个名称。

  ```vb
  Dim price = 0
  ' Not valid, because Price and price are the same name.
  ' Dim anon7 = New With {Key product.Price, Key price}
  ```

- 属性的初始类型和值取决于尚未建立的另一个属性。 例如， `.IDName = .LastName` 在匿名类型声明中无效，除非 `.LastName` 已经初始化。

  ```vb
  ' Not valid.
  ' Dim anon8 = New With {Key .IDName = .LastName, Key .LastName = "Jones"}
  ```

  在此示例中，可以通过逆转声明属性的顺序来解决问题。

  [!code-vb[VbVbalrAnonymousTypes#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#15)]

- 匿名类型的属性名和 <xref:System.Object>的成员的名称相同。 例如，因为 `Equals` 是 <xref:System.Object>的一个方法，所以下面的声明会失败。

  ```vb
  ' Not valid.
  ' Dim relationsLabels1 = New With {Key .Equals = "equals", Key .Greater = _
  '                       "greater than", Key .Less = "less than"}
  ```

  可以通过更改属性名来修复该问题：

  [!code-vb[VbVbalrAnonymousTypes#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrAnonymousTypes/VB/Class1.vb#16)]

## <a name="see-also"></a>另请参阅

- [对象初始值设定项：命名和匿名类型](object-initializers-named-and-anonymous-types.md)
- [局部类型推理](../variables/local-type-inference.md)
- [匿名类型](anonymous-types.md)
- [密钥](../../../language-reference/modifiers/key.md)
