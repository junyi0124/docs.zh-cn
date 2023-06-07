---
title: 静态
ms.date: 07/20/2015
f1_keywords:
- vb.Static
helpviewer_keywords:
- static modifier
- Static keyword [Visual Basic]
ms.assetid: 19013910-4658-47b6-a22e-1744b527979e
ms.openlocfilehash: 2b7113424969b0b18c981b0c8932aeef3795ca4a
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867672"
---
# <a name="static-visual-basic"></a>Static (Visual Basic)

指定一个或多个已声明的局部变量将继续存在，并在其声明过程终止后保留其最新值。  
  
## <a name="remarks"></a>备注  

 通常，过程停止后，过程中的局部变量将立即停止存在。 静态变量将继续存在并保留其最新值。 当你的代码下一次调用该过程时，不会重新初始化该变量，并且它仍保留你分配给它的最新值。 静态变量在定义它的类或模块的生存期内继续存在。  
  
## <a name="rules"></a>规则  
  
- **声明上下文。** `Static`只能在本地变量上使用。 这意味着变量的声明上下文 `Static` 必须是过程中的过程或块，而不能是源文件、命名空间、类、结构或模块。  
  
     不能 `Static` 在结构过程内使用。  
  
- `Static`无法推断局部变量的数据类型。 有关详细信息，请参阅 [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)。  
  
- **组合修饰符。** 不能 `Static` `ReadOnly` `Shadows` `Shared` 在同一声明中同时指定、或。  
  
## <a name="behavior"></a>行为  

 在过程中声明静态变量时 `Shared` ，只有一个静态变量副本可用于整个应用程序。 `Shared`使用类名称调用过程，而不是指向类的实例的变量。  
  
 如果在不存在的过程中声明静态变量 `Shared` ，则该类的每个实例只有一个变量副本。 使用指向类的特定实例的变量调用非共享过程。  
  
## <a name="example"></a>示例  

 以下示例演示了 `Static` 的用法。  
  
 [!code-vb[VbVbalrKeywords#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class1.vb#5)]  
  
 `Static`变量 `totalSales` 仅初始化为0次。 每次输入时 `updateSales` ， `totalSales` 仍然具有为其计算的最新值。  
  
 `Static`可以在此上下文中使用修饰符：  
  
 [Dim 语句](../statements/dim-statement.md)  
  
## <a name="see-also"></a>另请参阅

- [Shadows](shadows.md)
- [共享](shared.md)
- [Visual Basic 中的生存期](../../programming-guide/language-features/declared-elements/lifetime.md)
- [变量声明](../../programming-guide/language-features/variables/variable-declaration.md)
- [结构](../../programming-guide/language-features/data-types/structures.md)
- [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)
- [对象和类](../../programming-guide/language-features/objects-and-classes/index.md)
