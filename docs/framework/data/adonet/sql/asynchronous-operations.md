---
title: 异步操作
ms.date: 03/30/2017
ms.assetid: e7d32c3c-bf78-4bfc-a357-c9e82e4a4b3c
ms.openlocfilehash: f94a33b1ff06b5433f61687b8e28096ea37b412a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197581"
---
# <a name="asynchronous-operations"></a>异步操作

某些数据库操作（例如命令执行）可能需要大量的时间才能完成。 在这种情况下，单线程应用程序必须阻止其他操作，并等待该命令完成后才能继续执行自己的操作。 与此相反，通过将长时间运行的操作分配给后台线程，允许前台线程在操作过程中保持活动状态。 例如，在 Windows 应用程序中，将长时间运行的操作委托给后台线程允许用户界面线程在执行操作期间保持响应。  
  
 .NET Framework 提供若干标准异步设计模式，开发人员可以通过这些模式充分利用后台线程并释放用户界面线程或高优先级的线程以完成其他操作。 ADO.NET 在其 <xref:System.Data.SqlClient.SqlCommand> 类中支持相同的设计模式。 具体而言，<xref:System.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A>、<xref:System.Data.SqlClient.SqlCommand.BeginExecuteReader%2A> 和 <xref:System.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A> 方法与 <xref:System.Data.SqlClient.SqlCommand.EndExecuteNonQuery%2A>、<xref:System.Data.SqlClient.SqlCommand.EndExecuteReader%2A> 和 <xref:System.Data.SqlClient.SqlCommand.EndExecuteXmlReader%2A> 方法一起提供异步支持。  
  
> [!NOTE]
> 异步编程是 .NET Framework 的核心功能，并且 ADO.NET 充分利用了标准设计模式。 有关可供开发人员使用的不同异步技术的更多信息，请参阅[以异步方式调用同步方法](../../../../standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md)。  
  
 尽管将异步技术与 ADO.NET 功能一起使用没有什么新增的需特别注意的事项，但与 .NET Framework 的其他领域相比，很可能更多的开发人员将在 ADO.NET 中使用异步功能。 了解创建多线程应用程序的优缺点十分重要。 本部分中的示例指出了开发人员在构建合并多线程功能的应用程序时需要考虑的几个重要问题。  
  
## <a name="in-this-section"></a>本节内容  

 [使用回调的 Windows 应用程序](windows-applications-using-callbacks.md)  
 提供了一个示例，演示如何安全地执行异步命令，以及如何在单独线程中正确地处理与窗体及其内容的交互。  
  
 [使用等待句柄的 ASP.NET 应用程序](aspnet-apps-using-wait-handles.md)  
 提供了一个示例，演示如何通过 ASP.NET 页执行多个并发命令，使用 Wait 句柄在所有命令完成时管理操作。  
  
 [在控制台应用程序中轮询](polling-in-console-applications.md)  
 提供了一个示例，演示如何使用轮询等待从控制台应用程序中完成异步命令执行。 此方法也适用于类库或其他不带用户界面的应用程序。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 和 ADO.NET](index.md)
- [使用异步方式调用同步方法](../../../../standard/asynchronous-programming-patterns/calling-synchronous-methods-asynchronously.md)
- [ADO.NET 概述](../ado-net-overview.md)
