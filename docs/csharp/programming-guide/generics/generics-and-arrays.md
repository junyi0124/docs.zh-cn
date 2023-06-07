---
title: 泛型和数组 - C# 编程指南
description: 了解 C# 编程中的泛型和数组。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- generics [C#], arrays
- arrays [C#], generics
ms.assetid: 7d956536-3851-41b5-94ad-3e7c0a5fe485
ms.openlocfilehash: 808e9ddafea9806a74ccd105c8850e7b77b563be
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91151449"
---
# <a name="generics-and-arrays-c-programming-guide"></a>泛型和数组（C# 编程指南）

在 C# 2.0 和更高版本中，下限为零的单维数组自动实现 <xref:System.Collections.Generic.IList%601>。 这可使你创建可使用相同代码循环访问数组和其他集合类型的泛型方法。 此技术的主要用处在于读取集合中的数据。 <xref:System.Collections.Generic.IList%601> 接口无法用于添加元素或从数组删除元素。 如果在此上下文中尝试对数组调用 <xref:System.Collections.Generic.IList%601> 方法（例如 <xref:System.Collections.Generic.IList%601.RemoveAt%2A>），则会引发异常。  
  
 如下代码示例演示具有 <xref:System.Collections.Generic.IList%601> 输入参数的单个泛型方法如何可循环访问列表和数组（此例中为整数数组）。  
  
 [!code-csharp[csProgGuideGenerics#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#35)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Collections.Generic>
- [C# 编程指南](../index.md)
- [泛型](./index.md)
- [数组](../arrays/index.md)
- [泛型](../../../standard/generics/index.md)
