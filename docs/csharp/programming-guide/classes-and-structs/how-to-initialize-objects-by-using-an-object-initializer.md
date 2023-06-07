---
title: 如何使用对象初始值设定项初始化对象 - C# 编程指南
description: 了解如何使用对象初始值设定项来初始化 C# 中的类型对象，而无需调用构造函数。 使用对象初始值设定项来定义匿名类型。
ms.date: 12/20/2018
helpviewer_keywords:
- object initializers [C#], how to use
- objects [C#], initializing
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: 4b75ebb2-2e29-43de-929c-d736a8f27ce6
ms.openlocfilehash: 0c2d38713a1fdefd922234a02e23518c0f00add5
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97512777"
---
# <a name="how-to-initialize-objects-by-using-an-object-initializer-c-programming-guide"></a>如何使用对象初始值设定项初始化对象（C# 编程指南）

可以使用对象初始值设定项以声明方式初始化类型对象，而无需显式调用类型的构造函数。  
  
以下示例演示如何将对象初始值设定项用于命名对象。 编译器通过首先访问无参数实例构造函数，然后处理成员初始化来处理对象初始值设定项。 因此，如果无参数构造函数在类中声明为 `private`，则需要公共访问的对象初始值设定项将失败。
  
如果要定义匿名类型，则必须使用对象初始值设定项。 有关详细信息，请参阅[如何在查询中返回元素属性的子集](how-to-return-subsets-of-element-properties-in-a-query.md)。  
  
## <a name="example"></a>示例  

下面的示例演示如何使用对象初始值设定项初始化新的 `StudentName` 类型。 此示例在 `StudentName` 类型中设置属性：
  
[!code-csharp[InitializerObjectExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/HowToObjectInitializers.cs#HowToObjectInitializers)]  

对象初始值设定项可用于在对象中设置索引器。 下面的示例定义了一个 `BaseballTeam` 类，该类使用索引器获取和设置不同位置的球员。 初始值设定项可以根据位置的缩写或每个位置的棒球记分卡的编号来分配球员：

[!code-csharp[InitializerIndexerExample](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/object-collection-initializers/HowToIndexInitializer.cs#HowToIndexInitializer)]  

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [对象和集合初始值设定项](object-and-collection-initializers.md)
