---
title: 如何：自定义数据绑定行为（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, customizing
- WCF Data Services, data binding
ms.assetid: 40476b89-8941-4771-8d21-2fe430c85a9d
ms.openlocfilehash: 13847923a5f31108e93ef12cf7775109be3cd9eb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172510"
---
# <a name="how-to-customize-data-binding-behaviors-wcf-data-services"></a>如何：自定义数据绑定行为（WCF 数据服务）

使用 WCF 数据服务，可以提供自定义逻辑，该逻辑在 <xref:System.Data.Services.Client.DataServiceCollection%601> 添加或从绑定集合中删除对象或检测到属性更改时由调用。 此自定义逻辑作为方法提供，作为 <xref:System.Func%602> 委托引用， `false` 如果在自定义方法完成时仍应执行默认行为，并且 `true` 应停止事件的后续处理，则这些方法将返回值。  
  
 本主题中的示例为 `entityChanged` 的 `entityCollectionChanged` 和 <xref:System.Data.Services.Client.DataServiceCollection%601> 参数提供了自定义方法。 本主题中的示例使用 Northwind 示例数据服务和自动生成的客户端数据服务类。 此服务和客户端数据类是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。  
  
## <a name="example"></a>示例  

 下面的 XAML 文件代码隐藏页使用当更改绑定到绑定集合的数据时调用的自定义方法来创建 <xref:System.Data.Services.Client.DataServiceCollection%601>。 当发生 <xref:System.Collections.ObjectModel.ObservableCollection%601.CollectionChanged> 事件时，提供的方法阻止从数据服务删除已从绑定集合移除的项。 当发生 <xref:System.Collections.ObjectModel.ObservableCollection%601.PropertyChanged> 事件时，验证 `ShipDate` 值以确保不对已发货的订单进行更改。  
  
 [!code-csharp[Astoria Northwind Client#WpfDataBindingCustom](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customerorderscustom.xaml.cs#wpfdatabindingcustom)]
 [!code-vb[Astoria Northwind Client#WpfDataBindingCustom](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderscustom.xaml.vb#wpfdatabindingcustom)]
 [!code-vb[Astoria Northwind Client#WpfDataBindingCustom](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderscustom2.xaml.vb#wpfdatabindingcustom)]  
  
## <a name="example"></a>示例  

 下面的 XAML 定义上例的窗口。  
  
 [!code-xaml[Astoria Northwind Client#WpfDataBindingCustomXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderscustom.xaml#wpfdatabindingcustomxaml)]  
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
