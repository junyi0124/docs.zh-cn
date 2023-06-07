---
title: For...Next 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Step
- vb.Next
- vb.To
- vb.for
helpviewer_keywords:
- infinite loops
- Next keyword [Visual Basic], For...Next statements
- For keyword [Visual Basic], For...Next statements
- Step keyword [Visual Basic], For...Next statements
- operator overloading, For...Next statement
- To keyword [Visual Basic], For...Next statements
- endless loops
- loops, endless
- instructions, repeating
- Next statement [Visual Basic], For...Next
- For...Next statements
- loop structures [Visual Basic], For...Next
- loops, infinite
- Exit statement [Visual Basic], For...Next statements
- For statement [Visual Basic]
ms.assetid: f5fc0d51-67ce-4c36-9f09-31c9a91c94e9
ms.openlocfilehash: 6896c6cfb4ec5d6207011e56b72639c459120e53
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404636"
---
# <a name="fornext-statement-visual-basic"></a>For...Next 语句 (Visual Basic)

按指定次数重复一组语句。

## <a name="syntax"></a>语法

```vb
For counter [ As datatype ] = start To end [ Step step ]
    [ statements ]
    [ Continue For ]
    [ statements ]
    [ Exit For ]
    [ statements ]
Next [ counter ]
```

## <a name="parts"></a>组成部分

