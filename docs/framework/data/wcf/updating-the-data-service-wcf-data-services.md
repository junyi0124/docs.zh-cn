---
title: 更新数据服务（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, changing data
- WCF Data Services, client library
ms.assetid: 00d993be-ffed-4dea-baf7-6eea982cdb54
ms.openlocfilehash: 3e2bd3f4ca5402abe4a7f8ec8f5410effaee6700
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180603"
---
# <a name="updating-the-data-service-wcf-data-services"></a>更新数据服务（WCF 数据服务）

当你使用 WCF 数据服务的客户端库来使用 Open Data Protocol (OData) 源时，库会将源中的条目转换为客户端数据服务类的实例。 通过使用 <xref:System.Data.Services.Client.DataServiceContext> 所属的 <xref:System.Data.Services.Client.DataServiceQuery%601> 跟踪这些数据服务类。 客户端通过使用 <xref:System.Data.Services.Client.DataServiceContext> 上的方法跟踪您报告的实体更改。 客户端利用这些方法可跟踪已添加和删除的实体以及您对属性值或对实体实例之间关系的更改。 调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 方法时，将以基于 REST 的操作的形式将这些跟踪的更改发送回数据服务。  
  
> [!NOTE]
> 当使用 <xref:System.Data.Services.Client.DataServiceCollection%601> 实例将数据绑定到控件时，对绑定控件中数据所做的更改自动报告给 <xref:System.Data.Services.Client.DataServiceContext>。 有关详细信息，请参阅 [将数据绑定到控件](binding-data-to-controls-wcf-data-services.md)。  
  
