---
title: New 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.new
- vb.NewConstraint
helpviewer_keywords:
- constraints, Visual Basic generic types
- generics [Visual Basic], constraints
- constraints, New keyword [Visual Basic]
- New constraint
- New keyword [Visual Basic]
ms.assetid: d7d566d7-fe0e-4336-91f7-641a542de4d0
ms.openlocfilehash: 27b5b4516ef729045036c36fedc24b6c576a4f61
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348317"
---
# <a name="new-operator-visual-basic"></a>New 运算符 (Visual Basic)

引入 `New` 子句来创建新的对象实例，指定类型参数上的构造函数约束，或将 `Sub` 过程标识为类构造函数。

## <a name="remarks"></a>备注

在声明或赋值语句中，`New` 子句必须指定一个已定义的类，可从中创建实例。 这意味着，该类必须公开调用代码可以访问的一个或多个构造函数。

可在声明语句或赋值语句中使用 `New` 子句。 当语句运行时，它将调用指定类的适当构造函数，同时传递您提供的任何自变量。 下面的示例通过创建一个 `Customer` 类的实例，该实例具有两个构造函数，一个不采用参数，另一个采用字符串参数，如下所示：

[!code-vb[VbVbalrKeywords#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class6.vb#11)]

由于数组是类，因此 `New` 可以创建一个新的数组实例，如以下示例中所示：

[!code-vb[VbVbalrKeywords#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class6.vb#12)]

如果没有足够的内存来创建新的实例，则公共语言运行时（CLR）会引发 <xref:System.OutOfMemoryException> 错误。

> [!NOTE]
> 还可以在类型参数列表中使用 `New` 关键字，以指定提供的类型必须公开可访问的无参数构造函数。 有关类型参数和约束的详细信息，请参阅[Type List](../statements/type-list.md)。

若要为类创建构造函数过程，请将 `Sub` 过程的名称设置为 `New` 关键字。 有关详细信息，请参阅[对象生存期：如何创建和销毁对象](../../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)。

`New` 关键字可用于以下上下文中：

- [Dim 语句](../statements/dim-statement.md)
- [Of](../statements/of-clause.md)
- [Sub 语句](../statements/sub-statement.md)

## <a name="see-also"></a>另请参阅

- <xref:System.OutOfMemoryException>
- [关键字](../keywords/index.md)
- [类型列表](../statements/type-list.md)
- [Visual Basic 中的泛型类型](../../programming-guide/language-features/data-types/generic-types.md)
- [对象生存期：如何创建和销毁对象](../../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
