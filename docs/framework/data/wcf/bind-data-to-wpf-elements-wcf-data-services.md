---
title: 如何：将数据绑定到 Windows Presentation Foundation 元素（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding, WCF Data Services
- WCF Data Services, data binding
ms.assetid: d6538ab0-0abe-426a-b9d9-e6f3a5ca2016
ms.openlocfilehash: d6f50fb849d958ae1109324f1055b84451bde5a9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191627"
---
# <a name="how-to-bind-data-to-windows-presentation-foundation-elements-wcf-data-services"></a>如何：将数据绑定到 Windows Presentation Foundation 元素（WCF 数据服务）

使用 WCF 数据服务，你可以将 Windows Presentation Foundation (WPF) 元素（如 <xref:System.Windows.Controls.ListBox> 或 <xref:System.Windows.Controls.ComboBox> ）绑定到的实例 <xref:System.Data.Services.Client.DataServiceCollection%601> ，该实例处理控件引发的事件，以使 <xref:System.Data.Services.Client.DataServiceContext> 与对控件中的数据所做的更改保持同步。 有关详细信息，请参阅 [将数据绑定到控件](binding-data-to-controls-wcf-data-services.md)。  
  
 本主题中的示例使用罗斯文示例数据服务和自动生成的客户端数据服务类。 此服务和客户端数据类是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。  
  
## <a name="example"></a>示例  

 下面的示例摘自在 WPF 中定义 `SalesOrders` 窗口的可扩展应用程序标记语言 (XAML) 页的代码隐藏页。 加载此窗口时，将根据 <xref:System.Data.Services.Client.DataServiceCollection%601> 返回客户（按国家/地区筛选）的查询结果创建。 将加载此分页结果的所有页面和相关订单，并将这些页面绑定到作为 WPF 窗口的根布局控件的 <xref:System.Windows.FrameworkElement.DataContext%2A> 的 <xref:System.Windows.Controls.StackPanel> 属性。 有关详细信息，请参阅 [加载延迟的内容](loading-deferred-content-wcf-data-services.md)。  
  
 [!code-csharp[Astoria Northwind Client#BindPagedData](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customerorderswpf3.xaml.cs#bindpageddata)]
 [!code-vb[Astoria Northwind Client#BindPagedData](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf3.xaml.vb#bindpageddata)]  
  
## <a name="example"></a>示例  

 以下 XAML 在 WPF 中为以上示例定义 `SalesOrders` 窗口。  
  
 [!code-xaml[Astoria Northwind Client#BindPagedDataXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf3.xaml#bindpageddataxaml)]  
  
## <a name="example"></a>示例  

 下面的示例摘自在 WPF 中定义 `SalesOrders` 窗口的可扩展应用程序标记语言 (XAML) 页的代码隐藏页。 加载窗口时， <xref:System.Data.Services.Client.DataServiceCollection%601> 会根据查询结果创建，该查询返回具有相关对象的客户，并按国家/地区进行筛选。 此结果绑定到作为 WPF 窗口的根布局控件的 <xref:System.Windows.FrameworkElement.DataContext%2A> 的 <xref:System.Windows.Controls.StackPanel> 属性。  
  
 [!code-csharp[Astoria Northwind Client#WpfDataBinding](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customerorderswpf.xaml.cs#wpfdatabinding)]
 [!code-vb[Astoria Northwind Client#WpfDataBinding](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf.xaml.vb#wpfdatabinding)]  
  
## <a name="example"></a>示例  

 以下 XAML 在 WPF 中为以上示例定义 `SalesOrders` 窗口。  
  
 [!code-xaml[Astoria Northwind Client#WpfDataBindingXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf.xaml#wpfdatabindingxaml)]