## <a name="adding-modifying-and-changing-entities"></a>添加、修改和更改实体  

 使用 Visual Studio 中的 **添加服务引用** 对话框添加对 OData 源的引用时，每个生成的客户端数据服务类都具有静态 *Create* 方法，该方法将一个参数用于每个不可为 null 的实体属性。 可以使用此方法创建实体类型类的实例，如下面的示例所示：  
  
 [!code-csharp[Astoria Northwind Client#CreateNewProduct](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#createnewproduct)]
 [!code-vb[Astoria Northwind Client#CreateNewProduct](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#createnewproduct)]  
  
 若要添加实体实例，请在 "添加服务引用" 对话框生成的类上调用相应的*AddTo*方法 <xref:System.Data.Services.Client.DataServiceContext> ，如以下示例中所示： **Add Service Reference**  
  
 [!code-csharp[Astoria Northwind Client#AddProductSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addproductspecific)]
 [!code-vb[Astoria Northwind Client#AddProductSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addproductspecific)]  
  
 这会将对象添加到上下文和正确的实体集中。 您还可以调用 <xref:System.Data.Services.Client.DataServiceContext.AddObject%2A>，但是必须提供实体集名称。 如果添加的实体具有与其他实体的一种或多种关系，则您可以使用 <xref:System.Data.Services.Client.DataServiceContext.AddRelatedObject%2A> 方法或使用上述方法之一，还可以显式定义这些链接。 这些操作将在本主题的后面部分进行讨论。  
  
 若要修改现有的实体实例，请首先查询该实体，再对其属性进行必要的更改，然后调用 <xref:System.Data.Services.Client.DataServiceContext.UpdateObject%2A> 的 <xref:System.Data.Services.Client.DataServiceContext> 方法，以向客户端库指明它需要发送该对象的更新，如下面的示例所示：  
  
 [!code-csharp[Astoria Northwind Client#ModifyCustomerSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#modifycustomerspecific)]
 [!code-vb[Astoria Northwind Client#ModifyCustomerSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#modifycustomerspecific)]  
  
 若要删除实体实例，请调用 <xref:System.Data.Services.Client.DataServiceContext.DeleteObject%2A> 的 <xref:System.Data.Services.Client.DataServiceContext> 方法，如下面的示例所示：  
  
 [!code-csharp[Astoria Northwind Client#DeleteProductSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#deleteproductspecific)]
 [!code-vb[Astoria Northwind Client#DeleteProductSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#deleteproductspecific)]  
  
 有关详细信息，请参阅 [如何：添加、修改和删除实体](how-to-add-modify-and-delete-entities-wcf-data-services.md)。  
  
## <a name="attaching-entities"></a>附加实体  

 通过客户端库，可以保存对实体所做的更新，不必先执行查询来将实体加载到 <xref:System.Data.Services.Client.DataServiceContext>。 使用 <xref:System.Data.Services.Client.DataServiceContext.AttachTo%2A> 方法将现有对象附加到 <xref:System.Data.Services.Client.DataServiceContext> 中的特定实体集。 然后，可以修改对象，保存对数据服务的更改。 在下面的示例中，已更改的客户对象附加到上下文，然后在调用 <xref:System.Data.Services.Client.DataServiceContext.UpdateObject%2A> 之前调用 <xref:System.Data.Services.Client.EntityStates.Modified> 以便将附加的对象标记为 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A>：  
  
 [!code-csharp[Astoria Northwind Client#AttachObjectSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#attachobjectspecific)]
 [!code-vb[Astoria Northwind Client#AttachObjectSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#attachobjectspecific)]  
  
 在附加对象时需要考虑下列注意事项：  
  
- 对象以 <xref:System.Data.Services.Client.EntityStates.Unchanged> 状态附加。  
  
- 附加对象时，不会附加与附加的对象有关的对象。  
  
- 如果实体已被上下文跟踪，则不能附加对象。  
  
- 当附加收到的带有 eTag 值的实体对象时，使用采用 <xref:System.Data.Services.Client.DataServiceContext.AttachTo%28System.String%2CSystem.Object%2CSystem.String%29> 参数的 `etag` 方法重载。 然后，使用此 eTag 值在保存对附加对象的更改时检查并发。  
  
 有关详细信息，请参阅 [如何：将现有实体附加到 DataServiceContext](attach-an-existing-entity-to-dc-wcf-data.md)。  
  
## <a name="creating-and-modifying-relationship-links"></a>创建并修改关系链接  

 使用 <xref:System.Data.Services.Client.DataServiceContext.AddObject%2A> 添加服务引用对话框生成的类的方法或相应的*AddTo*方法添加新实体时 <xref:System.Data.Services.Client.DataServiceContext> ，不会自动**Add Service Reference**定义新实体和相关实体之间的任何关系。  
  
 可创建和更改实体实例之间的关系，以及让客户端库在数据服务中反映这些更改。 实体之间的关系在模型中定义为关联，<xref:System.Data.Services.Client.DataServiceContext> 在上下文中以链接对象的形式跟踪每个关系。 WCF 数据服务在类上提供以下方法 <xref:System.Data.Services.Client.DataServiceContext> 来创建、修改和删除这些链接：  
  
|方法|描述|  
|------------|-----------------|  
|<xref:System.Data.Services.Client.DataServiceContext.AddRelatedObject%2A>|在两个相关实体对象之间创建新的链接。 调用此方法相当于同时调用 <xref:System.Data.Services.Client.DataServiceContext.AddObject%2A> 和  <xref:System.Data.Services.Client.DataServiceContext.AddLink%2A> 以创建新对象并定义与现有对象的关系。|  
|<xref:System.Data.Services.Client.DataServiceContext.AddLink%2A>|在两个相关实体对象之间创建新的链接。|  
|<xref:System.Data.Services.Client.DataServiceContext.SetLink%2A>|更新两个相关实体对象之间的现有链接。 <xref:System.Data.Services.Client.DataServiceContext.SetLink%2A> 也可用于删除具有零或一对一基数 (`0..1:1`) 和一对一 (`1:1`) 基数的链接。 为此，可将相关对象设置为 `null`。|  
|<xref:System.Data.Services.Client.DataServiceContext.DeleteLink%2A>|将上下文跟踪的链接标记为在调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 方法时删除。 如果通过先删除指向现有对象的链接、再添加指向新的相关对象的链接的方式删除相关对象或更改关系，请使用此方法。|  
|<xref:System.Data.Services.Client.DataServiceContext.AttachLink%2A>|通知两个实体对象之间的现有链接的上下文。 该上下文假设此关系已在数据服务中存在，且不尝试在您调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 方法时创建链接。 如果要将对象附加到上下文且还需要附加两者之间的链接，请使用此方法。 如果要定义新关系，则应使用 <xref:System.Data.Services.Client.DataServiceContext.AddLink%2A>。|  
|<xref:System.Data.Services.Client.DataServiceContext.DetachLink%2A>|停止跟踪上下文中的指定链接。 该方法用于删除一对多 (`*:*`) 关系。 对于基数为一的关系链接，则必须使用  <xref:System.Data.Services.Client.DataServiceContext.SetLink%2A>。|  
  
 下面的示例演示如何使用 <xref:System.Data.Services.Client.DataServiceContext.AddRelatedObject%2A> 方法添加新的与现有 `Order_Detail` 实体相关的 `Orders`。 由于新的 `Order_Details` 对象现在由 <xref:System.Data.Services.Client.DataServiceContext> 跟踪，因此所添加的 `Order_Details` 对象与现有 `Products` 实体的关系将通过调用 <xref:System.Data.Services.Client.DataServiceContext.AddLink%2A> 方法进行定义：  
  
 [!code-csharp[Astoria Northwind Client#AddOrderDetailToOrderSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addorderdetailtoorderspecific)]
 [!code-vb[Astoria Northwind Client#AddOrderDetailToOrderSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addorderdetailtoorderspecific)]  
  
 <xref:System.Data.Services.Client.DataServiceContext.AddLink%2A> 方法定义的链接必须在数据服务中创建，要在上下文中的对象中反应这些链接，还必须为对象本身设置导航属性。 在上面的示例中，应当设置如下的导航属性：  
  
 [!code-csharp[Astoria Northwind Client#SetNavProps](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#setnavprops)]
 [!code-vb[Astoria Northwind Client#SetNavProps](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#setnavprops)]  
  
 有关详细信息，请参阅 [如何：定义实体关系](how-to-define-entity-relationships-wcf-data-services.md)。  
  
## <a name="saving-changes"></a>保存更改  

 在 <xref:System.Data.Services.Client.DataServiceContext> 实例中对更改进行跟踪，但不会将更改立即发送到服务器。 在完成对指定活动的所需更改后，调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 以将所有更改提交给数据服务。 有关详细信息，请参阅 [管理数据服务上下文](managing-the-data-service-context-wcf-data-services.md)。 还可以使用 <xref:System.Data.Services.Client.DataServiceContext.BeginSaveChanges%2A> 和 <xref:System.Data.Services.Client.DataServiceContext.EndSaveChanges%2A> 方法异步保存更改。 有关详细信息，请参阅 [异步操作](asynchronous-operations-wcf-data-services.md)。  
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
- [查询数据服务](querying-the-data-service-wcf-data-services.md)
- [异步操作](asynchronous-operations-wcf-data-services.md)
- [批处理操作](batching-operations-wcf-data-services.md)
- [对象具体化](object-materialization-wcf-data-services.md)
- [管理数据服务上下文](managing-the-data-service-context-wcf-data-services.md)
