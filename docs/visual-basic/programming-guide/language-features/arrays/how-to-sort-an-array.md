---
title: 如何：对数组进行排序
ms.date: 07/20/2015
f1_keywords:
- Array.Sort
helpviewer_keywords:
- arrays [Visual Basic], sorting
- examples [Visual Basic], arrays
ms.assetid: 9289aeaa-9626-4698-94a7-1d1fd3702b87
ms.openlocfilehash: 3fb9af8de0fc86075fdccd64506c855c1c720660
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351856"
---
# <a name="how-to-sort-an-array-in-visual-basic"></a>如何：在 Visual Basic 中对数组进行排序

本文演示如何对 Visual Basic 中的字符串数组进行排序。

## <a name="example"></a>示例

此示例声明一个名为 `zooAnimals``String` 对象的数组，填充该数组，然后按字母顺序对其进行排序：
  
```vb
Private Sub SortAnimals()
    Dim zooAnimals(2) As String
    zooAnimals(0) = "lion"
    zooAnimals(1) = "turtle"
    zooAnimals(2) = "ostrich"
    Array.Sort(zooAnimals)
End Sub
```

## <a name="robust-programming"></a>可靠编程

以下情况可能会导致异常：

- 数组为空（<xref:System.ArgumentNullException> 类）。
- Array 为多维（<xref:System.RankException> 类）。
- 数组中的一个或多个元素未实现 <xref:System.IComparable> 接口（<xref:System.InvalidOperationException> 类）。

## <a name="see-also"></a>另请参阅

- <xref:System.Array.Sort%2A?displayProperty=nameWithType>
- [数组](index.md)
- [数组疑难解答](troubleshooting-arrays.md)
- [集合](../../concepts/collections.md)
- [For Each...Next 语句](../../../language-reference/statements/for-each-next-statement.md)
