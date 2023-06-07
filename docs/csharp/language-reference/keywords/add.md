---
description: 了解如何使用 C# 中的 add 关键字来创建自定义事件访问器
title: add - C# 参考
ms.date: 07/20/2015
f1_keywords:
- add_CSharpKeyword
helpviewer_keywords:
- add event accessor [C#]
ms.assetid: faf30b99-10e8-45cd-ab9a-57585d4d1d8d
ms.openlocfilehash: 9daf635206ff9727a4bf333c123f68be812723cd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91168759"
---
# <a name="add-c-reference"></a>add（C# 参考）

`add` 上下文关键字用于定义一个在客户端代码订阅你的[事件](./event.md)时调用的自定义事件访问器。 如果提供自定义 `add` 访问器，还必须提供 [remove](./remove.md) 访问器。  
  
## <a name="example"></a>示例  

如下示例显示一个具有自定义 `add` 和 [remove](./remove.md) 访问器的事件。 有关完整示例，请参阅[如何实现接口事件](../../programming-guide/events/how-to-implement-interface-events.md)。
  
[!code-csharp[csrefKeywordsContextual#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#15)]
  
 通常不需要提供自己的自定义事件访问器。 大多数情况下，使用声明事件时由编译器自动生成的访问器就足够了。  
  
## <a name="see-also"></a>请参阅

- [事件](../../programming-guide/events/index.md)
