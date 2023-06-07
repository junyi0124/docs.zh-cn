---
title: 处理二进制数据（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, binary data
- WCF Data Services, streams
ms.assetid: aeccc45c-d5c5-4671-ad63-a492ac8043ac
ms.openlocfilehash: 3c391e641df52d9143630406a40e17c6bc853865
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90551746"
---
# <a name="working-with-binary-data-wcf-data-services"></a>处理二进制数据（WCF 数据服务）

WCF 数据服务客户端库可通过以下方式之一从 Open Data Protocol (OData) 源检索和更新二进制数据：

- 作为实体的基元类型属性。 当使用可轻松加载到内存中的小型二进制数据对象时，建议使用此方法。 在这种情况下，二进制属性是数据模型公开的实体属性，而数据服务会在响应消息中将二进制数据序列化为 base-64 二进制编码的 XML。

- 作为单独的二进制资源流。 当访问和更改可能表示照片、视频或其他任何类型的二进制编码数据的二进制大型对象 (BLOB) 数据时，建议使用此方法。

WCF 数据服务使用 OData 中定义的 HTTP 来实现二进制数据流式处理。 在此机制中，二进制数据被视为独立于实体但又与实体相关的媒体资源，这称为媒体链接入口。 有关详细信息，请参阅 [流式处理提供程序](streaming-provider-wcf-data-services.md)。

> [!TIP]
> 有关如何创建 Windows Presentation Foundation (WPF) 客户端应用程序（该应用程序从存储照片的 OData 服务下载二进制图像文件）的分步示例，请参阅文章 [数据服务流提供程序系列-第2部分：从客户端访问媒体资源流](/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client)。 若要下载博客文章中所述的流照片数据服务的示例代码，请参阅 GitHub 中的 [流式处理照片数据服务示例](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/Streaming%20Photo%20OData%20Service%20Sample) 。

## <a name="entity-metadata"></a>实体元数据

具有相关媒体资源流的实体由应用于实体类型（媒体链接入口）的 `HasStream` 属性在数据服务元数据中表示。 在下面的示例中， `PhotoInfo` 实体是具有相关媒体资源的媒体链接项，由 `HasStream` 特性指示。

[!code-xml[Astoria Photo Streaming Service#HasStream](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_photo_streaming_service/xml/photodata.edmx#hasstream)]

本主题中的其余示例揭示了如何访问和更改媒体资源流。 有关如何使用 WCF 数据服务客户端库在 .NET Framework 客户端应用程序中使用媒体资源流的完整示例，请参阅 [从客户端访问媒体资源流](/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client)一文。

## <a name="accessing-the-binary-resource-stream"></a>访问二进制资源流

WCF 数据服务客户端库提供了从基于 OData 的数据服务访问二进制资源流的方法。 下载媒体资源时，可以使用媒体资源的 URI，也可以获取一个包含媒体资源数据本身的二进制流。 还可以上载媒体资源数据作为一个二进制流。

> [!TIP]
> 有关如何创建 Windows Presentation Foundation (WPF) 客户端应用程序（该应用程序从存储照片的 OData 服务下载二进制图像文件）的分步示例，请参阅文章 [数据服务流提供程序系列-第2部分：从客户端访问媒体资源流](/archive/blogs/astoriateam/data-services-streaming-provider-series-part-2-accessing-a-media-resource-stream-from-the-client)。 若要下载博客文章中所述的流照片数据服务的示例代码，请参阅 GitHub 中的 [流式处理照片数据服务示例](https://github.com/microsoftarchive/msdn-code-gallery-community-s-z/tree/master/Streaming%20Photo%20OData%20Service%20Sample) 。

### <a name="getting-the-uri-of-the-binary-stream"></a>获取二进制流的 URI

检索某些类型的媒体资源（如图像和其他媒体文件）时，在应用程序中使用媒体资源的 URI 通常比处理二进制数据流本身更容易。 要获取与给定媒体链接入口相关联的资源流的 URI，必须对跟踪实体的 <xref:System.Data.Services.Client.DataServiceContext.GetReadStreamUri%2A> 实例调用 <xref:System.Data.Services.Client.DataServiceContext> 方法。 下面的示例揭示了如何调用 <xref:System.Data.Services.Client.DataServiceContext.GetReadStreamUri%2A> 方法以获取用于在客户端上创建新图像的媒体资源流的 URI：

[!code-csharp[Astoria Photo Streaming Client#GetReadStreamUri](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_photo_streaming_client/cs/photowindow.xaml.cs#getreadstreamuri)]
[!code-vb[Astoria Photo Streaming Client#GetReadStreamUri](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_photo_streaming_client/vb/photowindow.xaml.vb#getreadstreamuri)]

### <a name="downloading-the-binary-resource-stream"></a>下载二进制资源流

检索二进制资源流时，必须对跟踪媒体链接入口的 <xref:System.Data.Services.Client.DataServiceContext.GetReadStream%2A> 实例调用 <xref:System.Data.Services.Client.DataServiceContext> 方法。 该方法向数据服务发送一个返回 <xref:System.Data.Services.Client.DataServiceStreamResponse> 对象的请求，该对象具有对包含资源的流的引用。 当应用程序要求将二进制资源作为 <xref:System.IO.Stream> 时可使用该方法。 下面的示例揭示了如何调用 <xref:System.Data.Services.Client.DataServiceContext.GetReadStream%2A> 方法以检索用于在客户端上创建新图像的流：

[!code-csharp[Astoria Streaming Client#GetReadStreamClient](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_streaming_client/cs/customerphotowindow.xaml.cs#getreadstreamclient)]
[!code-vb[Astoria Streaming Client#GetReadStreamClient](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_streaming_client/vb/customerphotowindow.xaml.vb#getreadstreamclient)]

> [!NOTE]
> 数据服务不会设置包含二进制数据流的响应消息中的 Content-Length 标头。 此值可能不反映二进制数据流的实际长度。

### <a name="uploading-a-media-resource-as-a-stream"></a>将媒体资源作为流上载

若要插入或更新媒体资源，请对正在跟踪实体的 <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> 实例调用 <xref:System.Data.Services.Client.DataServiceContext> 方法。 此方法向包含从所提供的流中读取的媒体资源的数据服务发送请求。 下面的示例揭示了如何调用 <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> 方法以向数据服务发送图像：

[!code-csharp[Astoria Photo Streaming Client#SetSaveStream](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_photo_streaming_client/cs/photodetailswindow.xaml.cs#setsavestream)]
[!code-vb[Astoria Photo Streaming Client#SetSaveStream](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_photo_streaming_client/vb/photodetailswindow.xaml.vb#setsavestream)]

在此示例中，通过为 <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> 参数提供 `true` 值，调用 `closeStream` 方法。 这样可保证在将二进制数据上载到数据服务之后，<xref:System.Data.Services.Client.DataServiceContext> 将会关闭流。

> [!NOTE]
> 调用 <xref:System.Data.Services.Client.DataServiceContext.SetSaveStream%2A> 时，只有在调用 <xref:System.Data.Services.Client.DataServiceContext.SaveChanges%2A> 之后，才会将流发送到数据服务。

## <a name="see-also"></a>请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
- [将数据绑定到控件](binding-data-to-controls-wcf-data-services.md)
