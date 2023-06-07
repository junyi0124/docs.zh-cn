---
title: 如何：创建异步 Windows Presentation Framework 应用程序（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, asynchronous operations
ms.assetid: 834614df-1427-4839-b0be-90f68e5afffd
ms.openlocfilehash: 67f6e27e4042e1cddbef8b99a2235e1a21d270d2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91152737"
---
# <a name="how-to-create-an-asynchronous-windows-presentation-framework-application-wcf-data-services"></a>如何：创建异步 Windows Presentation Framework 应用程序（WCF 数据服务）

使用 WCF 数据服务，可以将从数据服务获取的数据绑定到 Windows Presentation Framework (WPF) 应用程序的 UI 元素。 有关详细信息，请参阅 [将数据绑定到控件](binding-data-to-controls-wcf-data-services.md)。您还可以采用异步方式对数据服务执行操作，从而使应用程序能够在等待对数据服务请求的响应时继续响应。 若要对数据服务进行异步访问，必须使用适用于 Silverlight 的应用程序。 有关详细信息，请参阅 [异步操作](asynchronous-operations-wcf-data-services.md)。  
  
 本主题介绍如何对数据服务进行异步访问并将结果绑定到 WPF 应用程序的元素。 本主题中的示例使用 Northwind 示例数据服务和自动生成的客户端数据服务类。 此服务和客户端数据类是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。  
  
## <a name="example"></a>示例  

 以下是定义 WPF 应用程序窗口的 XAML。  
  
 [!code-xaml[Astoria Northwind Client#WpfDataBindingAsyncXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerordersasync.xaml#wpfdatabindingasyncxaml)]  
  
## <a name="example"></a>示例  

 该 XAML 文件的以下代码隐藏页通过使用数据服务执行一个异步查询，并将结果绑定到 WPF 窗口中的元素。  
  
 [!code-csharp[Astoria Northwind Client#WpfDataBindingAsync](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customerordersasync.xaml.cs#wpfdatabindingasync)]
 [!code-vb[Astoria Northwind Client#WpfDataBindingAsync](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerordersasync.xaml.vb#wpfdatabindingasync)]  
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
