---
title: 聚合查询
ms.date: 03/30/2017
ms.assetid: 13ec5580-05ce-4a1f-9d3d-8660be7891a2
ms.openlocfilehash: 8dfe24a84c707b6d21afb7ccfc57ac7b0423942f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161537"
---
# <a name="aggregate-queries"></a>聚合查询

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支持 `Average`、`Count`、`Max`、`Min` 和 `Sum` 聚合运算符。 请注意 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中聚合运算符的以下特征：  
  
- 聚合查询是立即执行的。  
  
     有关详细信息，请参阅 [LINQ 查询简介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)。  
  
- 聚合查询通常返回一个数字，而非一个集合。  
  
     有关详细信息，请参阅 [聚合运算](/previous-versions/visualstudio/visual-studio-2013/bb546138(v=vs.120))。  
  
- 不能对匿名类型调用聚合。  
  
 以下主题中的示例来自 Northwind 示例数据库。 有关详细信息，请参阅 [下载示例数据库](downloading-sample-databases.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [从数值序列中返回平均值](return-the-average-value-from-a-numeric-sequence.md)  
 演示如何使用 <xref:System.Linq.Enumerable.Average%2A> 运算符。  
  
 [对序列中元素的数目进行计数](count-the-number-of-elements-in-a-sequence.md)  
 演示如何使用 <xref:System.Linq.Enumerable.Count%2A> 运算符。  
  
 [查找数值序列中的最大值](find-the-maximum-value-in-a-numeric-sequence.md)  
 演示如何使用 <xref:System.Linq.Enumerable.Max%2A> 运算符。  
  
 [查找数值序列中的最小值](find-the-minimum-value-in-a-numeric-sequence.md)  
 演示如何使用 <xref:System.Linq.Enumerable.Min%2A> 运算符。  
  
 [计算数值序列中值的和](compute-the-sum-of-values-in-a-numeric-sequence.md)  
 演示如何使用 <xref:System.Linq.Enumerable.Sum%2A> 运算符。  
  
## <a name="related-sections"></a>相关章节  

 [查询示例](query-examples.md)  
 提供指向 Visual Basic 和 C# 中 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 查询的链接。  
  
 [查询概念](query-concepts.md)  
 提供指向一些主题的链接，这些主题说明如何在中设计 LINQ 查询 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 。  
  
 [LINQ 查询简介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)  
 说明查询在 LINQ 中的工作方式。
