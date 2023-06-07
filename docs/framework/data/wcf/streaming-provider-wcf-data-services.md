---
title: 流提供程序（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, providers
- WCF Data Services, binary data
- streaming data provider [WCF Data Services]
- WCF Data Services, streams
ms.assetid: f0978fe4-5f9f-42aa-a5c2-df395d7c9495
ms.openlocfilehash: 9ed728fa8d1d56c835aa27645a28921aa4f641e9
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90544448"
---
# <a name="streaming-provider-wcf-data-services"></a>流提供程序（WCF 数据服务）

数据服务可公开二进制大型对象数据。 此二进制数据可以表示视频和音频流、图像、文档文件或其他类型的二进制媒体。 当数据模型中的某个实体包括一个或多个二进制属性时，数据服务会在响应源的入口内以 base-64 编码形式返回此二进制数据。 由于以这种方式加载和序列化大型二进制数据会影响性能，因此 Open Data Protocol (OData) 定义了一种机制，该机制独立于其所属的实体来检索二进制数据。 这一点是通过将实体和二进制数据分隔到一个或多个数据流来实现的。

- 媒体资源-属于某个实体的二进制数据，例如视频、音频、图像或其他类型的媒体资源流。

- 媒体链接入口 - 引用相关媒体资源流的实体。

使用 WCF 数据服务，可以通过实现流式数据提供程序来定义二进制资源流。 流提供程序实现以对象的形式向数据服务提供与特定实体关联的媒体资源流 <xref:System.IO.Stream> 。 有了此实现，数据服务能够以指定 MIME 类型的二进制数据流的形式通过 HTTP 接受和返回媒体资源。

将数据服务配置为支持二进制数据流需要以下步骤：

1. 将数据模型中的一个或多个实体特性化为媒体链接入口。 这些实体不应包括要进行流处理的二进制数据。 始终在实体中以 base-64 编码的二进制形式返回实体的所有二进制属性。

2. 实现 T:System.Data.Services.Providers.IDataServiceStreamProvider 接口。

3. 定义一个实现 <xref:System.IServiceProvider> 接口的数据服务。 数据服务使用 <xref:System.IServiceProvider.GetService%2A> 实现访问流数据提供程序实现。 此方法返回适当的流提供程序实现。

4. 在 Web 应用程序配置中启用大型消息流。

5. 启用对服务器上或数据源中的二进制资源的访问。

本主题中的示例基于示例流照片服务，该服务将在发布 [数据服务流提供程序系列：实现流提供程序 (第1部分) ](/archive/blogs/astoriateam/data-services-streaming-provider-series-implementing-a-streaming-provider-part-1)中深入讨论。 [GitHub](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/Streaming%20Photo%20OData%20Service%20Sample)上提供了流式处理照片数据服务示例的源代码。

## <a name="defining-a-media-link-entry-in-the-data-model"></a>在数据模型中定义媒体链接入口

数据源提供程序确定在数据模型中将某个实体定义为媒体链接入口的方式。

**实体框架提供程序**

若要指示某个实体为媒体链接入口，需将 `HasStream` 特性添加到概念模型中的相应实体类型定义，如以下示例所示：

