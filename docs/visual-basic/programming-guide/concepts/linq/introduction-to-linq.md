---
title: LINQ 介绍
ms.date: 07/20/2015
ms.assetid: c6339c12-9b2d-433e-961c-0d2b7f0091c2
ms.openlocfilehash: e99da74eb69511b92ddccfb42f8002adc7be83d1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91098276"
---
# <a name="introduction-to-linq-visual-basic"></a>LINQ 简介 (Visual Basic)

语言集成查询 (LINQ) 是 .NET Framework 3.5 版中引入的一项创新功能，它在对象领域和数据领域之间架起了一座桥梁。  
  
 数据查询历来都表示为简单的字符串，没有编译时类型检查或 IntelliSense 支持。 此外，需要针对每种类型的数据源了解不同的查询语言：SQL 数据库、XML 文档、各种 Web 服务等。 LINQ 使 *查询* 成为 Visual Basic 中的第一类语言构造。 可以使用语言关键字和熟悉的运算符针对强类型化对象集合编写查询。  
  
 可以在 Visual Basic 中为 SQL Server 数据库、XML 文档、ADO.NET 数据集以及支持或泛型接口的任何对象集合编写 LINQ 查询 <xref:System.Collections.IEnumerable> <xref:System.Collections.Generic.IEnumerable%601> 。 此外，第三方也为许多 Web 服务和其他数据库实现提供了 LINQ 支持。  
  
 LINQ 查询既可在新项目中使用，也可在现有项目中与非 LINQ 查询一起使用。 唯一的要求是项目应面向 .NET Framework 3.5 或更高版本。  
  
 下图为 Visual Studio 中的图例，显示了使用 C# 和 Visual Basic 针对 SQL Server 数据库编写的不完整 LINQ 查询，并具有完全类型检查和 IntelliSense 支持。  
  
 ![显示具有 Intellisense 的 LINQ 查询的图表。](./media/introduction-to-linq/linq-query-intellisense.png)  
  
## <a name="next-steps"></a>后续步骤  

 若要了解有关 LINQ 的详细信息，请首先熟悉入门部分中的一些基本概念 [入门使用 LINQ in Visual Basic](getting-started-with-linq.md)，然后阅读你感兴趣的 linq 技术的文档：  
  
- SQL Server 数据库：[LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)  
  
- XML 文档： [LINQ to XML (Visual Basic) ](../../../../standard/linq/linq-xml-overview.md)  
  
- ADO.NET 数据集：[LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)  
  
- .NET 集合、文件、字符串等： [LINQ to Objects (Visual Basic) ](linq-to-objects.md)  
  
## <a name="see-also"></a>请参阅

- [语言集成查询 (LINQ) (Visual Basic)](index.md)
