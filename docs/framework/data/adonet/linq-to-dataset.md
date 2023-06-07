---
title: LINQ to DataSet
ms.date: 03/30/2017
ms.assetid: 743e3755-3ecb-45a2-8d9b-9ed41f0dcf17
ms.openlocfilehash: 1c03cca80de6003a4e49278871983ed7fcb3dc0b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200661"
---
# <a name="linq-to-dataset"></a>LINQ to DataSet

LINQ to DataSet 可以更轻松、更快速地查询在对象中缓存的数据 <xref:System.Data.DataSet> 。 具体而言，LINQ to DataSet 通过使开发人员能够使用编程语言本身而不是使用单独的查询语言来编写查询，可以简化查询。 这对于 Visual Studio 开发人员特别有用，现在可以利用 Visual Studio 在其查询中提供的编译时语法检查、静态类型和 IntelliSense 支持。  
  
 LINQ to DataSet 还可以用于查询从一个或多个数据源合并的数据。 这可以使许多需要灵活表示和处理数据的方案（例如查询本地聚合的数据和 Web 应用程序中的中间层缓存）能够实现。 具体地说，一般报告、分析和业务智能应用程序将需要这种操作方法。  
  
 LINQ to DataSet 功能主要通过和类中的扩展方法公开 <xref:System.Data.DataRowExtensions> <xref:System.Data.DataTableExtensions> 。 LINQ to DataSet 在上构建并使用现有的 ADO.NET 体系结构，而不是在应用程序代码中替换 ADO.NET。 现有的 ADO.NET 代码将继续在 LINQ to DataSet 应用程序中运行。 下图演示了 LINQ to DataSet 与 ADO.NET 和数据存储之间的关系。  
  
 ![显示 LINQ to DataSet 基于 ADO.NET 提供程序的关系图。](./media/linq-to-dataset/linq-dataset-ado-dotnet-provider.gif)  
  
## <a name="in-this-section"></a>本节内容  

 [入门](getting-started-linq-to-dataset.md)  
  
 [编程指南](programming-guide-linq-to-dataset.md)  
  
## <a name="reference"></a>参考  

 <xref:System.Data.DataTableExtensions>  
  
 <xref:System.Data.DataRowExtensions>  
  
 <xref:System.Data.DataRowComparer>  
  
## <a name="see-also"></a>请参阅

- [语言集成查询 (LINQ) - C#](../../../csharp/programming-guide/concepts/linq/index.md)
- [语言集成查询 (LINQ) - Visual Basic](../../../visual-basic/programming-guide/concepts/linq/index.md)
- [LINQ 和 ADO.NET](linq-and-ado-net.md)
- [ADO.NET](index.md)
