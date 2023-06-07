---
title: Sub 语句
ms.date: 05/12/2018
f1_keywords:
- vb.Sub
helpviewer_keywords:
- Public keyword [Visual Basic], Sub statements
- procedures [Visual Basic], creating
- declaring procedures [Visual Basic], Sub statement
- arguments [Visual Basic], Sub procedures
- As keyword [Visual Basic], Sub statements
- Optional keyword [Visual Basic], Sub statements
- declarations [Visual Basic], procedures
- Sub keyword [Visual Basic]
- Handles keyword [Visual Basic], Sub statements
- Protected Friend keyword [Visual Basic]
- ParamArray keyword [Visual Basic], Sub statements
- Implements keyword [Visual Basic], Sub statements
- Sub statement [Visual Basic]
- subroutines
- ByRef keyword [Visual Basic], Sub statements
- Sub procedures [Visual Basic], Sub statement
- recursive procedures
- Private keyword [Visual Basic], Sub statements
- Friend keyword [Visual Basic], Sub statements
- Exit statement [Visual Basic], Sub statements
- procedures [Visual Basic], Sub
- End keyword [Visual Basic], Sub statements
- ByVal keyword [Visual Basic], Sub statements
- Visual Basic code, Sub procedures
ms.assetid: e347d700-d06c-405b-b302-e9b1edb57dfc
ms.openlocfilehash: e50b79c31c92ac116d6c82bcececba3340894d74
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404169"
---
# <a name="sub-statement-visual-basic"></a>Sub 语句 (Visual Basic)

声明定义过程的名称、参数和代码 `Sub` 。

## <a name="syntax"></a>语法

```vb
[ <attributelist> ] [ Partial ] [ accessmodifier ] [ proceduremodifiers ] [ Shared ] [ Shadows ] [ Async ]
Sub name [ (Of typeparamlist) ] [ (parameterlist) ] [ Implements implementslist | Handles eventlist ]
    [ statements ]
    [ Exit Sub ]
    [ statements ]
End Sub
```

## <a name="parts"></a>组成部分

- `attributelist`

  可选。 请参阅[特性列表](attribute-list.md)。

- `Partial`

  可选。 指示分部方法的定义。 请参阅[分部方法](../../programming-guide/language-features/procedures/partial-methods.md)。

- `accessmodifier`

  可选。 可以是以下其中一个值：

  - [公共](../modifiers/public.md)

  - [避免](../modifiers/protected.md)

  - [友好](../modifiers/friend.md)

  - 专用 

  - [Protected Friend](../modifiers/protected-friend.md)

  - [私有受保护](../modifiers/private-protected.md)

  请参阅[Visual Basic 中的访问级别](../../programming-guide/language-features/declared-elements/access-levels.md)。

- `proceduremodifiers`

  可选。 可以是以下其中一个值：

  - [重载](../modifiers/overloads.md)

  - [替代](../modifiers/overrides.md)

  - [Overrides](../modifiers/overridable.md)

  - [NotOverridable](../modifiers/notoverridable.md)

  - [New](../modifiers/mustoverride.md)

  - `MustOverride Overrides`

  - `NotOverridable Overrides`

- `Shared`

  可选。 请参阅[共享](../modifiers/shared.md)。

- `Shadows`

  可选。 请参阅[阴影](../modifiers/shadows.md)。

- `Async`

  可选。 请参阅[Async](../modifiers/async.md)。

- `name`

  必需。 过程的名称。 请参阅 [Declared Element Names](../../programming-guide/language-features/declared-elements/declared-element-names.md)。 若要为类创建构造函数过程，请将过程的名称设置 `Sub` 为 `New` 关键字。 有关详细信息，请参阅[对象生存期：如何创建和销毁对象](../../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)。

- `typeparamlist`

  可选。 泛型过程的类型参数的列表。 请参阅[类型列表](type-list.md)。

- `parameterlist`

  可选。 表示此过程参数的本地变量名称列表。 请参阅[参数列表](parameter-list.md)。

- `Implements`

  可选。 指示此过程实现了一个或多个 `Sub` 过程，每个过程都在此过程的包含类或结构实现的接口中定义。 请参阅[Implements 语句](implements-statement.md)。

- `implementslist`

  如果提供 `Implements`，则是必需的。 所实现的 `Sub` 过程的列表。

  `implementedprocedure [ , implementedprocedure ... ]`

  每个 `implementedprocedure` 都具有以下语法和部件：

  `interface.definedname`

  |组成部分|说明|
  |---|---|
  |`interface`|必需。 此过程的包含类或结构实现的接口的名称。|
  |`definedname`|必需。 在 `interface` 中用于定义过程的名称。|

- `Handles`

  可选。 指示此过程可以处理一个或多个特定事件。 请参阅[句柄](handles-clause.md)。

- `eventlist`

  如果提供 `Handles`，则是必需的。 此过程处理的事件列表。

  `eventspecifier [ , eventspecifier ... ]`

  每个 `eventspecifier` 都具有以下语法和部件：

  `eventvariable.event`

  |组成部分|说明|
  |---|---|
  |`eventvariable`|必需。 用引发事件的类或结构的数据类型声明的对象变量。|
  |`event`|必需。 此过程处理的事件的名称。|

- `statements`

  可选。 要在此过程中运行的语句块。

- `End Sub`

  终止此过程的定义。

## <a name="remarks"></a>备注

所有可执行代码都必须在过程内。 `Sub`如果不想将值返回到调用代码，请使用过程。 `Function`如果要返回值，请使用过程。

## <a name="defining-a-sub-procedure"></a>定义 Sub 过程

