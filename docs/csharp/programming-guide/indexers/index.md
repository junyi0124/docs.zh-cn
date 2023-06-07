---
title: 索引器 - C# 编程指南
description: C# 中的索引器支持类或结构实例像数组一样编制索引。 可以设置或获取索引值，而无需指定类型或实例成员。
ms.date: 03/10/2017
f1_keywords:
- cs.indexers
helpviewer_keywords:
- indexers [C#]
- C# language, indexers
ms.assetid: 022cd27d-d5e0-4cfe-8b97-dc018cc3355d
ms.openlocfilehash: ea95eef7bb9ba232e4d59e3f833b82e98398fc33
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89495299"
---
# <a name="indexers-c-programming-guide"></a>索引器（C# 编程指南）

索引器允许类或结构的实例就像数组一样进行索引。 无需显式指定类型或实例成员，即可设置或检索索引值。 索引器类似于[属性](../classes-and-structs/properties.md)，不同之处在于它们的访问器需要使用参数。  

 以下示例定义了一个泛型类，其中包含用于赋值和检索值的简单 [get](../../language-reference/keywords/get.md) 和 [set](../../language-reference/keywords/set.md) 访问器方法。 `Program` 类创建了此类的一个实例，用于存储字符串。  
  
 [!code-csharp[indexers#1](../../../../samples/snippets/csharp/programming-guide/indexers/indexer-1.cs)]  
  
> [!NOTE]
> 有关更多示例，请参阅[相关部分](./index.md#BKMK_RelatedSections)。  
  
## <a name="expression-body-definitions"></a>表达式主体定义  

索引器的 get 或 set 访问器包含一个用于返回或设置值的语句很常见。 为了支持这种情况，表达式主体成员提供了一种经过简化的语法。 自 C# 6 起，可以表达式主体成员的形式实现只读索引器，如以下示例所示。

[!code-csharp[indexers#2](../../../../samples/snippets/csharp/programming-guide/indexers/indexer-2.cs)]  

请注意，`=>` 引入了表达式主体，并未使用 `get` 关键字。

自 C# 7.0 起，get 和 set 访问器均可作为表达式主体成员实现。 在这种情况下，必须使用 `get` 和 `set` 关键字。 例如：

[!code-csharp[indexers#3](../../../../samples/snippets/csharp/programming-guide/indexers/indexer-3.cs)]  
  
## <a name="indexers-overview"></a>索引器概述  
  
- 使用索引器可以用类似于数组的方式为对象建立索引。  
  
- `get` 取值函数返回值。 `set` 取值函数分配值。  
  
- [this](../../language-reference/keywords/this.md) 关键字用于定义索引器。  
  
- [value](../../language-reference/keywords/value.md) 关键字用于定义由 `set` 访问器分配的值。  
  
- 索引器不必根据整数值进行索引；由你决定如何定义特定的查找机制。  
  
- 索引器可被重载。  
  
- 索引器可以有多个形参，例如当访问二维数组时。  
  
## <a name="related-sections"></a><a name="BKMK_RelatedSections"></a>相关部分  
  
- [使用索引器](./using-indexers.md)  
  
- [接口中的索引器](./indexers-in-interfaces.md)  
  
- [属性与索引器之间的比较](./comparison-between-properties-and-indexers.md)  
  
- [限制访问器可访问性](../classes-and-structs/restricting-accessor-accessibility.md)  
  
## <a name="c-language-specification"></a>C# 语言规范  

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[索引器](~/_csharplang/spec/classes.md#indexers)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [属性](../classes-and-structs/properties.md)
