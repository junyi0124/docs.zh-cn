---
title: Yield 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Yield
helpviewer_keywords:
- iterators, Yield statement [Visual Basic]
- iterators [Visual Basic]
- Yield statement [Visual Basic]
ms.assetid: f33126c5-d7c4-43e2-8e36-4ae3f0703d97
ms.openlocfilehash: 783785f2a078b6ad8f975846c44ee4e716a12773
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866496"
---
# <a name="yield-statement-visual-basic"></a>Yield 语句 (Visual Basic)

将集合的下一个元素发送到 `For Each...Next` 语句。  
  
## <a name="syntax"></a>语法  
  
```vb  
Yield expression  
```  
  
## <a name="parameters"></a>参数  
  
|术语|定义|  
|---|---|  
|`expression`|必需。 可隐式转换为迭代器函数或包含该语句的访问器的类型的表达式 `Get` `Yield` 。|  
  
## <a name="remarks"></a>备注  

 `Yield`语句一次返回集合中的一个元素。 `Yield`语句包含在迭代器函数或 `Get` 访问器中，后者对集合执行自定义迭代。  
  
 使用 For Each ... 的方法使用迭代器函数 [Next 语句](for-each-next-statement.md) 或 LINQ 查询。 循环的每次迭代都会 `For Each` 调用迭代器函数。 当 `Yield` 在迭代器函数中到达语句时， `expression` 将返回，并且保留代码中的当前位置。 下次调用迭代器函数时，将从该位置重新开始执行。  
  
 隐式转换必须从语句中的类型 `expression` `Yield` 到迭代器的返回类型存在。  
  
 您可以使用 `Exit Function` 或 `Return` 语句结束迭代。  
  
 "Yield" 不是保留字，只在 `Iterator` 函数或访问器中使用时具有特殊意义 `Get` 。  
  
 有关迭代器函数和访问器的详细信息 `Get` ，请参阅 [迭代](../../programming-guide/concepts/iterators.md)器。  
  
## <a name="iterator-functions-and-get-accessors"></a>迭代器函数和 Get 访问器  

 迭代器函数或 `Get` 访问器的声明必须满足以下要求：  
  
- 它必须包含 [迭代器](../modifiers/iterator.md) 修饰符。  
  
- 返回类型必须为 <xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator> 或 <xref:System.Collections.Generic.IEnumerator%601>。  
  
- 它不能具有任何 `ByRef` 参数。  
  
 迭代器函数不能出现在事件、实例构造函数、静态构造函数或静态析构函数中。  
  
 迭代器函数可以是匿名函数。 有关更多信息，请参见 [迭代器](../../programming-guide/concepts/iterators.md)。  
  
## <a name="exception-handling"></a>异常处理  

 `Yield`语句可以在 `Try` [Try ... 的块内Catch .。。Finally 语句](try-catch-finally-statement.md)。 `Try`带有语句的块 `Yield` 可以有 `Catch` 块，并且可以有 `Finally` 块。  
  
 `Yield`语句不能位于 `Catch` 块或 `Finally` 块中。  
  
 如果在 `For Each` 迭代器函数外 (主体) 引发异常，则 `Catch` 不会执行迭代器函数中的块，但 `Finally` 会执行迭代器函数中的块。 `Catch`Iterator 函数内的块仅捕获 iterator 函数内发生的异常。  
  
## <a name="technical-implementation"></a>技术实现  

 下面的代码 `IEnumerable (Of String)` 从迭代器函数返回，然后循环访问的元素 `IEnumerable (Of String)` 。  
  
```vb  
Dim elements As IEnumerable(Of String) = MyIteratorFunction()  
    …  
For Each element As String In elements  
Next  
```  
  
 对的调用 `MyIteratorFunction` 不会执行函数的主体。 相反，该调用会将 `IEnumerable(Of String)` 返回到 `elements` 变量中。  
  
 在 `For Each` 循环迭代时，将为 <xref:System.Collections.IEnumerator.MoveNext%2A> 调用 `elements` 方法。 此调用将执行 `MyIteratorFunction` 的主体，直至到达下一个 `Yield` 语句。 `Yield`语句返回的表达式不仅确定了循环体的使用的变量值 `element` ，还确定了元素的 <xref:System.Collections.Generic.IEnumerator%601.Current%2A> 属性，这是 `IEnumerable (Of String)` 。  
  
 在 `For Each` 循环的每个后续迭代中，迭代器主体的执行将从它暂停的位置继续，直至到达 `Yield` 语句后才会停止。 `For Each`当达到 iterator 函数或或语句的末尾时，循环 `Return` `Exit Function` 就会完成。  
  
## <a name="example"></a>示例  

 下面的示例包含一个 `Yield` 位于 [For .。。下一个](for-next-statement.md) 循环。 中 [每](for-each-next-statement.md) 个语句体的每次迭代 `Main` 都会创建对 `Power` 迭代器函数的调用。 对迭代器函数的每个调用将继续到 `Yield` 语句的下一次执行（在 `For…Next` 循环的下一次迭代期间发生）。  
  
 迭代器方法的返回类型是 <xref:System.Collections.Generic.IEnumerable%601> ，迭代器接口类型。 当调用迭代器方法时，它将返回一个包含数字幂的可枚举对象。  
  
 [!code-vb[VbVbalrStatements#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#98)]  
  
## <a name="example"></a>示例  

 下面的示例演示一个作为迭代器的 `Get` 访问器。 属性声明包含 `Iterator` 修饰符。  
  
 [!code-vb[VbVbalrStatements#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#99)]  
  
 有关其他示例，请参阅 [迭代](../../programming-guide/concepts/iterators.md)器。  
  
## <a name="see-also"></a>请参阅

- [语句](index.md)
