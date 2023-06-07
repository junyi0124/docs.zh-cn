---
title: 元素运算 (C#)
description: 了解执行元素操作的标准查询运算符方法，这些方法从 C# 中的序列返回单个元素。
ms.date: 10/03/2018
ms.assetid: 283206c9-3246-4c48-b01a-d9de409a7231
ms.openlocfilehash: 8cd34146a3b93205ec01ed128aacb79d566a03c6
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105453"
---
# <a name="element-operations-c"></a>元素运算 (C#)

元素运算从序列中返回唯一、特定的元素。  
  
 下节列出了执行元素运算的标准查询运算符方法。  
  
## <a name="methods"></a>方法  
  
|方法名|描述|C# 查询表达式语法|详细信息|  
|-----------------|-----------------|---------------------------------|----------------------|  
|ElementAt|返回集合中指定索引处的元素。|不适用。|<xref:System.Linq.Enumerable.ElementAt%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ElementAt%2A?displayProperty=nameWithType>|  
|ElementAtOrDefault|返回集合中指定索引处的元素；如果索引超出范围，则返回默认值。|不适用。|<xref:System.Linq.Enumerable.ElementAtOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ElementAtOrDefault%2A?displayProperty=nameWithType>|  
|First|返回集合的第一个元素或满足条件的第一个元素。|不适用。|<xref:System.Linq.Enumerable.First%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.First%2A?displayProperty=nameWithType>|  
|FirstOrDefault|返回集合的第一个元素或满足条件的第一个元素。 如果此类元素不存在，则返回默认值。|不适用。|<xref:System.Linq.Enumerable.FirstOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.FirstOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.FirstOrDefault%60%601%28System.Linq.IQueryable%7B%60%600%7D%29?displayProperty=nameWithType>|  
|上一个|返回集合的最后一个元素或满足条件的最后一个元素。|不适用。|<xref:System.Linq.Enumerable.Last%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Last%2A?displayProperty=nameWithType>|  
|LastOrDefault|返回集合的最后一个元素或满足条件的最后一个元素。 如果此类元素不存在，则返回默认值。|不适用。|<xref:System.Linq.Enumerable.LastOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.LastOrDefault%2A?displayProperty=nameWithType>|  
|Single|返回集合的唯一一个元素或满足条件的唯一一个元素。 如果没有要返回的元素或要返回多个元素，则引发 <xref:System.InvalidOperationException>。 |不适用。|<xref:System.Linq.Enumerable.Single%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Single%2A?displayProperty=nameWithType>|  
|SingleOrDefault|返回集合的唯一一个元素或满足条件的唯一一个元素。 如果没有要返回的元素，则返回默认值。 如果要返回多个元素，则引发 <xref:System.InvalidOperationException>。 |不适用。|<xref:System.Linq.Enumerable.SingleOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SingleOrDefault%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
- [如何查询目录树中的一个或多个最大的文件 (LINQ) (C#)](./how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq.md)
