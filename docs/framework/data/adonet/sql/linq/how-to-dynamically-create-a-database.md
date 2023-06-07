---
title: 如何：动态创建数据库
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fb7f23c4-4572-4c38-9898-a287807d070c
ms.openlocfilehash: 95073eed3e0534a74272ee426ac6e329954c85a6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173479"
---
# <a name="how-to-dynamically-create-a-database"></a>如何：动态创建数据库

在 LINQ to SQL 中，对象模型映射到关系数据库。 通过使用基于属性的映射或外部映射文件启用映射，以描述关系数据库的结构。 在两种方案中，存在足够的有关使用 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法创建新的数据库实例的关系数据库的信息。  
  
 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法仅根据对象模型中已编码的信息创建数据库的副本。 映射文件和您的对象模型中的属性可能不会对有关现有数据库结构的所有内容都进行编码。 映射信息不表示用户定义的函数、存储过程、触发器或 CHECK 约束的内容。 这种行为足以满足各种数据库的需要。  
  
 您可以在任意数量的方案中，尤其是在已知数据提供程序（如 Microsoft SQL Server 2008）可用时，使用 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法。 典型的方案包括：  
  
- 您正在生成自动将自身安装到客户系统上的应用程序。  
  
- 您正在生成需要用本地数据库来保存其脱机状态的客户端应用程序。  
  
 您还可以通过使用 .mdf 文件或只使用目录名（取决于您的连接字符串），将 <xref:System.Data.Linq.DataContext.CreateDatabase%2A?displayProperty=nameWithType> 方法与 SQL Server 一起使用。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 使用连接字符串来定义要创建的数据库和作为数据库创建位置的服务器。  
  
> [!NOTE]
> 请尽可能使用 Windows 集成安全性连接到数据库，以便连接字符串不需要密码。  
  
## <a name="example"></a>示例  

 下面的代码提供了一个示例，此示例演示了如何创建一个名为 MyDVDs.mdf 的新数据库。  
  
 [!code-csharp[DLinqSubmittingChanges#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSubmittingChanges/cs/Program.cs#5)]
 [!code-vb[DLinqSubmittingChanges#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSubmittingChanges/vb/Module1.vb#5)]  
  
## <a name="example"></a>示例  

 您可以使用对象模型按如下方式操作来创建数据库：  
  
 [!code-csharp[DLinqSubmittingChanges#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSubmittingChanges/cs/Program.cs#6)]
 [!code-vb[DLinqSubmittingChanges#6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSubmittingChanges/vb/Module1.vb#6)]  
  
## <a name="example"></a>示例  

 生成自动将自身安装在客户系统上的应用程序时，请检查数据库是否已经存在，并且在创建新的数据库前将其删除。 <xref:System.Data.Linq.DataContext> 类提供帮助您完成此过程的 <xref:System.Data.Linq.DataContext.DatabaseExists%2A> 和 <xref:System.Data.Linq.DataContext.DeleteDatabase%2A> 方法。  
  
 下面的示例演示使用这些方法来实现此方法的一种方式：  
  
 [!code-csharp[DLinqSubmittingChanges#7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSubmittingChanges/cs/Program.cs#7)]
 [!code-vb[DLinqSubmittingChanges#7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSubmittingChanges/vb/Module1.vb#7)]  
  
## <a name="see-also"></a>请参阅

- [基于特性的映射](attribute-based-mapping.md)
- [外部映射](external-mapping.md)
- [SQL-CLR 类型映射](sql-clr-type-mapping.md)
- [背景信息](background-information.md)
- [进行和提交数据更改](making-and-submitting-data-changes.md)
