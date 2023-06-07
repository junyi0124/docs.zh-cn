---
title: 源自定义（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, feeds
- WCF Data Services, Atom protocol
- Atom Publishing Protocol [WCF Data Services]
- WCF Data Services, customizing feeds
ms.assetid: 0d1a39bc-6462-4683-bd7d-e74e0fd28a85
ms.openlocfilehash: bf25737d98a880ac720318de18b687b4df5b0708
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172627"
---
# <a name="feed-customization-wcf-data-services"></a>源自定义（WCF 数据服务）

WCF 数据服务使用 Open Data Protocol (OData) 将数据公开为源。 OData 支持用于数据馈送的 Atom 和 JavaScript 对象表示法 (JSON) 格式。 使用 Atom 馈送时，OData 提供一种标准方法，用于将数据（如实体和关系）序列化为可包含在 HTTP 消息正文中的 XML 格式。 OData 定义实体中包含的数据与 Atom 元素之间的默认实体属性映射。 有关详细信息，请参阅 [OData： Atom 格式](https://www.odata.org/documentation/odata-version-2-0/atom-format/)。  
  
 您的应用程序方案可能要求以自定义方式而非标准源格式序列化数据服务返回的属性数据。 使用 OData，你可以在数据馈送中自定义序列化，以便可以将实体的属性映射到某个条目的未使用的元素和属性，或者映射到源中条目的自定义元素。  
  
> [!NOTE]
> 只有 Atom 馈送才支持源自定义。 当为返回的源请求 JSON 格式时，不会返回自定义源。  
  
 使用 WCF 数据服务，可以通过手动将属性应用于数据模型中的实体类型，为 Atom 有效负载定义备用实体属性映射。 数据服务的数据源提供程序确定应当如何应用这些特性。  
  
> [!IMPORTANT]
> 定义自定义源时，必须保证已定义自定义映射的所有实体属性都包含在投影中。 如果投影中不包含某个已映射的实体属性，则可能会丢失数据。 有关详细信息，请参阅 [查询投影](query-projections-wcf-data-services.md)。  
  
## <a name="customizing-feeds-with-the-entity-framework-provider"></a>使用实体框架提供程序自定义源  

 在 .edmx 文件中，用于实体框架提供程序的数据模型以 XML 格式表示。 在这种情况下，用于定义自定义源的特性将会添加到表示数据模型中的实体类型和属性的 `EntityType` 和 `Property` 元素中。 这些源自定义属性未定义为[ \[ MC-CSDL \] ：概念架构定义文件格式](/openspecs/windows_protocols/mc-csdl/c03ad8c3-e8b7-4306-af96-a9e52bb3df12)，这是实体框架提供程序用于定义数据模型的格式。 因此，必须在某个特定的架构命名空间中声明源自定义特性，此架构命名空间应按 `m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"` 格式定义。 以下 XML 片段显示应用于 `Property` 实体类型的 `Products` 元素的源自定义特性，这些特性定义 `ProductName`、`ReorderLevel` 和 `UnitsInStock` 属性。  
  
 [!code-xml[Astoria Custom Feeds#EdmFeedAttributes](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_custom_feeds/xml/northwind.csdl#edmfeedattributes)]  
  
 这些特性为 `Products` 实体集生成以下自定义数据源。 在自定义数据源中，`ProductName` 属性值同时显示在 `author` 元素以及 `ProductName` 属性元素中，`UnitsInStock` 属性显示在具有其自己的唯一命名空间并且其 `ReorderLevel` 属性作为特性的自定义元素中：  
  
 [!code-xml[Astoria Custom Feeds#EdmFeedResultProduct](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_custom_feeds/xml/edmfeedresult.xml#edmfeedresultproduct)]  
  
 有关详细信息，请参阅 [如何：利用实体框架提供程序自定义源](how-to-customize-feeds-with-ef-provider-wcf-data-services.md)。  
  
> [!NOTE]
> 由于实体设计器不支持数据模型扩展，因此必须手动修改包含数据模型的 XML 文件。 有关实体数据模型工具生成的 .edmx 文件的详细信息，请参阅实体框架) 中的 [.Edmx 文件概述 (](/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))。  
  
### <a name="custom-feed-attributes"></a>自定义源特性  

 下表列出了一些 XML 特性，这些特性自定义可添加到用于定义数据模型的概念架构定义语言 (CSDL) 的源。 这些特性等效于用于反射提供程序的 <xref:System.Data.Services.Common.EntityPropertyMappingAttribute> 的属性。  
  
|属性名称|描述|  
|--------------------|-----------------|  
|`FC_ContentKind`|指示内容类型。 以下关键字定义联合内容类型。<br /><br /> `text:` 属性值在源中显示为文本。<br /><br /> `html:` 属性值在源中显示为 HTML。<br /><br /> `xhtml:` 属性值在源中显示为 XML 格式的 HTML。<br /><br /> 这些关键字等效于用于反射提供程序的 <xref:System.Data.Services.Common.SyndicationTextContentKind> 枚举的值。<br /><br /> 如果使用了 `FC_NsPrefix` 和 `FC_NsUri` 特性，则不支持此特性。<br /><br /> 为 `xhtml` 特性指定值 `FC_ContentKind` 时，必须确保该属性值包含格式正确的 XML。 数据服务返回值，但不执行任何转换。 还必须确保返回的 XML 中任何 XML 元素前缀在映射的源中定义了命名空间 URI 和前缀。|  
|`FC_KeepInContent`|指示是否应将引用的属性值包含到源的内容部分和映射的位置中。 有效值为 `true` 和 `false`。 若要使结果源与 WCF 数据服务早期版本向后兼容，请将值指定 `true` 为以确保该值包含在源的内容部分中。|  
|`FC_NsPrefix`|非联合映射中的 XML 元素的命名空间前缀。 此特性必须与 `FC_NsUri` 特性一起使用，但不能与 `FC_ContentKind` 特性一起使用。|  
|`FC_NsUri`|非联合映射中的 XML 元素的命名空间 URI。 此特性必须与 `FC_NsPrefix` 特性一起使用，但不能与 `FC_ContentKind` 特性一起使用。|  
|`FC_SourcePath`|应用此源映射规则的实体属性的路径。 仅当在 `EntityType` 元素中使用时，才支持此特性。<br /><br /> <xref:System.Data.Services.Common.EntityPropertyMappingAttribute.SourcePath%2A> 属性无法直接引用复杂类型。 对于复杂类型，必须使用路径表达式，在此路径表达式中，使用反斜杠 (`/`) 字符分隔属性名称。 例如，对于 `Person` 具有整数属性 `Age` 和复杂属性的实体类型，允许使用以下值：<br /><br /> `Address`:<br /><br /> `Age`<br /><br /> `Address/Street`<br /><br /> 不能将 <xref:System.Data.Services.Common.EntityPropertyMappingAttribute.SourcePath%2A> 属性设置为包含空格或在属性名称中无效的任何其他字符的值。|  
|`FC_TargetPath`|属性将映射到的结果源中的目标元素的名称。 此元素可以是按 Atom 规范定义的元素，也可以是自定义元素。<br /><br /> 以下关键字是指向 OData 源中特定位置的预定义联合目标路径值。<br /><br /> `SyndicationAuthorEmail:``atom:email`元素的子元素 `atom:author` 。<br /><br /> `SyndicationAuthorName:``atom:name`元素的子元素 `atom:author` 。<br /><br /> `SyndicationAuthorUri:``atom:uri`元素的子元素 `atom:author` 。<br /><br /> `SyndicationContributorEmail:``atom:email`元素的子元素 `atom:contributor` 。<br /><br /> `SyndicationContributorName:``atom:name`元素的子元素 `atom:contributor` 。<br /><br /> `SyndicationContributorUri:``atom:uri`元素的子元素 `atom:contributor` 。<br /><br /> `SyndicationCustomProperty:` 自定义属性元素。 映射到自定义元素时，目标必须是路径表达式，在此路径表达式中，使用反斜杠 (`/`) 分隔嵌套元素，并使用 `@` 指定特性。 在以下示例中，字符串 `UnitsInStock/@ReorderLevel` 将属性值映射到根项元素的 `ReorderLevel` 子元素上的 `UnitsInStock` 特性。<br /><br /> `<Property Name="ReorderLevel" Type="Int16"               m:FC_TargetPath="UnitsInStock/@ReorderLevel"               m:FC_NsPrefix="Northwind"               m:FC_NsUri="http://schemas.examples.microsoft.com/dataservices"               m:FC_KeepInContent="false"               />`<br /><br /> 如果目标是自定义元素名称，还必须指定 `FC_NsPrefix` 和 `FC_NsUri` 特性。<br /><br /> `SyndicationPublished:``atom:published`元素。<br /><br /> `SyndicationRights:``atom:rights`元素。<br /><br /> `SyndicationSummary:``atom:summary`元素。<br /><br /> `SyndicationTitle:``atom:title`元素。<br /><br /> `SyndicationUpdated:``atom:updated`元素。<br /><br /> 这些关键字等效于用于反射提供程序的 <xref:System.Data.Services.Common.SyndicationItemProperty> 枚举的值。|  
  
> [!NOTE]
> 特性名称和值是区分大小写的。 特性可以应用于 `EntityType` 元素，也可以应用于一个或多个 `Property` 元素，但不能同时应用于二者。  
  
## <a name="customizing-feeds-with-the-reflection-provider"></a>使用反射提供程序自定义源  

 若要为使用反射提供程序实现的数据模型自定义源，请将 <xref:System.Data.Services.Common.EntityPropertyMappingAttribute> 特性的一个或多个实例添加到表示数据模型中的实体类型的类中。 <xref:System.Data.Services.Common.EntityPropertyMappingAttribute> 类的属性对应于在上一节中介绍的源自定义特性。 以下是 `Order` 类型的声明示例，该示例为两个属性定义了自定义源映射。  
  
> [!NOTE]
> 主题 [如何：使用反射提供程序创建数据服务](create-a-data-service-using-rp-wcf-data-services.md)中定义了此示例的数据模型。  
  
 [!code-csharp[Astoria Custom Feeds#CustomOrderFeed](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_custom_feeds/cs/orderitems.svc.cs#customorderfeed)]
 [!code-vb[Astoria Custom Feeds#CustomOrderFeed](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_custom_feeds/vb/orderitems.svc.vb#customorderfeed)]  
  
 这些特性为 `Orders` 实体集生成以下自定义数据源。 在此自定义源中， `OrderId` 属性值仅显示在 `title` 的元素中， `entry` `Customer` 属性值同时显示在 `author` 元素和 `Customer` 属性元素中：  
  
 [!code-xml[Astoria Custom Feeds#IQueryableFeedResult](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_custom_feeds/xml/iqueryablefeedresult.xml#iqueryablefeedresult)]  
  
 有关详细信息，请参阅 [如何：用反射提供程序自定义源](how-to-customize-feeds-with-the-reflection-provider-wcf-data-services.md)。  
  
## <a name="customizing-feeds-with-a-custom-data-service-provider"></a>使用自定义数据服务提供程序自定义源  

 通过对表示数据模型中实体类型的 <xref:System.Data.Services.Providers.ResourceType.AddEntityPropertyMappingAttribute%2A> 调用 <xref:System.Data.Services.Providers.ResourceType>，可以为资源类型定义通过使用自定义数据服务提供程序定义的数据模型的源自定义。 有关详细信息，请参阅 [自定义数据服务提供程序](custom-data-service-providers-wcf-data-services.md)。  
  
## <a name="consuming-custom-feeds"></a>使用自定义源  

 当应用程序直接使用 OData 源时，它必须能够处理返回的源中的任何自定义元素和特性。 如果已在数据模型中实现自定义源，不管数据服务提供程序如何，`$metadata` 终结点都会返回自定义源信息，并作为数据服务返回的 CSDL 中的自定义源特性。 使用 **添加服务引用** 对话框或 [datasvcutil.exe](wcf-data-service-client-utility-datasvcutil-exe.md) 工具生成客户端数据服务类时，将使用自定义的源属性来保证正确处理对数据服务的请求和响应。  
  
## <a name="feed-customization-considerations"></a>源自定义注意事项  

 定义自定义源映射时，应考虑下列事项。  
  
- 当源中的映射元素仅包含空白时，WCF 数据服务客户端将源中的映射元素视为空。 因此，仅包含空白的映射元素在具有相同空白的客户端上不具体化。 若要在客户端上保留此空格，必须 `KeepInContext` `true` 在 "源映射" 属性中将的值设置为。  
  
## <a name="versioning-requirements"></a>版本控制要求  

 源自定义具有以下 OData 协议版本要求：  
  
- 源自定义要求客户端和数据服务都支持 OData 协议版本2.0 和更高版本。  
  
 有关详细信息，请参阅 [数据服务版本控制](data-service-versioning-wcf-data-services.md)。  
  
## <a name="see-also"></a>请参阅

- [反射提供程序](reflection-provider-wcf-data-services.md)
- [实体框架提供程序](entity-framework-provider-wcf-data-services.md)
