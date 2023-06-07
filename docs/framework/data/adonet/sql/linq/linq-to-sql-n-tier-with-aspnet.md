---
title: 具有 ASP.NET 的 LINQ to SQL N 层
ms.date: 03/30/2017
ms.assetid: f6cc863a-d6a6-4281-ba8b-197c01cf6c6f
ms.openlocfilehash: a184c9dcb29e7994aefa4062be2b30484539c4e1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175305"
---
# <a name="linq-to-sql-n-tier-with-aspnet"></a>具有 ASP.NET 的 LINQ to SQL N 层

在使用的 ASP.NET 应用程序中 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ，你将使用 <xref:System.Web.UI.WebControls.LinqDataSource> Web 服务器控件。 控件处理查询所必须满足的大部分逻辑 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ，将数据传递到浏览器，检索数据，然后将其提交到， [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] <xref:System.Data.Linq.DataContext> 然后更新数据库。 只需要在标记中配置该控件，然后由该控件处理 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 和浏览器之间的所有数据传输。 由于该控件处理与表示层之间的交互，并 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 处理与数据层之间的通信，因此，ASP.NET 多层应用程序中的主要焦点是编写自定义业务逻辑。  
  
 有关的详细信息 `LINQDataSource` ，请参阅 [LinqDataSource Web 服务器控件概述](/previous-versions/aspnet/bb547113(v=vs.100))。  
  
## <a name="see-also"></a>请参阅

- [N 层和远程应用程序与 LINQ to SQL](n-tier-and-remote-applications-with-linq-to-sql.md)
