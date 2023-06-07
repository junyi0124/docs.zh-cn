---
title: 如何合并委托（多播委托）- C# 编程指南
description: 了解如何合并委托以创建多播委托。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- delegates [C#], combining
- multicast delegates [C#]
ms.assetid: 4e689450-6d0c-46de-acfd-f961018ae5dd
ms.openlocfilehash: 3e86c8ed4b7b091ee8c75930d427c22aa5bc0c52
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185946"
---
# <a name="how-to-combine-delegates-multicast-delegates-c-programming-guide"></a>如何合并委托（多播委托）（C# 编程指南）

此示例演示如何创建多播委托。 [委托](../../language-reference/builtin-types/reference-types.md)对象的一个有用属性在于可通过使用 `+` 运算符将多个对象分配到一个委托实例。 多播委托包含已分配委托列表。 此多播委托被调用时会依次调用列表中的委托。 仅可合并类型相同的委托。  
  
 `-` 运算符可用于从多播委托中删除组件委托。  
  
## <a name="example"></a>示例  

 [!code-csharp[csProgGuideDelegates#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDelegates/CS/Delegates.cs#11)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.MulticastDelegate>
- [C# 编程指南](../index.md)
- [事件](../events/index.md)
