---
title: 数据分区 (C#)
description: 了解如何在 LINQ 中对数据进行分区。 查看显示分区操作结果的图示。
ms.date: 07/20/2015
ms.assetid: 2a5c507b-fe22-443c-a768-dec7f9ec568d
ms.openlocfilehash: 31beacd672addb3eb38ade8f2bf9cfae25f4d27a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176261"
---
# <a name="partitioning-data-c"></a>数据分区 (C#)

LINQ 中的分区是指将输入序列划分为两个部分的操作，无需重新排列元素，然后返回其中一个部分。  
  
 下图显示对字符序列进行三种不同的分区操作的结果。 第一个操作返回序列中的前三个元素。 第二个操作跳过前三个元素，返回剩余元素。 第三个操作跳过序列中的前两个元素，返回接下来的三个元素。  
  
 ![显示三个 LINQ 分区操作的图示。](./media/partitioning-data/linq-partitioning-operations.png)  
  
 下面一节列出了对序列进行分区的标准查询运算符方法。  
  
## <a name="operators"></a>运算符  
  
|运算符名称|描述|C# 查询表达式语法|更多信息|  
|-------------------|-----------------|---------------------------------|----------------------|  
|Skip|跳过序列中指定位置之前的元素。|不适用。|<xref:System.Linq.Enumerable.Skip%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Skip%2A?displayProperty=nameWithType>|  
|SkipWhile|基于谓词函数跳过元素，直到元素不符合条件。|不适用。|<xref:System.Linq.Enumerable.SkipWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SkipWhile%2A?displayProperty=nameWithType>|  
|Take|获取序列中指定位置之前的元素。|不适用。|<xref:System.Linq.Enumerable.Take%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Take%2A?displayProperty=nameWithType>|  
|TakeWhile|基于谓词函数获取元素，直到元素不符合条件。|不适用。|<xref:System.Linq.Enumerable.TakeWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.TakeWhile%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
