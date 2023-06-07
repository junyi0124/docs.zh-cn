---
title: 如何：显示生成的 SQL
description: 了解如何使用 Log 属性查看为查询生成的 SQL 代码，以帮助了解 LINQ to SQL 功能和调试。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 626492c0-5ee3-4675-88e8-8c40379510b6
ms.openlocfilehash: 81f6b9603cc7f8b7863f787272ce6a1af920fa75
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169474"
---
# <a name="how-to-display-generated-sql"></a>如何：显示生成的 SQL

您可以通过使用 <xref:System.Data.Linq.DataContext.Log%2A> 属性查看为查询生成的 SQL 代码和更改处理方式。 此方法对了解 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 功能和调试特定的问题可能很有用。  
  
## <a name="example"></a>示例  

 下面的示例使用 <xref:System.Data.Linq.DataContext.Log%2A> 属性在 SQL 代码执行前在控制台窗口中显示此代码。  您可以将此属性与查询、插入、更新和删除命令一起使用。  
  
 控制台窗口中的行是执行下面的 Visual Basic 或 c # 代码时看到的内容。  
  
```console  
SELECT [t0].[CustomerID], [t0].[CompanyName], [t0].[ContactName], [t0].[ContactT  
itle], [t0].[Address], [t0].[City], [t0].[Region], [t0].[PostalCode], [t0].[Coun  
try], [t0].[Phone], [t0].[Fax]  
FROM [dbo].[Customers] AS [t0]  
WHERE [t0].[City] = @p0  
-- @p0: Input String (Size = 6; Prec = 0; Scale = 0) [London]  
-- Context: SqlProvider(Sql2005) Model: AttributedMetaModel Build: 3.5.20810.0  
```  
  
```console  
AROUT  
BSBEV  
CONSH  
EASTC  
NORTS  
SEVES  
```  
  
 [!code-csharp[DLinqDebuggingSupport#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqDebuggingSupport/cs/Program.cs#1)]
 [!code-vb[DLinqDebuggingSupport#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqDebuggingSupport/vb/Module1.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [调试支持](debugging-support.md)
