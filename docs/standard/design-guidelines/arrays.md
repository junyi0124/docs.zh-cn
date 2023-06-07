---
title: 数组
ms.date: 10/22/2008
helpviewer_keywords:
- class library design guidelines [.NET Framework], arrays
- arrays [.NET Framework], usage guidelines
- empty arrays
ms.assetid: 66a1b3d8-6f3f-4715-b235-e1ff95e32d8e
ms.openlocfilehash: 11c1d23af4cf599ba632144634947520a1647ae7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701388"
---
# <a name="arrays"></a>数组

✔️确实更喜欢在公共 Api 中对数组使用集合。 " [集合](guidelines-for-collections.md) " 部分提供了有关如何在集合和数组之间进行选择的详细信息。

 ❌ 不要使用只读数组字段。 字段本身是只读的并且不能更改，但可以更改数组中的元素。

 ✔️考虑使用交错数组而不是多维数组。

 交错数组是具有同样数组的元素的数组。 构成元素的数组可以是不同的大小，从而减少某些数据集的浪费空间， (例如，稀疏矩阵) 与多维数组相比。 此外，CLR 还会优化交错数组的索引操作，因此，在某些情况下，它们可能会表现出更好的运行时性能。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- <xref:System.Array>
- [框架设计准则](index.md)
- [使用准则](usage-guidelines.md)
