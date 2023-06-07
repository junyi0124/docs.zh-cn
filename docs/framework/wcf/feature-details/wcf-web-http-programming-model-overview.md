---
title: WCF Web HTTP 编程模型概述
ms.date: 03/30/2017
ms.assetid: 381fdc3a-6e6c-4890-87fe-91cca6f4b476
ms.openlocfilehash: 713dd05daa5071f253afd70e735475e49a986aa7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239011"
---
# <a name="wcf-web-http-programming-model-overview"></a>WCF Web HTTP 编程模型概述

Windows Communication Foundation (WCF) WEB HTTP 编程模型提供了用 WCF 生成 WEB HTTP 服务所需的基本元素。 WCF WEB HTTP 服务旨在由最大范围的可能客户端（包括 Web 浏览器）访问，并且具有以下独特要求：  
  
- **Uri 和 Uri 处理** Uri 在 WEB HTTP 服务的设计中扮演着一个中心角色。 WCF WEB HTTP 编程模型使用 <xref:System.UriTemplate> 和 <xref:System.UriTemplateTable> 类来提供 URI 处理功能。  
  
- **支持 GET 和 POST 操作** WEB HTTP 服务除了使用各种调用谓词来进行数据修改和远程调用以外，还使用 GET 谓词进行数据检索。 WCF WEB HTTP 编程模型使用 <xref:System.ServiceModel.Web.WebGetAttribute> 和 <xref:System.ServiceModel.Web.WebInvokeAttribute> 将服务操作与 GET 和其他 HTTP 谓词（如 PUT、POST 和 DELETE）相关联。  
  
- **多种数据格式** 除了 SOAP 消息以外，Web 样式服务还处理多种类型的数据。 WCF WEB HTTP 编程模型使用 <xref:System.ServiceModel.WebHttpBinding> 和 <xref:System.ServiceModel.Description.WebHttpBehavior> 来支持多种不同的数据格式，包括 XML 文档、JSON 数据对象和二进制内容（如图像、视频文件或纯文本）的流。  
  
 WCF WEB HTTP 编程模型扩展了 WCF 的覆盖范围，涵盖了包含 WEB HTTP 服务、AJAX 和 JSON 服务以及联合 (ATOM/RSS) 源的 Web 样式方案。 有关 AJAX 和 JSON 服务的详细信息，请参阅 [Ajax 集成和 Json 支持](ajax-integration-and-json-support.md)。 有关联合的详细信息，请参阅 [WCF 联合概述](wcf-syndication-overview.md)。  
  
 对于可从 WEB HTTP 服务返回的数据的类型没有额外的限制。 任何可序列化类型都可以从 WEB HTTP 服务操作返回。 因为 WEB HTTP 服务操作可以通过 Web 浏览器调用，所以对可在 URL 中指定的数据类型有一个限制。 有关默认情况下支持的类型的详细信息，请参阅下面的 **UriTemplate 查询字符串参数和 url** 部分。 通过提供您自己的 T:System.ServiceModel.Dispatcher.QueryStringConverter 实现来指定如何将 URL 中指定的参数转换为实际参数类型，可以更改默认行为。 有关详细信息，请参阅<xref:System.ServiceModel.Dispatcher.QueryStringConverter>。  
  
> [!CAUTION]
> 使用 WCF WEB HTTP 编程模型编写的服务不使用 SOAP 消息。 由于不使用 SOAP，因此不能使用 WCF 提供的安全功能。 然而，您可以通过使用 HTTPS 承载服务来使用基于传输的安全性。 有关 WCF 安全的详细信息，请参阅 [安全性概述](security-overview.md)  
  
