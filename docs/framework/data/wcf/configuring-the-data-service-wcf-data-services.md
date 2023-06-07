---
title: 配置数据服务（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, configuring
ms.assetid: 59efd4c8-cc7a-4800-a0a4-d3f8abe6c55c
ms.openlocfilehash: a30a8c2c731e8c5cb2b22c8d7f34ec32d149803c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91152788"
---
# <a name="configuring-the-data-service-wcf-data-services"></a>配置数据服务（WCF 数据服务）

通过 WCF 数据服务，您可以创建 (OData) 源公开 Open Data Protocol 数据服务。 这些源中的数据可以来自各种数据源。 WCF 数据服务使用数据提供程序将此数据作为 OData 源公开。 这些提供程序包括实体框架提供程序、反射提供程序和一组自定义数据服务提供程序接口。 提供程序实现为服务定义数据模型。 有关详细信息，请参阅 [数据服务提供程序](data-services-providers-wcf-data-services.md)。  
  
 在 WCF 数据服务中，数据服务是从类继承的类 <xref:System.Data.Services.DataService%601> ，其中数据服务的类型是数据模型的实体容器。 此实体容器具有一个或多个属性，这些属性返回用于访问数据模型中的实体集的 <xref:System.Linq.IQueryable%601>。  
  
 数据服务的行为是由 <xref:System.Data.Services.DataServiceConfiguration> 类的成员以及 <xref:System.Data.Services.DataServiceBehavior> 类的成员定义的，可从 <xref:System.Data.Services.DataServiceConfiguration.DataServiceBehavior%2A> 类的 <xref:System.Data.Services.DataServiceConfiguration> 属性访问这些行为。 <xref:System.Data.Services.DataServiceConfiguration> 类提供给由数据服务实现的 `InitializeService` 方法，如下面的罗斯文数据服务的实现所示：  
  
