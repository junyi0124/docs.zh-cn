---
title: 作为对象的数组 - C# 编程指南
description: C# 中的数组是抽象基类型数组的对象。 可以使用数组的属性和其他类成员（例如 Length 属性）。
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [C#], as objects
ms.assetid: f76d4403-bd0a-42a0-9bc8-694c55b2c926
ms.openlocfilehash: 984def3ef74b07d7099fae6cae6d6f8ce7e03e12
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474717"
---
# <a name="arrays-as-objects-c-programming-guide"></a>作为对象的数组（C# 编程指南）

在 C# 中，数组实际上是对象，而不只是如在 C 和 C++ 中的连续内存的可寻址区域。 <xref:System.Array> 是所有数组类型的抽象基类型。 可以使用 <xref:System.Array> 具有的属性和其他类成员。 例如，使用 <xref:System.Array.Length%2A> 属性来获取数组的长度。 以下代码可将 `numbers` 数组的长度 `5` 分配给名为 `lengthOfNumbers` 的变量：

[!code-csharp[csProgGuideArrays#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#3)]

<xref:System.Array> 类可提供多种其他有用的方法和属性，用于对数组进行排序、搜索和复制。

## <a name="example"></a>示例

此示例使用 <xref:System.Array.Rank%2A> 属性显示数组的维数。

[!code-csharp[csProgGuideArrays#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#2)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [数组](./index.md)
- [一维数组](./single-dimensional-arrays.md)
- [多维数组](./multidimensional-arrays.md)
- [交错数组](./jagged-arrays.md)