> [!WARNING]
> 为 IIS 安装 WebDAV 扩展会导致 Web HTTP 服务返回 HTTP 405 错误，因为 WebDAV 扩展试图处理所有 PUT 请求。 若要解决此问题，你可卸载 WebDAV 扩展或为网站禁用 WebDAV 扩展。 有关详细信息，请参阅 [IIS 和 WebDav](https://learn.iis.net/page.aspx/357/webdav-for-iis-70/)  
  
## <a name="uri-processing-with-uritemplate-and-uritemplatetable"></a>使用 UriTemplate 和 UriTemplateTable 进行 URI 处理  

 URI 模板提供了一种可以高效地表示很大的结构相似的 URI 集的语法。 例如，下面的模板表示所有以“a”开始并以“c”结束而中间段的值不限的、由三个段组成的 URI：a/{segment}/c  
  
 此模板描述如下所示的 URI：  
  
- a/x/c  
  
- a/y/c  
  
- a/z/c  
  
- 等等。  
  
 在此模板中，大括号表示法 ("{segment}") 指示变量段而不是文本值。  
  
 .NET Framework 提供了一个 API 来处理名为 <xref:System.UriTemplate> 的 URI 模板。 `UriTemplates` 允许执行下列操作：  
  
- 您可以 `Bind` 使用一组参数调用其中一个方法来生成与模板匹配的 *完全关闭的 URI* 。 这意味着，URI 模板中的所有变量均由实际值替换。  
  
- 可以使用候选 URI 调用 `Match`()，此时会使用模板将候选 URI 的各个组成部分分解开来，并会返回一个字典，其中包含根据模板中的变量标记的 URI 的不同部分。  
  
- `Bind`() 和 `Match`() 互为逆方法，因此可以调用 `Match`( `Bind`( x ) ) 并返回到开始时的相同环境。  
  
 在很多时候（尤其是在服务器需要基于 URI 将请求调度到某个服务操作时），对于那些可以单独对包含的每个模板进行寻址的数据结构，您需要一直跟踪其中的一组 <xref:System.UriTemplate> 对象。 <xref:System.UriTemplateTable> 表示一组 URI 模板，并在给定的一组模板和候选 URI 中选择最匹配的项。 这并不隶属于任何特定的网络堆栈 (WCF 包含) 因此，你可以在必要的地方使用它。  
  
 WCF 服务模型使用 <xref:System.UriTemplate> 和 <xref:System.UriTemplateTable> 将服务操作与由 <xref:System.UriTemplate> 描述的一组 URI 相关联。 通过使用 <xref:System.UriTemplate> 或 <xref:System.ServiceModel.Web.WebGetAttribute>，将服务操作与 <xref:System.ServiceModel.Web.WebInvokeAttribute> 相关联。 有关和的详细 <xref:System.UriTemplate> 信息 <xref:System.UriTemplateTable> ，请参阅 [UriTemplate 和 UriTemplateTable](uritemplate-and-uritemplatetable.md)  
  
## <a name="webget-and-webinvoke-attributes"></a>WebGet 和 WebInvoke 特性  

 WCF WEB HTTP 服务使用检索谓词 (例如 HTTP GET) 以及各种调用谓词， (例如 HTTP POST、PUT 和 DELETE) 。 WCF WEB HTTP 编程模型允许服务开发人员通过和控制与其服务操作相关联的 URI 模板和谓词 <xref:System.ServiceModel.Web.WebGetAttribute> <xref:System.ServiceModel.Web.WebInvokeAttribute> 。 可以使用 <xref:System.ServiceModel.Web.WebGetAttribute> 和 <xref:System.ServiceModel.Web.WebInvokeAttribute> 来控制各个操作如何绑定到 URI 以及与这些 URI 相关联的 HTTP 方法。 例如，在下面的代码中添加 <xref:System.ServiceModel.Web.WebGetAttribute> 和 <xref:System.ServiceModel.Web.WebInvokeAttribute>。  
  
```csharp
[ServiceContract]  
interface ICustomer  
{  
  //"View It"  
  
  [WebGet]  
  Customer GetCustomer():  
  
  //"Do It"  
    [WebInvoke]  
  Customer UpdateCustomerName( string id,
                               string newName );  
}  
```  
  
 可以使用上面的代码生成下面的 HTTP 请求。  
  
 `GET /GetCustomer`  
  
 `POST /UpdateCustomerName`  
  
 <xref:System.ServiceModel.Web.WebInvokeAttribute> 的默认值为 POST，但也可以将其用于其他谓词。  
  
```csharp
[ServiceContract]  
interface ICustomer  
{  
  //"View It" -> HTTP GET  
    [WebGet( UriTemplate="customers/{id}" )]  
  Customer GetCustomer( string id ):  
  
  //"Do It" -> HTTP PUT  
  [WebInvoke( UriTemplate="customers/{id}", Method="PUT" )]  
  Customer UpdateCustomer( string id, Customer newCustomer );  
}  
```  
  
 若要查看使用 WCF WEB HTTP 编程模型的 WCF 服务的完整示例，请参阅 [如何：创建基本 WCF WEB Http 服务](how-to-create-a-basic-wcf-web-http-service.md)  
  
## <a name="uritemplate-query-string-parameters-and-urls"></a>UriTemplate 查询字符串参数和 URL  

 可以通过键入与服务操作相关联的 URL 来从 Web 浏览器调用 Web 样式服务。 这些服务操作可以采用查询字符串参数，必须在 URL 内使用字符串格式指定这些参数。 下表演示可以在 URL 内传递的类型和使用的格式。  
  
|类型|格式|  
|----------|------------|  
|<xref:System.Byte>|0 - 255|  
|<xref:System.SByte>|-128 - 127|  
|<xref:System.Int16>|-32768 - 32767|  
|<xref:System.Int32>|-2,147,483,648 - 2,147,483,647|  
|<xref:System.Int64>|-9,223,372,036,854,775,808 - 9,223,372,036,854,775,807|  
|<xref:System.UInt16>|0 - 65535|  
|<xref:System.UInt32>|0 - 4,294,967,295|  
|<xref:System.UInt64>|0 - 18,446,744,073,709,551,615|  
|<xref:System.Single>|-3.402823e38 - 3.402823e38（不需要指数表示法）|  
|<xref:System.Double>|-1.79769313486232e308 - 1.79769313486232e308（不需要指数表示法）|  
|<xref:System.Char>|任何单个字符|  
|<xref:System.Decimal>|使用标准表示法的任何小数（无指数）|  
|<xref:System.Boolean>|True 或 False（不区分大小写）|  
|<xref:System.String>|任何字符串（不支持空字符串，且不进行转义）|  
|<xref:System.DateTime>|MM/DD/YYYY<br /><br /> MM/DD/YYYY HH： MM： SS [AM&#124;PM]<br /><br /> 月、日、年<br /><br /> 月份日期年 HH： MM： SS [AM&#124;PM]|  
|<xref:System.TimeSpan>|DD.HH:MM:SS<br /><br /> 此处，DD = 天、HH = 小时、MM = 分钟、SS = 秒钟|  
|<xref:System.Guid>|一个 GUID，例如：<br /><br /> 936DA01F-9ABD-4d9d-80C7-02AF85C822A8|  
|<xref:System.DateTimeOffset>|MM/DD/YYYY HH:MM:SS MM:SS<br /><br /> 此处，DD = 天、HH = 小时、MM = 分钟、SS = 秒钟|  
|枚举|例如，定义枚举的枚举值，如以下代码中所示。<br /><br /> `public enum Days{ Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday };`<br /><br /> 可以在查询字符串中指定任何单独的枚举值（或其对应的整数值）。|  
|具有可在类型和字符串表示形式之间来回进行转换的 `TypeConverterAttribute` 的类型。|取决于类型转换器。|  
  
## <a name="formats-and-the-wcf-web-http-programming-model"></a>格式和 WCF WEB HTTP 编程模型  

 WCF WEB HTTP 编程模型具有用于处理多种不同数据格式的新功能。 在绑定层上，<xref:System.ServiceModel.WebHttpBinding> 可以读取和写入下列不同种类的数据：  
  
- XML  
  
- JSON  
  
- 不透明二进制流  
  
 这意味着 WCF WEB HTTP 编程模型可以处理任何类型的数据，但您可能会对进行编程 <xref:System.IO.Stream> 。  
  
 .NET Framework 3.5 支持 JSON 数据 (AJAX) ，以及联合源 (包括 ATOM 和 RSS) 。 有关这些功能的详细信息，请参阅 [Wcf WEB HTTP 格式设置](wcf-web-http-formatting.md)、 [wcf 联合概述](wcf-syndication-overview.md)以及 [AJAX 集成和 JSON 支持](ajax-integration-and-json-support.md)。  
  
## <a name="wcf-web-http-programming-model-and-security"></a>WCF WEB HTTP 编程模型和安全  

由于 WCF WEB HTTP 编程模型不支持 WS-* 协议，因此保证 WCF WEB HTTP 服务安全的唯一方式是使用 SSL 通过 HTTPS 公开服务。 有关设置 SSL 和 IIS 7.0 的详细信息，请参阅 [如何在 iis 中实现 ssl](https://support.microsoft.com/help/299875/how-to-implement-ssl-in-iis)。
  
## <a name="troubleshooting-the-wcf-web-http-programming-model"></a>WCF WEB HTTP 编程模型疑难解答  

 当使用 <xref:System.ServiceModel.Channels.ChannelFactoryBase%601> 调用 WCF WEB HTTP 服务以创建通道时，即使将其他 <xref:System.ServiceModel.Description.WebHttpBehavior> 传递给 <xref:System.ServiceModel.EndpointAddress>，<xref:System.ServiceModel.EndpointAddress> 也会使用配置文件中设置的 <xref:System.ServiceModel.Channels.ChannelFactoryBase%601>。  
  
## <a name="see-also"></a>另请参阅

- [WCF 联合](wcf-syndication.md)
- [WCF Web HTTP 编程对象模型](wcf-web-http-programming-object-model.md)
- [WCF Web HTTP 编程模型](wcf-web-http-programming-model.md)