[!code-xml[Astoria Photo Streaming Service#HasStream](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_photo_streaming_service/xml/photodata.edmx#hasstream)]

还必须将命名空间 `xmlns:m=http://schemas.microsoft.com/ado/2007/08/dataservices/metadata` 添加到实体，或添加到定义数据模型的 .edmx 或 .csdl 文件的根目录中。

有关使用实体框架提供程序并公开媒体资源的数据服务的示例，请参阅文章 [数据服务流提供程序系列：实现流提供程序 (第1部分) ](/archive/blogs/astoriateam/data-services-streaming-provider-series-implementing-a-streaming-provider-part-1)。

**反射提供程序**

若要指示某个实体为媒体链接入口，需将 <xref:System.Data.Services.Common.HasStreamAttribute> 添加到在反射提供程序中定义相应实体类型的类中。

**自定义数据服务提供程序**

使用自定义服务提供程序时，可实现 <xref:System.Data.Services.Providers.IDataServiceMetadataProvider> 接口来定义数据服务的元数据。 有关详细信息，请参阅 [自定义数据服务提供程序](custom-data-service-providers-wcf-data-services.md)。 通过将表示实体类型（媒体链接入口）的 <xref:System.Data.Services.Providers.ResourceType> 的 <xref:System.Data.Services.Providers.ResourceType.IsMediaLinkEntry%2A> 属性设置为 `true`，可以指示一个二进制资源流属于 <xref:System.Data.Services.Providers.ResourceType>。

## <a name="implementing-the-idataservicestreamprovider-interface"></a>实现 IDataServiceStreamProvider 接口

若要创建支持二进制数据流的数据服务，必须实现 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 接口。 有了此实现，数据服务能够以流的形式将二进制数据返回给客户端，并使用从客户端发送的流形式的二进制数据。 每当数据服务需要访问流形式的二进制数据时，都会创建一个此接口实例。 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 接口指定以下成员：

|成员名称|说明|
|-----------------|-----------------|
|<xref:System.Data.Services.Providers.IDataServiceStreamProvider.DeleteStream%2A>|当删除媒体资源的媒体链接入口时，数据服务将调用此方法来删除相应媒体资源。 当实现 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 时，此方法将包含会删除与所提供媒体链接入口关联的媒体资源的代码。|
|<xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetReadStream%2A>|数据服务调用此方法来以流的形式返回媒体资源。 当实现 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 时，此方法将包含提供流的代码，数据服务使用提供的流返回与所提供媒体链接入口关联的媒体资源。|
|<xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetReadStreamUri%2A>|数据服务调用此方法，来返回用于请求媒体链接入口的媒体资源的 URI。 使用此值可以在媒体链接入口的内容元素中创建 `src` 特性，也可以请求数据流。 当此方法返回 `null` 时，数据服务自动确定 URI。 需要向客户端提供不使用流提供程序直接访问二进制数据的权限时，使用此方法。|
|<xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetStreamContentType%2A>|数据服务调用此方法，来返回与指定媒体链接入口关联的媒体资源的 Content-Type 值。|
|<xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetStreamETag%2A>|数据服务调用此方法，来返回与指定实体关联的数据流的 eTag。 管理二进制数据的并发性时使用此方法。 如果此方法返回 null，则数据服务不会跟踪并发。|
|<xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetWriteStream%2A>|数据服务调用此方法，来获取在接收从客户端发送的流时所使用的流。 当实现 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 时，您必须返回一个可写入流，以便数据服务将接收到的流数据写入到其中。|
|<xref:System.Data.Services.Providers.IDataServiceStreamProvider.ResolveType%2A>|返回一个命名空间限定的类型名称，该名称表示数据服务运行时必须为媒体链接项创建的类型，该媒体链接项与正在插入的媒体资源的数据流相关联。|

## <a name="creating-the-streaming-data-service"></a>创建流数据服务

若要为 WCF 数据服务运行时提供对实现的访问权限 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> ，则创建的数据服务还必须实现 <xref:System.IServiceProvider> 接口。 下面的示例演示如何实现 <xref:System.IServiceProvider.GetService%2A> 方法，以返回实现 `PhotoServiceStreamProvider` 的 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 类的实例。

[!code-csharp[Astoria Photo Streaming Service#PhotoServiceStreamingProvider](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_photo_streaming_service/cs/photodata.svc.cs#photoservicestreamingprovider)]
[!code-vb[Astoria Photo Streaming Service#PhotoServiceStreamingProvider](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_photo_streaming_service/vb/photodata.svc.vb#photoservicestreamingprovider)]

有关如何创建数据服务的一般信息，请参阅 [配置数据服务](configuring-the-data-service-wcf-data-services.md)。

## <a name="enabling-large-binary-streams-in-the-hosting-environment"></a>在宿主环境中启用大型二进制数据流

在 ASP.NET Web 应用程序中创建数据服务时，Windows Communication Foundation (WCF) 用于提供 HTTP 协议实现。 默认情况下，WCF 将 HTTP 消息的大小限制为仅 65 KB。 为了使大型二进制数据能够流入或流出数据服务，还必须将 Web 应用程序配置为启用大型二进制文件并使用流进行转换。 为此，请将以下内容添加到应用程序的 Web.config 文件的 `<configuration />` 元素中：

> [!NOTE]
> 必须使用 <xref:System.ServiceModel.TransferMode.Streamed?displayProperty=nameWithType> 传输模式来确保请求消息和响应消息中的二进制数据都经过流处理，并且不会被 WCF 缓冲。

有关详细信息，请参阅 [流式传输消息传输](../../wcf/feature-details/streaming-message-transfer.md) 和 [传输配额](../../wcf/feature-details/transport-quotas.md)。

默认情况下，Internet Information Services (IIS) 还会将请求的大小限制为 4 MB。 若要允许数据服务在 IIS 上运行时接收大于 4 MB 的流，还必须 `maxRequestLength` 在 "配置" 部分中将 "httpRuntime" (元素的属性设置为 " [ASP.NET Settings Schema) ](/previous-versions/dotnet/netframework-4.0/e1f13641(v=vs.100)) " `<system.web />` ，如以下示例中所示：

## <a name="using-data-streams-in-a-client-application"></a>在客户端应用程序中使用数据流

利用 WCF 数据服务客户端库，您可以在客户端上以二进制流的形式检索和更新这些公开的资源。 有关详细信息，请参阅使用 [二进制数据](working-with-binary-data-wcf-data-services.md)。

## <a name="considerations-for-working-with-a-streaming-provider"></a>使用流提供程序时的注意事项

以下是在实现流提供程序时以及从数据服务访问媒体资源时要考虑的一些事项。

- 对于媒体资源不支持 MERGE 请求。 使用 PUT 请求可更改现有实体的媒体资源。

- POST 请求不能用于创建新的媒体链接入口。 相反，您必须发出一个 POST 请求来创建新的媒体资源，而数据服务器则会使用默认值创建新的媒体链接入口。 这个新实体可由后续的 MERGE 或 PUT 请求进行更新。 您可能还应考虑缓存该实体并在处置器中进行更新，例如将属性值设置为 POST 请求中的 Slug 标头的值。

- 收到 POST 请求时，数据服务将先调用 <xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetWriteStream%2A> 来创建媒体资源，然后再调用 <xref:System.Data.Services.IUpdatable.SaveChanges%2A> 来创建媒体链接入口。

- <xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetWriteStream%2A> 的实现不应返回 <xref:System.IO.MemoryStream> 对象。 如果使用这种类型的流，则在服务接收到非常大的数据流时将会释放内存资源。

- 以下是在数据库中存储媒体资源时应考虑的事项：

  - 数据模型中不应包括作为媒体资源的二进制属性。 数据模型中公开的所有属性都会在响应源的入口中返回。

  - 为了提高使用大型二进制数据流时的性能，建议您创建一个自定义流类来存储数据库中的二进制数据。 此类由 <xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetWriteStream%2A> 实现返回并将二进制数据分块区发送到数据库。 对于 SQL Server 数据库，当二进制数据大于 1 MB 时，建议使用 FILESTREAM 将数据流式传输到数据库。

  - 确保您的数据库设计为存储将由数据服务接收的大型二进制数据流。

  - 当客户端发送 POST 请求以便将媒体链接入口与媒体资源一起插入到单个请求时，将先调用 <xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetWriteStream%2A> 以获取流，然后数据服务才将新实体插入到数据库。 流提供程序实现必须能够处理此数据服务行为。 请考虑使用单独的数据表来存储二进制数据或者在文件中存储数据流，直至已在数据库中插入实体。

- 当实现 <xref:System.Data.Services.Providers.IDataServiceStreamProvider.DeleteStream%2A>、<xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetReadStream%2A> 或 <xref:System.Data.Services.Providers.IDataServiceStreamProvider.GetWriteStream%2A> 方法时，您必须使用以方法参数的形式提供的 eTag 和 Content-Type 值。 不要在 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 提供程序实现中设置 eTag 或 Content-Type 标头。

- 默认情况下，客户端通过使用分块的 HTTP 传输编码发送大型二进制数据流。 由于 ASP.NET 开发服务器不支持这种编码，因此不能使用此 Web 服务器来承载必须接受大型二进制数据流的流数据服务。 有关 ASP.NET 开发服务器的详细信息，请参阅 [Visual Studio for ASP.NET Web 项目中的 Web 服务器](/previous-versions/aspnet/58wxa9w5(v=vs.120))。

<a name="versioning"></a>

## <a name="versioning-requirements"></a>版本控制要求

流提供程序具有以下 OData 协议版本要求：

- 流提供程序要求数据服务支持 OData 协议版本2.0 和更高版本。

有关详细信息，请参阅 [数据服务版本控制](data-service-versioning-wcf-data-services.md)。

## <a name="see-also"></a>请参阅

- [数据服务提供程序](data-services-providers-wcf-data-services.md)
- [自定义数据服务提供程序](custom-data-service-providers-wcf-data-services.md)
- [处理二进制数据](working-with-binary-data-wcf-data-services.md)
