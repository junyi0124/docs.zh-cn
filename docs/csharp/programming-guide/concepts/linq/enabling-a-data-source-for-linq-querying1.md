---
title: 启用数据源以进行 LINQ 查询
description: 了解如何扩展 C# 中的 LINQ，使其能够在 LINQ 模式中查询任何数据源，从而使客户端可以轻松查询数据源。
ms.date: 07/20/2015
ms.assetid: d2ef04a5-31a6-45cb-af9a-a5ce7732662c
ms.openlocfilehash: d7d751c0584072e740b4e5292071e400a5020f82
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202612"
---
# <a name="enabling-a-data-source-for-linq-querying"></a>启用数据源以进行 LINQ 查询

可通过多种方式来扩展 LINQ，以便能够在 LINQ 模式中查询任何数据源。 数据源可以是数据结构、Web 服务、文件系统或数据库等。 LINQ 模式使客户端可以轻松地查询能够进行 LINQ 查询的数据源，因为查询的语法和模式没有更改。 可通过以下方式将 LINQ 扩展到这些数据源：  
  
- 在某个类型中实现 <xref:System.Collections.Generic.IEnumerable%601> 接口，使 LINQ to Objects 能够查询该类型。  
  
- 创建扩展某个类型的标准查询运算符方法（比如 <xref:System.Linq.Enumerable.Where%2A> 和 <xref:System.Linq.Enumerable.Select%2A>），使自定义 LINQ 能够查询该类型。  
  
- 为实现 <xref:System.Linq.IQueryable%601> 接口的数据源创建一个提供程序。 实现此接口的提供程序以表达式树的形式接收 LINQ 查询，提供程序能够以自定义方式（例如以远程方式）执行该查询。  
  
- 为数据源创建一个利用现有 LINQ 技术的提供程序。 这种提供程序不仅能够进行查询，而且能够为用户定义的类型插入、更新和删除操作及映射。  
  
 本主题将讨论这些选项。  
  
## <a name="how-to-enable-linq-querying-of-your-data-source"></a>如何使 LINQ 能够查询您的数据源  
  
### <a name="in-memory-data"></a>内存中的数据  

 通过两种方式，可以使 LINQ 能够查询内存中数据。 如果数据的类型实现了 <xref:System.Collections.Generic.IEnumerable%601>，你可以使用 LINQ to Objects 来查询数据。 如果无法通过实现 <xref:System.Collections.Generic.IEnumerable%601> 接口启用类型枚举，可以在该类型中定义 LINQ 标准查询运算符方法，或创建扩展该类型的 LINQ 标准查询运算符方法。 标准查询运算符的自定义实现应使用延迟执行来返回结果。  
  
### <a name="remote-data"></a>远程数据  

 使 LINQ 能够查询远程数据源的最佳选择是实现 <xref:System.Linq.IQueryable%601> 接口。 但是，这与为数据源扩展提供程序（比如 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]）有所不同。 Visual Studio 2008 中没有用于将现有 LINQ 技术（比如 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]）扩展为其他数据源类型的提供程序模型。
  
## <a name="iqueryable-linq-providers"></a>IQueryable LINQ 提供程序  

 实现 <xref:System.Linq.IQueryable%601> 的 LINQ 提供程序之间的复杂性可能差别很大。 本节讨论这些不同程度的复杂性。  
  
 复杂性较低的 `IQueryable` 提供程序可与 Web 服务的一个方法交互。 这种类型的提供程序非常具体，因为它需要所处理的查询中有具体信息。 它有封闭的类型系统，可能会公开单一结果类型。 大多数查询都是在本地执行的，例如，通过使用标准查询运算符的 <xref:System.Linq.Enumerable> 实现来执行。 复杂性较低的提供程序可能只会检查表示查询的表达式树中的一个方法调用表达式，并允许在其他地方处理查询的其余逻辑。  
  
 中等复杂性的 `IQueryable` 提供程序可能针对具有部分可表达查询语言的数据源。 如果该提供程序针对 Web 服务，它可以与 Web 服务的多个方法进行交互，并基于查询提出的问题选择要调用的方法。 与简单提供程序相比，中等复杂性的提供程序的类型系统较丰富，但它仍然是固定类型系统。 例如，提供程序可以公开具有可遍历的一对多关系的类型，但将不会为用户定义的类型提供映射技术。  
  
 复杂的 `IQueryable` 提供程序（如 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] 提供程序）可将完整的 LINQ 查询转换为可表达查询语言（如 SQL）。 与复杂性较低的提供程序相比，复杂的提供程序更为全面，因为它能在查询中处理各种各样的问题。 它还具有开放的类型系统，因而必须包含广泛的基础结构来映射用户定义的类型。 开发复杂的提供程序需要耗费大量的精力。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq.IQueryable%601>
- <xref:System.Collections.Generic.IEnumerable%601>
- <xref:System.Linq.Enumerable>
- [标准查询运算符概述 (C#)](./standard-query-operators-overview.md)
- [LINQ to Objects (C#)](./linq-to-objects.md)
