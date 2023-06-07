---
title: 如何：创建过程
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- Visual Basic code, reusing
- procedure declarations
- procedures [Visual Basic], about procedures
ms.assetid: 4f779247-0b50-47e8-9e5c-ab5cf39ac0d2
ms.openlocfilehash: a831814c18f97991fca8067f1c9c8e491da1b665
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74344914"
---
# <a name="how-to-create-a-procedure-visual-basic"></a>如何：创建过程 (Visual Basic)

在开始声明语句（`Sub` 或 `Function`）与结束声明语句（`End Sub` 或 `End Function`）之间包含过程。 过程的所有代码都位于这些语句之间。

 一个过程不能包含另一个过程，所以其开始和结束语句必须在其他任何过程之外。

 如果你的代码在不同的位置执行相同的任务，则可以将该任务作为过程写入一次，然后从代码中的不同位置调用它。

### <a name="to-create-a-procedure-that-does-not-return-a-value"></a>创建不返回值的过程

1. 在任何其他过程之外，使用 `Sub` 语句，后跟 `End Sub` 语句。

2. 在 `Sub` 语句中，将 `Sub` 关键字跟过程的名称一起，然后将参数列表括在括号中。

3. 将过程的代码语句置于 `Sub` 和 `End Sub` 语句之间。

### <a name="to-create-a-procedure-that-returns-a-value"></a>创建返回值的过程

1. 在任何其他过程之外，使用 `Function` 语句，后跟 `End Function` 语句。

2. 在 `Function` 语句中，将 `Function` 关键字后面跟过程的名称，然后将参数列表括在括号中 `As`，然后指定返回值的数据类型。

3. 将过程的代码语句置于 `Function` 和 `End Function` 语句之间。

4. 使用 `Return` 语句将值返回到调用代码。

### <a name="to-connect-your-new-procedure-with-the-old-repetitive-blocks-of-code"></a>将新过程与旧的、重复的代码块连接起来

1. 请确保在旧代码有权访问的位置定义新过程。

2. 在旧的、重复的代码块中，将执行重复任务的语句替换为调用 `Sub` 或 `Function` 过程的单个语句。

3. 如果你的过程是返回值的 `Function`，请确保调用语句使用返回值执行操作（例如，将其存储在变量中），否则值将丢失。

## <a name="example"></a>示例

 以下 `Function` 过程将计算直角三角形的最长边（或斜边），并给出另一方的值：

 [!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]

## <a name="see-also"></a>另请参阅

- [过程](index.md)
- [Sub 过程](sub-procedures.md)
- [Function 过程](function-procedures.md)
- [属性过程](property-procedures.md)
- [运算符过程](operator-procedures.md)
- [过程参数和自变量](procedure-parameters-and-arguments.md)
- [递归过程](recursive-procedures.md)
- [过程重载](procedure-overloading.md)
- [对象和类](../objects-and-classes/index.md)
- [面向对象的编程 (Visual Basic)](../../concepts/object-oriented-programming.md)
