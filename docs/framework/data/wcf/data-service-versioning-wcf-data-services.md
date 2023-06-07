---
title: 数据服务版本管理（WCF 数据服务）
ms.date: 03/30/2017
helpviewer_keywords:
- versioning, WCF Data Services
- versioning [WCF Data Services]
- WCF Data Services, versioning
ms.assetid: e3e899cc-7f25-4f67-958f-063f01f79766
ms.openlocfilehash: 8d7cc0f0033c75c05ac9c39cfbf1ce09dc032a4c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91182865"
---
# <a name="data-service-versioning-wcf-data-services"></a>数据服务版本管理（WCF 数据服务）

利用 Open Data Protocol (OData) ，你可以创建数据服务，以便客户端可以使用基于数据模型的 Uri 作为资源访问数据。 OData 还支持服务操作的定义。 这些数据服务在初始部署之后，可能出于多种原因（例如，更改业务需求、信息技术需求，或者为了解决其他问题）而需要更改，并且在其生存期期间可能需要更改多次。 更改现有数据服务时，您必须考虑是否要定义您的数据服务的新版本以及如何将对现有客户端应用程序的影响降至最低。 本主题提供了有关何时以及如何创建一个新版本的数据服务的指导。 它还介绍了 WCF 数据服务如何处理支持不同版本 OData 协议的客户端和数据服务之间的交换。

## <a name="versioning-a-wcf-data-service"></a>WCF 数据服务版本管理

 一旦部署数据服务和使用数据，对数据服务的更改有可能导致与现有客户端应用程序的兼容性问题。 但是，因为服务的整体业务需求而经常需要进行更改，您必须考虑何时以及如何创建一个新的数据服务版本，以最大程度减小对客户端上应用程序的影响。

### <a name="data-model-changes-that-recommend-a-new-data-service-version"></a>建议发布新的数据服务版本的数据模型更改

 在考虑是否要发布数据服务的新版本时，了解不同种类的更改如何影响客户端应用程序很重要。 可能要求您创建一个新版本的数据服务的数据服务更改可分为以下两个类别：

- 对服务协定的更改 — 它包括对服务操作的更新、对实体集（源）的可访问性的更改或对服务行为的其他更改。

- 对数据协定的更改 — 它包括对数据模型、源格式或源自定义的更改。

 下表详细说明出现哪类更改时您应考虑发布新版本的数据服务：

|更改类型|需要新版本|不需要新版本|
|--------------------|----------------------------|----------------------------|
|服务操作|-添加新参数<br />-更改返回类型<br />-删除服务操作|-删除现有参数<br />-添加新服务操作|
|服务行为|-Disable 请求计数<br />-禁用投影支持<br />-增加所需的数据服务版本|-启用计数请求<br />-启用投影支持<br />-减少所需的数据服务版本|
|实体集权限|-限制实体集权限<br />-更改响应代码 (新的第一个数字值) <sup>1</sup>|-放宽实体集权限<br />-更改响应代码 (相同的第一个数字值) |
|实体属性|-删除现有的属性或关系<br />-添加不可为 null 的属性<br />-更改现有属性|-添加可以为 null 的属性<sup>2</sup>|
|实体集|-删除实体集|-添加派生类型<br />-更改基类型<br />-添加实体集|
|源自定义|-更改实体-属性映射||

 <sup>1</sup> 这可能取决于客户端应用程序在接收特定错误代码时的严格程度。

 <sup>2</sup> 可以将属性设置 <xref:System.Data.Services.Client.DataServiceContext.IgnoreMissingProperties%2A> 为 `true` ，使客户端忽略数据服务发送的、未在客户端上定义的任何新属性。 但是，当插入时，客户端在 POST 请求中不包括的属性都设置为其默认值。 对于更新，对客户端未知的属性中任何现有数据可能用默认值改写。 在此情况下，应将更新作为 MERGE 要求发送，这是默认值。 有关详细信息，请参阅 [管理数据服务上下文](managing-the-data-service-context-wcf-data-services.md)。

