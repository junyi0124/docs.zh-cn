---
title: 比较属性和索引器 - C# 编程指南
description: 比较 C# 中的索引器与属性的相似之处。 除一些差别外，对属性访问器定义的所有规则同样适用于索引器访问器。
ms.date: 07/20/2015
helpviewer_keywords:
- properties [C#], vs. indexers
- indexers [C#], vs. properties
ms.assetid: 3358a89f-44a0-4a4d-bf8c-07237a90af39
ms.openlocfilehash: fff20ca965e7614d26f7da32321a829d13292389
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91192524"
---
# <a name="comparison-between-properties-and-indexers-c-programming-guide"></a>属性和索引器之间的比较（C# 编程指南）

索引器与属性相似。 除下表所示的差别外，对属性访问器定义的所有规则也适用于索引器访问器。  
  
|Property|索引器|  
|--------------|-------------|  
|允许以将方法视作公共数据成员的方式调用方法。|通过在对象自身上使用数组表示法，允许访问对象内部集合的元素。|  
|通过简单名称访问。|通过索引访问。|  
|可为静态成员或实例成员。|必须是实例成员。|  
|属性的 [get](../../language-reference/keywords/get.md) 访问器没有任何参数。|索引器的 `get` 访问器具有与索引器相同的形参列表。|  
|属性的 [set](../../language-reference/keywords/set.md) 访问器包含隐式 `value` 参数。|索引器的 `set` 访问器具有与索引器相同的形参列表，[value](../../language-reference/keywords/value.md) 参数也是如此。|  
|通过[自动实现的属性](../classes-and-structs/auto-implemented-properties.md)支持简短语法。|支持仅使用索引器的 expression-bodied 成员。|  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [索引器](./index.md)
- [属性](../classes-and-structs/properties.md)