[!code-csharp[Astoria Northwind Service#DataServiceConfigComplete](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_service/cs/northwind.svc.cs#dataserviceconfigcomplete)]  
[!code-vb[Astoria Northwind Service#DataServiceConfigComplete](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_service/vb/northwind.svc.vb#dataserviceconfigcomplete)]  
  
## <a name="data-service-configuration-settings"></a>数据服务配置设置  

 <xref:System.Data.Services.DataServiceConfiguration> 类用于指定下列数据服务行为：  
  
|成员|行为|  
|------------|--------------|  
|<xref:System.Data.Services.DataServiceBehavior.AcceptCountRequests%2A>|用于禁用通过使用 `$count` 路径段和 `$inlinecount` 查询选项提交给数据服务的计数请求。 有关详细信息，请参阅 [OData： URI 约定](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)。|  
|<xref:System.Data.Services.DataServiceBehavior.AcceptProjectionRequests%2A>|用于在通过使用 `$select` 查询选项提交给数据服务的请求中禁用数据投影支持。 有关详细信息，请参阅 [OData： URI 约定](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)。|  
|<xref:System.Data.Services.DataServiceConfiguration.EnableTypeAccess%2A>|用于在通过使用 <xref:System.Data.Services.Providers.IDataServiceMetadataProvider> 接口所定义的动态元数据提供程序的元数据中公开数据类型。|  
|<xref:System.Data.Services.DataServiceConfiguration.EnableTypeConversion%2A>|用于指定数据服务运行时是否应将负载中包含的类型转换为请求中指定的实际属性类型。|  
|<xref:System.Data.Services.DataServiceBehavior.InvokeInterceptorsOnLinkDelete%2A>|用于指定在删除两个实体之间的关系链接时是否对相关实体调用注册的变更侦听器。|  
|<xref:System.Data.Services.DataServiceConfiguration.MaxBatchCount%2A>|用于限制单个批处理中允许的变更集和查询操作的数量。 有关详细信息，请参阅 [OData：批处理](https://www.odata.org/documentation/odata-version-2-0/batch-processing/) 和 [批处理操作](batching-operations-wcf-data-services.md)。|  
|<xref:System.Data.Services.DataServiceConfiguration.MaxChangesetCount%2A>|用于限制单个变更集中可包含的变更的数量。 有关详细信息，请参阅 [如何：启用数据服务结果分页](how-to-enable-paging-of-data-service-results-wcf-data-services.md)。|  
|<xref:System.Data.Services.DataServiceConfiguration.MaxExpandCount%2A>|用于限制响应的大小，方法是使用 `$expand` 查询运算符限制单个请求中可包含的相关实体的数量。 有关详细信息，请参阅 [OData： URI 约定](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/) 和 [加载延迟的内容](loading-deferred-content-wcf-data-services.md)。|  
|<xref:System.Data.Services.DataServiceConfiguration.MaxExpandDepth%2A>|用于限制响应的大小，方法是使用 `$expand` 查询运算符限制单个请求中可包含的相关实体的图深度。 有关详细信息，请参阅 [OData： URI 约定](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/) 和 [加载延迟的内容](loading-deferred-content-wcf-data-services.md)。|  
|<xref:System.Data.Services.DataServiceConfiguration.MaxObjectCountOnInsert%2A>|用于限制要插入的（即单个 POST 请求中可包含的）实体数量。|  
|<xref:System.Data.Services.DataServiceBehavior.MaxProtocolVersion%2A>|定义数据服务所使用的 Atom 协议的版本。 当的值 <xref:System.Data.Services.DataServiceBehavior.MaxProtocolVersion%2A> 设置为小于的最大值时 <xref:System.Data.Services.Common.DataServiceProtocolVersion> ，WCF 数据服务的最新功能对于访问数据服务的客户端不可用。 有关详细信息，请参阅 [数据服务版本控制](data-service-versioning-wcf-data-services.md)。|  
|<xref:System.Data.Services.DataServiceConfiguration.MaxResultsPerCollection%2A>|用于限制响应的大小，方法是限制以数据馈送形式返回的每个实体集中实体的数量。|  
|<xref:System.Data.Services.DataServiceConfiguration.RegisterKnownType%2A>|向可由数据服务识别的类型列表中添加数据类型。|  
|<xref:System.Data.Services.DataServiceConfiguration.SetEntitySetAccessRule%2A>|为可用于数据服务的实体集资源设置访问权限。 通过为名称参数提供星号值 (`*`)，可将对其余所有实体集的访问权限设置为同一级别。 建议您将对实体集的访问权限设置为能够提供客户端应用程序访问数据服务资源所需的最低访问权限。 有关更多信息，请参见 [Securing WCF Data Services](securing-wcf-data-services.md)。 有关给定 URI 和 HTTP 操作所需的最小访问权限的示例，请参阅 [最小资源访问要求](configuring-the-data-service-wcf-data-services.md#accessRequirements) 部分中的表。|  
|<xref:System.Data.Services.DataServiceConfiguration.SetEntitySetPageSize%2A>|为实体集资源设置最大页面大小。 有关详细信息，请参阅 [如何：启用数据服务结果分页](how-to-enable-paging-of-data-service-results-wcf-data-services.md)。|  
|<xref:System.Data.Services.DataServiceConfiguration.SetServiceOperationAccessRule%2A>|为在数据服务中定义的服务操作设置访问权限。 有关详细信息，请参阅 [服务操作](service-operations-wcf-data-services.md)。 通过为名称参数提供星号值 (`*`)，可将对所有服务操作的访问权限设置为同一级别。 建议您将对服务操作的访问权限设置为能够提供客户端应用程序访问数据服务资源所需的最低访问权限。 有关更多信息，请参见 [Securing WCF Data Services](securing-wcf-data-services.md)。|  
|<xref:System.Data.Services.DataServiceConfiguration.UseVerboseErrors%2A>|使用此配置属性可以在错误响应消息中返回更多信息，从而使您可以更轻松地对数据服务进行故障排除。 此选项不适用于生产环境。 有关详细信息，请参阅 [开发和部署 WCF 数据服务](developing-and-deploying-wcf-data-services.md)。|  
  
<a name="accessRequirements"></a>

## <a name="minimum-resource-access-requirements"></a>最低资源访问要求  

 下表详细描述了为执行特定操作而必须授予的最低实体集权限。 路径示例基于在完成 [快速入门](quickstart-wcf-data-services.md)时创建的 Northwind 数据服务。 由于 <xref:System.Data.Services.EntitySetRights> 枚举和 <xref:System.Data.Services.ServiceOperationRights> 枚举都是通过使用 <xref:System.FlagsAttribute> 定义的，因此您可以使用逻辑 OR 运算符来针对单个实体集或操作指定多个权限。 有关详细信息，请参阅 [如何：启用对数据服务的访问](how-to-enable-access-to-the-data-service-wcf-data-services.md)。  
  
|路径/操作|`GET`|`DELETE`|`MERGE`|`POST`|`PUT`|  
|------------------|-----------|--------------|-------------|------------|-----------|  
|`/Customers`|<xref:System.Data.Services.EntitySetRights.ReadMultiple>|不支持|不支持|<xref:System.Data.Services.EntitySetRights.WriteAppend>|不支持|  
|`/Customers('ALFKI')`|<xref:System.Data.Services.EntitySetRights.ReadSingle>|<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteDelete>|<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge>|不适用|<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteReplace>|  
|`/Customers('ALFKI')/Orders`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadMultiple>|不支持|不支持|`Customers`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge> 或 <xref:System.Data.Services.EntitySetRights.WriteReplace><br /><br /> －和－<br /><br /> `Orders` `:` 和 <xref:System.Data.Services.EntitySetRights.WriteAppend>|不支持|  
|`/Customers('ALFKI')/Orders(10643)`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteDelete>|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge>|不支持|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteReplace>|  
|`/Orders(10643)/Customer`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|`Customers`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteDelete><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|`Customers`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge>；<br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|`Customers`: <xref:System.Data.Services.EntitySetRights.WriteAppend><br /><br /> －和－<br /><br /> `Orders`：<xref:System.Data.Services.EntitySetRights.WriteAppend> 和 <xref:System.Data.Services.EntitySetRights.ReadSingle>|不支持|  
|`/Customers('ALFKI')/$links/Orders`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadMultiple>|不支持|不支持|`Customers`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge> 或 <xref:System.Data.Services.EntitySetRights.WriteReplace><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|不支持|  
|`/Customers('ALFKI')/$links/Orders(10643)`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|`Customers`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge> 或 <xref:System.Data.Services.EntitySetRights.WriteReplace><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|不支持|不支持|不支持|  
|`/Orders(10643)/$links/Customer`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadSingle>|`Orders`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge> 或 <xref:System.Data.Services.EntitySetRights.WriteReplace>|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteMerge>|不支持|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle>;<br /><br /> －和－<br /><br /> `Orders`：<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteReplace>|  
|`/Customers/$count`|<xref:System.Data.Services.EntitySetRights.ReadMultiple>|不支持|不支持|不支持|不支持|  
|`/Customers('ALFKI')/ContactName`|<xref:System.Data.Services.EntitySetRights.ReadSingle>|不支持|<xref:System.Data.Services.EntitySetRights.WriteMerge>|不支持|<xref:System.Data.Services.EntitySetRights.WriteReplace>|  
|`/Customers('ALFKI')/Address/StreetAddress/$value`<sup>1</sup>|<xref:System.Data.Services.EntitySetRights.ReadSingle>|<xref:System.Data.Services.EntitySetRights.WriteDelete>|不支持|不支持|不支持|  
|`/Customers('ALFKI')/ContactName/$value`|<xref:System.Data.Services.EntitySetRights.ReadSingle>|<xref:System.Data.Services.EntitySetRights.ReadSingle> 和 <xref:System.Data.Services.EntitySetRights.WriteDelete>|<xref:System.Data.Services.EntitySetRights.WriteMerge>|不支持|<xref:System.Data.Services.EntitySetRights.WriteReplace>|  
|`/Customers('ALFKI')/$value` <sup>2</sup>|<xref:System.Data.Services.EntitySetRights.ReadSingle>|不支持|不支持|不支持|<xref:System.Data.Services.EntitySetRights.WriteReplace>|  
|`/Customers?$select=Orders/*&$expand=Orders`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadMultiple>|不支持|不支持|`Customers`: <xref:System.Data.Services.EntitySetRights.WriteAppend>|不支持|  
|`/Customers('ALFKI')?$select=Orders/*&$expand=Orders`|`Customers`: <xref:System.Data.Services.EntitySetRights.ReadSingle><br /><br /> －和－<br /><br /> `Orders`: <xref:System.Data.Services.EntitySetRights.ReadMultiple>|不支持|不支持|不支持|不支持|  
  
 <sup>1</sup> 在此示例中， `Address` 表示 `Customers` 实体的一个具有名为的属性的复杂类型属性 `StreetAddress` 。 罗斯文数据服务所使用的模型不会显式定义此复杂类型。 如果数据模型是使用实体框架提供程序定义的，则可以使用实体数据模型工具来定义这样的复杂类型。 有关详细信息，请参阅 [如何：创建和修改复杂类型](/previous-versions/dotnet/netframework-4.0/dd456820(v=vs.100))。  
  
 <sup>2</sup> 在将 (BLOB) 返回二进制大型对象的属性定义为属于媒体链接项的实体（在本例中为）时，此 URI 受支持 `Customers` 。 有关详细信息，请参阅 [流式处理提供程序](streaming-provider-wcf-data-services.md)。  
  
<a name="versioning"></a>

## <a name="versioning-requirements"></a>版本控制要求  

 以下数据服务配置行为需要 OData 协议版本2或更高版本：  
  
- 计数请求支持。  
  
- 投影的 $select 查询选项支持。  
  
 有关详细信息，请参阅 [数据服务版本控制](data-service-versioning-wcf-data-services.md)。  
  
## <a name="see-also"></a>请参阅

- [定义 WCF 数据服务](defining-wcf-data-services.md)
- [承载数据服务](hosting-the-data-service-wcf-data-services.md)