只能 `Sub` 在模块级别定义过程。 因此，sub 过程的声明上下文必须是类、结构、模块或接口，不能是源文件、命名空间、过程或块。 有关详细信息，请参阅[声明上下文和默认访问级别](declaration-contexts-and-default-access-levels.md)。

`Sub`过程默认为公共访问。 您可以使用访问修饰符调整其访问级别。

如果过程使用 `Implements` 关键字，则包含类或结构必须具有 `Implements` 紧跟在其或语句后面的语句 `Class` `Structure` 。 `Implements`语句必须包括在中指定的每个接口 `implementslist` 。 但是，接口用于定义 `Sub` （在中）的名称不必 `definedname` 与此过程的名称匹配（在中为 `name` ）。

## <a name="returning-from-a-sub-procedure"></a>从 Sub 过程返回

当 `Sub` 过程返回到调用代码时，执行将继续执行调用它的语句之后的语句。

下面的示例演示如何从过程返回 `Sub` 。

```vb
Sub mySub(ByVal q As String)
    Return
End Sub
```

`Exit Sub`和 `Return` 语句导致直接从 `Sub` 过程退出。 任意数量的 `Exit Sub` 和 `Return` 语句可以出现在过程中的任何位置，并且可以混合使用 `Exit Sub` 和 `Return` 语句。

## <a name="calling-a-sub-procedure"></a>调用 Sub 过程

`Sub`通过在语句中使用过程名称，然后将该名称跟在括号中的参数列表后面来调用过程。 仅当未提供任何参数时，才可以省略括号。 但是，如果你始终包含括号，你的代码将更具可读性。

`Sub`过程和 `Function` 过程可以具有参数并执行一系列语句。 但 `Function` 过程返回值，并且 `Sub` 过程不会。 因此，不能 `Sub` 在表达式中使用过程。

您可以在 `Call` 调用过程时使用关键字 `Sub` ，但对于大多数用途，不建议使用该关键字。 有关详细信息，请参阅[Call 语句](call-statement.md)。

Visual Basic 有时会重新排列算术表达式以提高内部效率。 出于此原因，如果参数列表包含调用其他过程的表达式，则不应假定将按特定顺序调用这些表达式。

## <a name="async-sub-procedures"></a>Async Sub 过程

通过使用异步功能，你可以调用异步函数而无需使用显式回调或在多个函数或 lambda 表达式中手动拆分你的代码。

如果使用[Async](../modifiers/async.md)修饰符标记过程，则可以在过程中使用[Await](../operators/await-operator.md)运算符。 当控件 `Await` 在过程中到达表达式时 `Async` ，控件将返回到调用方，并且在等待的任务完成之前，会挂起过程中的进度。 任务完成后，可以在过程中继续执行。

> [!NOTE]
> `Async`如果遇到了第一个尚未完成的对象或过程结束时 `Async` （以先发生者为准），则过程返回到调用方。

还可以使用修饰符标记[函数语句](function-statement.md) `Async` 。 `Async`函数的返回类型可以是 <xref:System.Threading.Tasks.Task%601> 或 <xref:System.Threading.Tasks.Task> 。 本主题后面的示例演示了 `Async` 返回类型为的函数 <xref:System.Threading.Tasks.Task%601> 。

`Async``Sub`过程主要用于事件处理程序，其中不能返回值。 `Async` `Sub` 无法等待过程，并且过程的调用方 `Async` `Sub` 无法捕获该 `Sub` 过程引发的异常。

`Async`过程不能声明任何[ByRef](../modifiers/byref.md)参数。

有关过程的详细信息 `Async` ，请参阅[采用 Async 和 Await 的异步编程](../../programming-guide/concepts/async/index.md)、[异步程序中的控制流](../../programming-guide/concepts/async/control-flow-in-async-programs.md)和[异步返回类型](../../programming-guide/concepts/async/async-return-types.md)。

## <a name="example"></a>示例

下面的示例使用 `Sub` 语句来定义构成过程正文的名称、参数和代码 `Sub` 。

[!code-vb[VbVbalrStatements#58](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#58)]

## <a name="example"></a>示例

在下面的示例中， `DelayAsync` 是 `Async` `Function` 具有返回类型的 <xref:System.Threading.Tasks.Task%601> 。 `DelayAsync` 具有返回整数的 `Return` 语句。 因此，的函数声明 `DelayAsync` 必须具有返回类型 `Task(Of Integer)` 。 由于返回类型是 `Task(Of Integer)` ，中表达式的计算会 `Await` `DoSomethingAsync` 生成一个整数，如以下语句所示： `Dim result As Integer = Await delayTask` 。

此 `startButton_Click` 过程是过程的示例 `Async Sub` 。 由于 `DoSomethingAsync` 是一个 `Async` 函数，因此对的调用的任务 `DoSomethingAsync` 必须等待，如以下语句所示： `Await DoSomethingAsync()` 。 此 `startButton_Click` `Sub` 过程必须使用修饰符进行定义， `Async` 因为它具有 `Await` 表达式。

[!code-vb[csAsyncMethod#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csasyncmethod/vb/mainwindow.xaml.vb#1)]

## <a name="see-also"></a>另请参阅

- [Implements 语句](implements-statement.md)
- [Function 语句](function-statement.md)
- [参数列表](parameter-list.md)
- [Dim 语句](dim-statement.md)
- [Call 语句](call-statement.md)
- [个](of-clause.md)
- [参数数组](../../programming-guide/language-features/procedures/parameter-arrays.md)
- [如何：使用泛型类](../../programming-guide/language-features/data-types/how-to-use-a-generic-class.md)
- [过程疑难解答](../../programming-guide/language-features/procedures/troubleshooting-procedures.md)
- [分部方法](../../programming-guide/language-features/procedures/partial-methods.md)
