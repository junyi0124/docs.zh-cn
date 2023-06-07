---
title: LINQ to Objects
ms.date: 07/20/2015
ms.assetid: dd4c30bc-1c9b-4781-a482-b5eada38deb2
ms.openlocfilehash: cb277a255151bf7db9d0fb0a50605ee07ba1423f
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075286"
---
# <a name="linq-to-objects-visual-basic"></a>LINQ to Objects (Visual Basic)

术语“LINQ to Objects”指直接将 LINQ 查询与任何 <xref:System.Collections.IEnumerable> 或 <xref:System.Collections.Generic.IEnumerable%601> 集合一起使用，而不使用中间 LINQ 提供程序或 API，例如 [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md) 或 [LINQ to XML](../../../../standard/linq/linq-xml-overview.md)。 可以使用 LINQ 来查询任何可枚举的集合，例如 <xref:System.Collections.Generic.List%601>、<xref:System.Array> 或 <xref:System.Collections.Generic.Dictionary%602>。 该集合可以是用户定义的集合，也可以是由 .NET Framework API 返回的集合。  
  
 从根本上说，“LINQ to Objects”表示一种新的处理集合的方法。 采用旧方法，必须编写指定如何从集合检索数据的复杂的 `For Each` 循环。 而采用 LINQ 方法，只需编写描述要检索的内容的声明性代码。  
  
 此外，LINQ 查询与传统 `For Each` 循环相比具有三大优势：  
  
1. 它们更简明、更易读，尤其在筛选多个条件时。  
  
2. 它们使用最少的应用程序代码提供强大的筛选、排序和分组功能。  
  
3. 无需修改或只需做很小的修改即可将它们移植到其他数据源。  
  
 通常，对数据执行的操作越复杂，就越能体会到 LINQ 相较于传统迭代技术的优势。  
  
 本节的目的是使用一些精选示例来演示 LINQ 方法。 并不打算详尽说明。  
  
## <a name="in-this-section"></a>本节内容  

 [LINQ 和字符串 (Visual Basic)](linq-and-strings.md)  
 阐释如何使用 LINQ 来查询和转换字符串和字符串集合。 还包括指向演示这些原则的主题的链接。  
  
 [LINQ 和反射 (Visual Basic) ](linq-and-reflection.md)  
 指向演示 LINQ 如何使用反射的示例的链接。  
  
 [LINQ 和文件目录 (Visual Basic)](linq-and-file-directories.md)  
 阐释如何使用 LINQ 来与文件系统进行交互。 还包括指向演示这些概念的主题的链接。  
  
 [如何：使用 LINQ (Visual Basic 查询 ArrayList) ](how-to-query-an-arraylist-with-linq.md)  
 演示如何使用 C# 查询 ArrayList。  
  
 [如何：为 LINQ 查询添加自定义方法 (Visual Basic) ](how-to-add-custom-methods-for-linq-queries.md)  
 阐释如何通过向 <xref:System.Collections.Generic.IEnumerable%601> 接口中添加扩展方法来扩展可用于 LINQ 查询的方法集。  
  
 [语言集成查询 (LINQ) (Visual Basic)](index.md)  
 提供指向阐释 LINQ 并提供执行查询的代码示例的主题的链接。
