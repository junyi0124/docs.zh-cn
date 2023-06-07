---
title: 管理数据服务上下文（WCF 数据服务）
ms.date: 03/30/2017
ms.assetid: 15b19d09-7de7-4638-9556-6ef396cc45ec
ms.openlocfilehash: e67f7280bc85c7577f960707659890f59470e535
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194292"
---
# <a name="managing-the-data-service-context-wcf-data-services"></a>管理数据服务上下文（WCF 数据服务）

<xref:System.Data.Services.Client.DataServiceContext> 类封装针对指定数据服务支持的操作。 尽管 OData 服务是无状态的，但上下文不是。 因此，你可以使用 <xref:System.Data.Services.Client.DataServiceContext> 类在与数据服务之间的交互之间维护客户端的状态，以支持更改管理等功能。 该类还对更改的标识和跟踪进行管理。  
  
## <a name="merge-options-and-identity-resolution"></a>合并选项和标识解析  

 当执行 <xref:System.Data.Services.Client.DataServiceQuery%601> 时，响应源中的实体将具体化为对象。 有关详细信息，请参阅 [对象具体化](object-materialization-wcf-data-services.md)。 将响应消息中的项具体化为对象所采用的方式将基于标识解析，并且依赖于执行查询时所依据的合并选项。 在单个的范围内执行多个查询或加载请求时 <xref:System.Data.Services.Client.DataServiceContext> ，WCF 数据服务客户端只跟踪具有特定键值的对象的单个实例。 用于执行标识解析的此键唯一地标识一个实体。  
  
 默认情况下，客户端只针对 <xref:System.Data.Services.Client.DataServiceContext> 尚未跟踪的实体，将响应源中的项具体化为对象。 这意味着不会覆盖缓存中已存在的对象更改。 此行为通过为查询和加载操作指定 <xref:System.Data.Services.Client.MergeOption> 值进行控制。 通过设置 <xref:System.Data.Services.Client.DataServiceContext.MergeOption%2A> 的 <xref:System.Data.Services.Client.DataServiceContext> 属性可指定此选项。 默认的合并选项值为 <xref:System.Data.Services.Client.MergeOption.AppendOnly>。 这样将只为尚未进行跟踪的实体具体化对象，这意味着不会覆盖现有对象。 防止数据服务中的更新覆盖客户端上的对象更改的另一种方式是指定 <xref:System.Data.Services.Client.MergeOption.PreserveChanges>。 当指定 <xref:System.Data.Services.Client.MergeOption.OverwriteChanges> 时，客户端上对象的值将替换为响应源中对应项的最新值，即使已对这些对象进行了更改也是如此。 当使用 <xref:System.Data.Services.Client.MergeOption.NoTracking> 合并选项时，<xref:System.Data.Services.Client.DataServiceContext> 无法将对客户端对象所做的更改发送到数据服务。 使用此选项时，将始终用数据服务中的值覆盖更改。  
  
## <a name="managing-concurrency"></a>管理并发  

 OData 支持开放式并发，使数据服务能够检测更新冲突。 可以按这种方式配置数据服务提供程序，使得数据服务能够使用并发标记检查实体是否更改。 此标记包含实体类型的一个或多个属性，数据服务通过验证这些属性来确定某个资源是否已更改。 并发令牌（包括在对来自数据服务的请求和响应的 eTag 标头中）由 WCF 数据服务客户端管理。 有关详细信息，请参阅 [更新数据服务](updating-the-data-service-wcf-data-services.md)。  
  
 <xref:System.Data.Services.Client.DataServiceContext> 通过使用 <xref:System.Data.Services.Client.DataServiceContext.AddObject%2A>、<xref:System.Data.Services.Client.DataServiceContext.UpdateObject%2A> 和 <xref:System.Data.Services.Client.DataServiceContext.DeleteObject%2A> 或通过 <xref:System.Data.Services.Client.DataServiceCollection%601> 跟踪已手动报告的对对象所做的更改。 当调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 方法时，客户端将更改发送回数据服务。 如果客户端中的数据更改与数据服务中的更改冲突，则 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 会失败。 当发生这种情况时，您必须再次查询实体资源以接收更新数据。 若要覆盖数据服务中的更改，请使用 <xref:System.Data.Services.Client.MergeOption.PreserveChanges> 合并选项执行查询。 再次调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 时，只要尚未对数据服务中的资源进行其他更改，客户端上保留的更改将永久保存到数据服务。  
  
