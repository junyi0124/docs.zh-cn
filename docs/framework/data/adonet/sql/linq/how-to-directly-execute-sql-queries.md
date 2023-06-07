---
title: 如何：直接执行 SQL 查询
description: 了解如何使用 ExecuteQuery 运行查询，并在 LINQ to SQL 查询不足的情况下将结果直接转换为对象。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e491b9bf-741a-4296-9f51-76c25ddf6a82
ms.openlocfilehash: 7ebd02581d789266396b58296bbd6ad312dd468e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200571"
---
# <a name="how-to-directly-execute-sql-queries"></a>如何：直接执行 SQL 查询

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 将您编写的查询转换成参数化 SQL 查询（以文本形式），然后将它们发送至 SQL 服务器进行处理。  
  
 SQL 无法执行可由您的应用程序在本地使用的各种方法。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会设法将这些本地方法转换成在 SQL 环境中可用的等效操作和函数。 .NET Framework 内置类型上的大多数方法和运算符都可以直接翻译到 SQL 命令。 有些则可以用可用的函数生成。 无法生成的那些方法和运算符会产生运行时异常。 有关详细信息，请参阅 [SQL-CLR 类型映射](sql-clr-type-mapping.md)。  
  
 如果 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 查询不足以满足专门任务的需要，你可以使用 <xref:System.Data.Linq.DataContext.ExecuteQuery%2A> 方法来执行 SQL 查询，然后将查询的结果直接转换成对象。  
  
## <a name="example"></a>示例  

 在下面的示例中，假定 `Customer` 类的数据分布在两个表（customer1 和 customer2）中。 此查询将返回 `Customer` 对象的序列。  
  
 [!code-csharp[DLinqQuerying#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#4)]
 [!code-vb[DLinqQuerying#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#4)]  
  
 只要表格结果中的列名与您的实体类的列属性匹配，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 就会为您创建不在任何 SQL 查询范围之内的对象。  
  
## <a name="example"></a>示例  

 <xref:System.Data.Linq.DataContext.ExecuteQuery%2A> 方法也允许带有参数。 请使用类似如下内容的代码来执行参数化查询。  
  
 [!code-csharp[DLinqQuerying#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#5)]
 [!code-vb[DLinqQuerying#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#5)]  
  
 在查询文本中使用 `Console.WriteLine()` 和 `String.Format()` 所用的大括号表示法来表示参数。 事实上， `String.Format()` 实际上是在你所提供的查询字符串上调用，用生成的参数名（例如 @p0 ， @p1 ...） @p (n) 来替换大大括号内参数。  
  
## <a name="see-also"></a>请参阅

- [背景信息](background-information.md)
- [查询数据库](querying-the-database.md)
