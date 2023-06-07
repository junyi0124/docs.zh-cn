---
title: 带有命名方法的委托与匿名方法 - C# 编程指南
description: 了解通过命名或匿名方法进行的委托。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], with named vs. anonymous methods
- methods [C#], in delegates
ms.assetid: 98fa8c61-66b6-4146-986c-3236c4045733
ms.openlocfilehash: d8d2c6d8c7792b81f2fc2e25d0836e3780e58e52
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202495"
---
# <a name="delegates-with-named-vs-anonymous-methods-c-programming-guide"></a>带有命名方法的委托与匿名方法（C# 编程指南）

[委托](../../language-reference/builtin-types/reference-types.md)可以与命名方法相关联。 使用命名方法实例化委托时，该方法作为参数传递，例如：  
  
 [!code-csharp[csProgGuideDelegates#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#1)]  
  
 这称为使用命名方法。 使用命名方法构造的委托可以封装[静态](../../language-reference/keywords/static.md)方法或实例方法。 命名方法是在早期版本的 C# 中实例化委托的唯一方式。 但是，如果创建新方法会造成多余开销，C# 允许你实例化委托并立即指定调用委托时委托将处理的代码块。 代码块可包含 Lambda 表达式或匿名方法。 有关详细信息，请参阅[匿名函数](../statements-expressions-operators/anonymous-functions.md)。  
  
## <a name="remarks"></a>备注  

 作为委托参数传递的方法必须具有与委托声明相同的签名。  
  
 委托实例可以封装静态方法或实例方法。  
  
 尽管委托可以使用 [out](../../language-reference/keywords/out-parameter-modifier.md) 参数，但不建议将该委托与多播事件委托配合使用，因为你无法知道将调用哪个委托。  
  
## <a name="example-1"></a>示例 1  

 以下是声明和使用委托的简单示例。 请注意，委托 `Del` 与关联的方法 `MultiplyNumbers` 具有相同的签名  
  
 [!code-csharp[csProgGuideDelegates#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#2)]  
  
## <a name="example-2"></a>示例 2  

 在下面的示例中，一个委托映射到静态方法和实例方法，并返回来自两种方法的具体信息。  
  
 [!code-csharp[csProgGuideDelegates#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#3)]  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [委托](./index.md)
- [如何合并委托（多播委托）](./how-to-combine-delegates-multicast-delegates.md)
- [事件](../events/index.md)
