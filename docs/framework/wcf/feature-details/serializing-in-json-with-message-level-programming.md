---
title: 使用消息级编程在 Json 中序列化
ms.date: 03/30/2017
ms.assetid: 5f940ba2-57ee-4c49-a779-957c5e7e71fa
ms.openlocfilehash: 8343f7a8aa3c01557aae7df420351fa318cbb4b5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253929"
---
# <a name="serializing-in-json-with-message-level-programming"></a>使用消息级编程在 Json 中序列化

WCF 支持以 JSON 格式序列化数据。 本主题描述如何使用 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 告知 WCF 序列化您的类型。  
  
## <a name="typed-message-programming"></a>类型化的消息编程  

 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 或 <xref:System.ServiceModel.Web.WebGetAttribute> 应用于服务操作时使用 <xref:System.ServiceModel.Web.WebInvokeAttribute>。 这两个属性允许您指定 `RequestFormat` 和 `ResponseFormat`。 将 JSON 用于请求和响应。 将这两个设置为 `WebMessageFormat.Json` 。  若要使用 JSON，必须使用 <xref:System.ServiceModel.WebHttpBinding> ，后者会自动配置 <xref:System.ServiceModel.Description.WebHttpBehavior> 。 有关 WCF 序列化的详细信息，请参阅 [序列化和反序列](serialization-and-deserialization.md)化。 有关 JSON 和 WCF 的详细信息，请参阅 [Service 工作站-使用 WCF 的 RESTful 服务简介](/archive/msdn-magazine/2009/january/service-station-an-introduction-to-restful-services-with-wcf)。  
  
> [!IMPORTANT]
> 使用 JSON 需要使用 <xref:System.ServiceModel.WebHttpBinding> 和 <xref:System.ServiceModel.Description.WebHttpBehavior>，这两者不支持 SOAP 通信。 与通信的服务 <xref:System.ServiceModel.WebHttpBinding> 不支持公开服务元数据，因此你将无法使用 Visual Studio 的添加服务引用功能或 svcutil.exe 命令行工具来生成客户端代理。 有关如何以编程方式调用使用的服务的详细信息 <xref:System.ServiceModel.WebHttpBinding> ，请参阅 [如何通过 WCF 使用 REST 服务](/archive/blogs/pedram/how-to-consume-rest-services-with-wcf)。  
  
## <a name="untyped-message-programming"></a>非类型化的消息编程  

 当直接使用非类型化的消息对象时，您必须显式设置非类型化消息的属性，以便将其序列化为 JSON。 下面的代码段演示如何执行此操作。  
  
```csharp
 Message response = Message.CreateMessage(  
                  MessageVersion.None,    // No SOAP message version  
                             "*",                     // SOAP action, ignored since this is JSON  
                             "Response string: JSON format specified", // Message body  
                             new DataContractJsonSerializer(typeof(string))); // Specify DataContractJsonSerializer  
      response.Properties.Add( WebBodyFormatMessageProperty.Name,
                    new WebBodyFormatMessageProperty(WebContentFormat.Json)); // Use JSON format  
```  
  
## <a name="see-also"></a>另请参阅

- [AJAX 集成和 JSON 支持](ajax-integration-and-json-support.md)
- [独立 JSON 序列化](stand-alone-json-serialization.md)
- [JSON 序列化](../samples/json-serialization.md)
