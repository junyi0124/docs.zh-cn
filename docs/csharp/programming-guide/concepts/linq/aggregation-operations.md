---
title: 聚合运算 (C#)
description: 了解执行聚合操作的方法。 聚合运算从值的集合中计算出单个值。
ms.date: 07/20/2015
ms.assetid: 6fc035e5-7639-48b8-bc7f-b093dd31b039
ms.openlocfilehash: c302667ec7f1d4ed5acb98439ea9f8f80250077d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91159379"
---
# <a name="aggregation-operations-c"></a>聚合运算 (C#)

聚合运算从值的集合中计算出单个值。 例如，从一个月累计的每日温度值计算出日平均温度值就是一个聚合运算。  
  
 下图显示对数字序列进行两种不同聚合操作所得结果。 第一个操作累加数字。 第二个操作返回序列中的最大值。  
  
 ![显示 LINQ 聚合运算的插图。](./media/aggregation-operations/linq-aggregation-operations.png)  
  
 下节列出了执行聚合运算的标准查询运算符方法。  
  
## <a name="methods"></a>方法  
  
|方法名|描述|C# 查询表达式语法|详细信息|  
|-----------------|-----------------|---------------------------------|----------------------|  
|聚合|对集合的值执行自定义聚合运算。|不适用。|<xref:System.Linq.Enumerable.Aggregate%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Aggregate%2A?displayProperty=nameWithType>|  
|平均值|计算值集合的平均值。|不适用。|<xref:System.Linq.Enumerable.Average%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Average%2A?displayProperty=nameWithType>|  
|计数|对集合中元素计数，可选择仅对满足谓词函数的元素计数。|不适用。|<xref:System.Linq.Enumerable.Count%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Count%2A?displayProperty=nameWithType>|  
|LongCount|对大型集合中元素计数，可选择仅对满足谓词函数的元素计数。|不适用。|<xref:System.Linq.Enumerable.LongCount%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.LongCount%2A?displayProperty=nameWithType>|  
|最大值|确定集合中的最大值。|不适用。|<xref:System.Linq.Enumerable.Max%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Max%2A?displayProperty=nameWithType>|  
|最小值|确定集合中的最小值。|不适用。|<xref:System.Linq.Enumerable.Min%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Min%2A?displayProperty=nameWithType>|  
|Sum|对集合中的值求和。|不适用。|<xref:System.Linq.Enumerable.Sum%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Sum%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
- [如何在 CSV 文本文件中计算列值 (LINQ) (C#)](./how-to-compute-column-values-in-a-csv-text-file-linq.md)
- [如何查询目录树中的一个或多个最大的文件 (LINQ) (C#)](./how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq.md)
- [如何查询一组文件夹中的总字节数 (LINQ) (C#)](./how-to-query-for-the-total-number-of-bytes-in-a-set-of-folders-linq.md)