### <a name="how-to-version-a-data-service"></a>如何对数据服务进行版本管理

 在需要时，通过创建新服务实例（使用更新的服务协定或数据模型）定义新的数据服务版本。 通过使用新的 URI 终结点（使其与以前的版本不同）公开该新服务。 例如：

- 旧版本：`http://services.odata.org/Northwind/v1/Northwind.svc/`

- 新版本：`http://services.odata.org/Northwind/v2/Northwind.svc/`

 升级数据服务时，客户端将需要也更新（基于新的数据服务的元数据）并使用新的根 URI。 如果可能，您应维护以前版本的数据服务以支持尚未升级、无法使用新版本的客户端。 在不再需要时，可以删除旧版本的数据服务。 您应该考虑在外部配置文件中维护数据服务终结点 URI。

## <a name="odata-protocol-versions"></a>OData 协议版本

 新版本的 OData 发布后，客户端应用程序可能无法使用数据服务所支持的 OData 协议版本。 较旧的客户端应用程序可以访问支持更高版本 OData 的数据服务。 客户端应用程序也可能使用较新版本的 WCF 数据服务客户端库，该版本支持 OData 的较新版本，而不是正在访问的数据服务。

 WCF 数据服务利用 OData 提供的支持来处理此类版本控制方案。 当客户端使用的 OData 版本不同于数据服务所使用的 OData 版本时，还支持生成和使用数据模型元数据来创建客户端数据服务类。 有关详细信息，请参阅 [OData：概述](https://www.odata.org/documentation/odata-version-2-0/overview/) 一文中的协议版本控制部分。

### <a name="version-negotiation"></a>版本协商

 数据服务可配置为定义服务将使用的 OData 协议的最高版本，而与客户端请求的版本无关。 为此，可以 <xref:System.Data.Services.Common.DataServiceProtocolVersion> 为 <xref:System.Data.Services.DataServiceBehavior.MaxProtocolVersion%2A> 数据服务所用的的属性指定一个值 <xref:System.Data.Services.DataServiceBehavior> 。 有关详细信息，请参阅 [配置数据服务](configuring-the-data-service-wcf-data-services.md)。

 当应用程序使用 WCF 数据服务的客户端库访问数据服务时，库会自动将这些标头设置为正确的值，具体取决于 OData 的版本以及应用程序中使用的功能。 默认情况下，WCF 数据服务使用支持请求的操作的最低协议版本。

 下表详细说明了包含特定版本 OData 协议的 WCF 数据服务支持的 .NET Framework 和 Silverlight 版本。

|OData 协议版本|支持引入方式|
|-----------------------------------------------------------------------------------|----------------------------|
|版本 1|-.NET Framework 3.5 Service Pack 1 (SP1) <br />-Silverlight 版本3|
|版本 2|-.NET Framework 4<br />-Silverlight 版本4|

### <a name="metadata-versions"></a>元数据版本

 默认情况下，WCF 数据服务使用 CSDL 版本1.1 来表示数据模型。 对于基于反射提供程序或自定义数据服务提供程序的数据模型始终如此。 但是，如果数据模型是使用实体框架定义的，则返回的 CSDL 版本就是该实体框架所用的版本。 CSDL 的版本由 [Schema 元素 () ](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#schema-element-csdl)的命名空间确定。

 返回的元数据的 `DataServices` 元素还包含一个 `DataServiceVersion` 特性，该特性的值与响应消息中 `DataServiceVersion` 标头的值相同。 客户端应用程序（如 Visual Studio 中的 " **添加服务引用** " 对话框）使用此信息来生成客户端数据服务类，这些类与承载数据服务的 WCF 数据服务的版本一起正确工作。 有关详细信息，请参阅 [OData：概述](https://www.odata.org/documentation/odata-version-2-0/overview/) 一文中的协议版本控制部分。

## <a name="see-also"></a>请参阅

- [数据服务提供程序](data-services-providers-wcf-data-services.md)
- [定义 WCF 数据服务](defining-wcf-data-services.md)