|组成部分|说明|
|----------|-----------------|
|`counter`|语句中必需 `For` 。 数值变量。 循环的控制变量。 有关详细信息，请参阅本主题后面的[计数器参数](#BKMK_Counter)。|
|`datatype`|可选。 的数据类型 `counter` 。 有关详细信息，请参阅本主题后面的[计数器参数](#BKMK_Counter)。|
|`start`|必需。 数值表达式。 `counter` 的初始值。|
|`end`|必需。 数值表达式。 的最终值 `counter` 。|
|`step`|可选。 数值表达式。 `counter`每次通过循环增加的量。|
|`statements`|可选。 `For`与之间运行指定次数的一个或多个语句 `Next` 。|
|`Continue For`|可选。 将控制转移到下一个循环迭代。|
|`Exit For`|可选。 将控制转移到 `For` 循环外。|
|`Next`|必需。 终止循环的定义 `For` 。|

> [!NOTE]
> `To`此语句使用关键字来指定计数器的范围。 你还可以在[Select .。。Case 语句](select-case-statement.md)和数组声明。 有关数组声明的详细信息，请参阅[Dim 语句](dim-statement.md)。

## <a name="simple-examples"></a> 简单示例

`For` `Next` 如果要对一组语句重复一组次数，请使用 ... 结构。

在下面的示例中， `index` 变量的值为1，并与循环的每次迭代递增，在的值达到5之后结束 `index` 。

[!code-vb[VbVbalrStatements#111](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#111)]

在下面的示例中， `number` 变量从2开始，在循环的每次迭代后减0.25，截止值为 `number` 0。 的 `Step` 自变量会 `-.25` 在循环的每次迭代时减少值0.25。

[!code-vb[VbVbalrStatements#112](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#112)]

> [!TIP]
> ... [End While 语句](while-end-while-statement.md)或[Do .。。](do-loop-statement.md)当您事先不知道在循环中运行语句的次数时，Loop 语句可以正常工作。 但是，当你想要运行循环特定次数时，" `For` ..." `Next` 循环是更好的选择。 您在第一次进入循环时确定迭代次数。

## <a name="nesting-loops"></a>嵌套循环

可以 `For` 通过在另一个循环中放置循环来嵌套循环。 下面的示例演示 `For` `Next` 具有不同步长值的嵌套 ... 结构。 外部循环为循环的每次迭代创建字符串。 内部循环为循环的每次迭代递减循环计数器变量。

[!code-vb[VbVbalrStatements#113](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#113)]

嵌套循环时，每个循环必须具有唯一 `counter` 变量。

你还可以在彼此之间嵌套不同种类的控制结构。 有关详细信息，请参阅[嵌套控制结构](../../programming-guide/language-features/control-flow/nested-control-structures.md)。

## <a name="exit-for-and-continue-for"></a>退出并继续进行

`Exit For`语句立即退出 `For` .。。`Next` 循环并将控制转移到语句后面的语句 `Next` 。

`Continue For`语句将控制立即传输到循环的下一次迭代。 有关详细信息，请参阅[Continue 语句](continue-statement.md)。

下面的示例演示如何使用 `Continue For` 和 `Exit For` 语句。

[!code-vb[VbVbalrStatements#115](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#115)]

可以将任意数量的语句放入 `Exit For` `For` .。。`Next` 圈. 当在嵌套 `For` .。。`Next` 循环， `Exit For` 退出最内层的循环，并将控制转移到下一个更高的嵌套级别。

`Exit For`在你计算某个条件（例如，在 `If` ... `Then`...`Else`结构）。 你可能想要将 `Exit For` 用于以下情况：

- 不需要继续循环访问。 错误的值或终止请求可能会创建此条件。

- `Try`... `Catch`...`Finally`语句捕获异常。 可以 `Exit For` 在块的末尾使用 `Finally` 。

- 您有一个无限循环，该循环是一种循环，它可以运行很大甚至无限次。 如果检测到这种情况，则可以使用 `Exit For` 来转义循环。 有关详细信息，请参阅[Do .。。循环语句](do-loop-statement.md)。

## <a name="technical-implementation"></a>技术实现

当 `For` ... `Next` 循环启动时，Visual Basic 将计算 `start` 、 `end` 和 `step` 。 Visual Basic 此时仅计算这些值，然后将赋值 `start` 给 `counter` 。 在语句块运行之前，Visual Basic 与进行比较 `counter` `end` 。 如果已大于 `counter` `end` 值（如果为负，则为较小的值 `step` ）， `For` 循环将结束并控制传递到语句后面的语句 `Next` 。 否则，语句块将运行。

每次 Visual Basic 遇到 `Next` 语句时，它会 `counter` 递增 `step` 并返回到 `For` 语句。 同样，它与进行比较 `counter` `end` ，并再次运行该块或退出循环，具体取决于结果。 此过程将一直 `counter` 继续 `end` ，直到 `Exit For` 遇到传递或语句。

直到经过传递后循环才会停止 `counter` `end` 。 如果 `counter` 等于 `end` ，则循环将继续。 确定是否运行块的比较是否为 `counter`  <=  `end` `step` 正且为 `counter`  >=  `end` `step` 负。

如果在循环内更改的值 `counter` ，则可能会更难读取和调试代码。 更改 `start` 、或的值 `end` `step` 不会影响在第一次输入循环时确定的迭代值。

如果嵌套循环，编译器在 `Next` 内部级别的语句之前遇到外部嵌套级别的语句时，会发出错误信号 `Next` 。 但是，只有在 `counter` 每个语句中指定时，编译器才能检测到此重叠错误 `Next` 。

### <a name="step-argument"></a>步骤参数

的值 `step` 可以是正数也可以是负数。 此参数根据下表确定循环处理：

|**步骤值**|**如果**|
|--------------------|--------------------------|
|正或零|`counter` <= `end`|
|负|`counter` >= `end`|

`step` 的默认值为 1。

### <a name="counter-argument"></a><a name="BKMK_Counter"></a>计数器参数

下表指示是否 `counter` 定义了作用域为整个循环的新局部变量 `For…Next` 。 此确定取决于是否 `datatype` 存在以及是否 `counter` 已定义。

|是否 `datatype` 存在？|已 `counter` 定义？|Result （是否 `counter` 定义一个作用域为整个循环的新局部变量 `For...Next` ）|
|----------------------------|-----------------------------------|-------------------------------------------------------------------------------------------------------------|
|否|是|不是，因为已 `counter` 定义。 如果该过程的作用域 `counter` 不是本地的，则会发生编译时警告。|
|否|否|是。 数据类型是从 `start` 、和表达式中推断出来的 `end` `step` 。 有关类型推理的信息，请参阅[选项推断语句](option-infer-statement.md)和[局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)。|
|是|是|是，但仅当现有 `counter` 变量在过程外定义时才存在。 该变量保持独立。 如果现有变量的作用域 `counter` 是过程的本地值，则会发生编译时错误。|
|是|否|是。|

的数据类型 `counter` 确定迭代的类型，该类型必须是以下类型之一：

- `Byte`、、 `SByte` 、 `UShort` `Short` 、 `UInteger` 、、、、、 `Integer` `ULong` `Long` `Decimal` `Single` 或 `Double` 。

- 使用[Enum 语句](enum-statement.md)声明的枚举。

- 一个`Object`。

- `T`具有以下运算符的类型，其中 `B` 是可在表达式中使用的类型 `Boolean` 。

  `Public Shared Operator >= (op1 As T, op2 As T) As B`

  `Public Shared Operator <= (op1 As T, op2 As T) As B`

  `Public Shared Operator - (op1 As T, op2 As T) As T`

  `Public Shared Operator + (op1 As T, op2 As T) As T`

您可以选择 `counter` 在语句中指定变量 `Next` 。 此语法可提高程序的可读性，尤其是在具有嵌套循环的情况下 `For` 。 您必须指定出现在相应语句中的变量 `For` 。

`start`、 `end` 和表达式的 `step` 计算结果可以是扩大到类型的任何数据类型 `counter` 。 如果对使用用户定义的类型 `counter` ，则可能需要定义 `CType` 转换运算符，以将、或的类型转换 `start` `end` `step` 为类型 `counter` 。

## <a name="example"></a>示例

下面的示例从泛型列表中移除所有元素。 而不是[为每个 .。。下一条语句](for-each-next-statement.md)中，该示例显示了一个 `For` `Next` "..." 语句，该语句将按降序循环访问。 该示例使用此方法，因为 `removeAt` 方法会使删除的元素之后的元素具有更低的索引值。

[!code-vb[VbVbalrStatements#114](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#114)]

## <a name="example"></a>示例

下面的示例循环访问通过使用[Enum 语句](enum-statement.md)声明的枚举。

[!code-vb[VbVbalrStatements#116](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#116)]

## <a name="example"></a>示例

在下面的示例中，语句参数使用具有 `+` 、、 `-` `>=` 和运算符的运算符重载的类 `<=` 。

[!code-vb[VbVbalrStatements#117](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class7.vb#117)]

## <a name="see-also"></a>另请参阅

- <xref:System.Collections.Generic.List%601>
- [循环结构](../../programming-guide/language-features/control-flow/loop-structures.md)
- [While...End While 语句](while-end-while-statement.md)
- [Do...Loop 语句](do-loop-statement.md)
- [嵌套的控件结构](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Exit 语句](exit-statement.md)
- [集合](../../programming-guide/concepts/collections.md)