## <a name="saving-changes"></a>保存更改  

 在 <xref:System.Data.Services.Client.DataServiceContext> 实例中对更改进行跟踪，但不会将更改立即发送到服务器。 在完成对指定活动的所需更改后，调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 以将所有更改提交给数据服务。 <xref:System.Data.Services.Client.DataServiceResponse> 操作完成后，会返回 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 对象。 <xref:System.Data.Services.Client.DataServiceResponse> 对象包括一系列 <xref:System.Data.Services.Client.OperationResponse> 对象，这些对象依次又包含一系列表示已保留或尝试的更改的 <xref:System.Data.Services.Client.EntityDescriptor> 或 <xref:System.Data.Services.Client.LinkDescriptor> 实例。 在数据服务中创建或修改实体之后，<xref:System.Data.Services.Client.EntityDescriptor> 包含对已更新实体的引用，其中包括所有服务器生成的属性值，例如上面示例中生成的 `ProductID` 值。 客户端库自动更新 .NET Framework 对象以获取这些新值。  
  
 对于成功的插入和更新操作，与操作关联的 <xref:System.Data.Services.Client.EntityDescriptor> 或 <xref:System.Data.Services.Client.LinkDescriptor> 对象的状态属性设置为 <xref:System.Data.Services.Client.EntityStates.Unchanged>，并通过使用 <xref:System.Data.Services.Client.MergeOption.OverwriteChanges> 合并新值。 如果数据服务中的插入、更新或删除操作失败，则实体状态保持在调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 之前的状态，并且 <xref:System.Data.Services.Client.OperationResponse.Error%2A> 的 <xref:System.Data.Services.Client.OperationResponse> 属性设置为包含有关该错误的信息的 <xref:System.Data.Services.Client.DataServiceRequestException>。 有关详细信息，请参阅 [更新数据服务](updating-the-data-service-wcf-data-services.md)。  
  
### <a name="setting-the-http-method-for-updates"></a>为更新设置 HTTP 方法  

 默认情况下，.NET Framework 客户端库将更新作为 MERGE 请求发送至现有实体。 MERGE 请求将更新实体的选定属性；但客户端始终会在 MERGE 请求中包含所有属性，即使是未更改的属性也包含在内。 OData 协议还支持发送 PUT 请求来更新实体。 在 PUT 请求中，现有实体实际上将替换为该实体的一个新实例，新实例具有来自客户端的属性值。 要使用 PUT 请求，请在调用 <xref:System.Data.Services.Client.SaveChangesOptions.ReplaceOnUpdate> 时为 <xref:System.Data.Services.Client.SaveChangesOptions> 枚举设置 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 标志。  
  
> [!NOTE]
> 当客户端不知道实体的所有属性时，PUT 请求的行为会与 MERGE 请求不同。 当将某个实体类型投影到客户端上的新类型时，可能会发生这种情况。 当新属性已经添加到服务数据模型中的实体中，同时 <xref:System.Data.Services.Client.DataServiceContext.IgnoreMissingProperties%2A> 的 <xref:System.Data.Services.Client.DataServiceContext> 属性设置为 `true` 以忽略这类客户端映射错误时，也会发生这种情况。 在这种情况下，PUT 请求会将客户端未知的任何属性重置为默认值。  
  
### <a name="post-tunneling"></a>POST 隧道  

 默认情况下，客户端库使用 POST、GET、PUT/MERGE/PATCH 和 DELETE 的相应 HTTP 方法向 OData 服务发送创建、读取、更新和删除请求。 这保持了具象状态传输 (REST) 的基本原则。 不过，不是每个 Web 服务器实现都支持完整的 HTTP 方法集。 在某些情况下，支持的方法可能仅限 GET 和 POST。 在中介（如防火墙）用某些方法阻止请求时，可能会发生这种情况。 由于 GET 和 POST 方法通常受支持，因此 OData 要求使用 POST 请求来执行任何不受支持的 HTTP 方法。 这会*method tunneling*使客户端*POST tunneling*能够使用自定义标头中指定的实际方法发送 POST 请求 `X-HTTP-Method` 。 要为请求启用 POST 隧道，请将 <xref:System.Data.Services.Client.DataServiceContext.UsePostTunneling%2A> 实例上的 <xref:System.Data.Services.Client.DataServiceContext> 属性设置为 `true`。  
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
- [更新数据服务](updating-the-data-service-wcf-data-services.md)
- [异步操作](asynchronous-operations-wcf-data-services.md)
- [批处理操作](batching-operations-wcf-data-services.md)
