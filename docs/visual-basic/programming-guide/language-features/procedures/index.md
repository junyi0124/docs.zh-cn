---
title: 过程
ms.date: 04/28/2017
helpviewer_keywords:
- procedures [Visual Basic], structured code
- Visual Basic code, procedures
- procedures [Visual Basic], types of
- structured code [Visual Basic], procedures
- procedures
ms.assetid: 9effbcf0-80a0-4d1a-98f4-2c6920592766
ms.openlocfilehash: 926d2dcc7f29102457d5ed9632e7455f8f0c7b96
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071321"
---
# <a name="procedures-in-visual-basic"></a>Visual Basic 中的过程

*过程*是由声明语句括起来的 Visual Basic 语句块 `Function` ， (、 `Sub` 、 `Operator` 、 `Get` 、 `Set`) 和匹配 `End` 声明。 Visual Basic 中的所有可执行语句都必须在某一过程中。  
  
## <a name="calling-a-procedure"></a>调用过程  

 从代码中的其他位置调用过程。 这称为过程调用**。 过程运行完毕后，会将控件返回到调用它的代码，称为调用代码**。 调用代码是一个语句或语句中的一个表达式，它通过名称指定过程并将控件转移给该过程。  
  
## <a name="returning-from-a-procedure"></a>从过程中返回  

 过程运行完毕后，会将控件返回给调用代码。 为此，它可以使用 [Return 语句](../../../language-reference/statements/return-statement.md)、该过程的适当 [Exit 语句](../../../language-reference/statements/exit-statement.md) 语句或过程的 [End \<keyword> 语句](../../../language-reference/statements/end-keyword-statement.md) 语句。 然后控件在过程调用之后传递给调用代码。  
  
- 使用 `Return` 语句，控件将立即返回到调用代码。 `Return` 语句之后的语句不会运行。 在同一过程中可拥有多个 `Return` 语句。  
  
- 使用 `Exit Sub` 或 `Exit Function` 语句，控件立即返回到调用代码。 `Exit` 语句之后的语句不会运行。 在同一过程中可拥有多个 `Exit` 语句，也可混合 `Return` 和 `Exit` 语句。  
  
- 如果一个过程没有 `Return` 或 `Exit` 语句，则在过程主体的最后一个语句之后以 `End Sub` 或 `End Function`、`End Get` 或 `End Set` 语句结尾。 `End` 语句立即将控件返回到调用代码。 一个过程中只能有一个 `End` 语句。  
  
## <a name="parameters-and-arguments"></a>形参和实参  

 在大多数情况下，每次调用过程时，过程都需对不同数据进行操作。 可将此信息作为过程调用的一部分传递给该过程。 过程定义零个或多个形参**，每个形参表示一个该过程希望你传递给它的值。 过程调用中，与过程定义中每个形参相对应的是的实参**。 实参表示给定过程调用中传递给相应形参的值。  
  
## <a name="types-of-procedures"></a>过程类型  

 Visual Basic 使用几种类型的过程：  
  
- [Sub 过程](./sub-procedures.md)执行操作，但不向调用代码返回值。  
  
- 事件处理过程是为响应由用户操作所引发的事件或由程序中的发生所引发的事件而执行的 `Sub` 过程。  
  
- [Function 过程](./function-procedures.md)向调用代码返回值。 其可在返回前执行其他操作。

    某些用 C# 编写的函数会返回引用返回值**。 函数调用方可修改返回值，这种修改反映在被调用对象的状态中。 从 Visual Basic 2017 开始，Visual Basic 代码可以使用引用返回值，但不能返回引用的值。 有关详细信息，请参阅[引用返回值](ref-return-values.md)。
  
- [属性过程](./property-procedures.md)返回并分配对象或模块上的属性值。  
  
- 如果一个或两个操作数是新定义的类或结构，则[运算符过程](./operator-procedures.md)定义标准运算符的行为。  
  
- [Visual Basic 中的泛型过程](../data-types/generic-procedures.md)除定义其正常参数外，还定义一个或多个类型参数**，因此调用代码可在每次调用时传递特定的数据类型。  
  
## <a name="procedures-and-structured-code"></a>过程和结构化代码  

 应用程序中的每行可执行代码都必须位于某个过程内，例如 `Main`、`calculate` 或 `Button1_Click`。 如果将较大的过程细分为较小的过程，则应用程序将更易读取。  
  
 过程对于执行重复或共享任务（如常用的计算、文本和控制处理以及数据库操作）非常有用。 可从代码中的众多不同位置调用过程，因此可将过程用作应用程序的构建基块。  
  
 使用过程来构建代码具有以下好处：  
  
- 过程允许将程序分解成离散的逻辑单元。 调试独立的单元比在没有过程时调试整个程序更容易。  
  
- 开发可供某一程序使用的过程后，也可在其他程序中使用它们，通常只需很少修改或无需修改。 这有助于避免代码重复。  
  
## <a name="see-also"></a>请参阅

- [如何：创建过程](./how-to-create-a-procedure.md)
- [Sub 过程](./sub-procedures.md)
- [Function 过程](./function-procedures.md)
- [Property 过程](./property-procedures.md)
- [运算符过程](./operator-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [递归过程](./recursive-procedures.md)
- [过程重载](./procedure-overloading.md)
- [Generic Procedures in Visual Basic](../data-types/generic-procedures.md)
- [对象和类](../objects-and-classes/index.md)
