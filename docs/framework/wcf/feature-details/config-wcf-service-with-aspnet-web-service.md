---
title: 如何：配置 WCF 服务以与 ASP.NET Web 服务客户端进行互操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 48e1cd90-de80-4d6c-846e-631878955762
ms.openlocfilehash: 9c241c06f153e4f85c70459ff3c50889057103f5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295036"
---
# <a name="how-to-configure-wcf-service-to-interoperate-with-aspnet-web-service-clients"></a>如何：配置 WCF 服务以与 ASP.NET Web 服务客户端进行互操作

若要将 Windows Communication Foundation (WCF) 服务终结点配置为可与 ASP.NET Web 服务客户端进行互操作，请使用 <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType> 类型作为服务终结点的绑定类型。  
  
 你可以根据需要在该绑定上启用对 HTTPS 和传输级客户端身份验证的支持。 ASP.NET Web 服务客户端不支持 MTOM 消息编码，因此 <xref:System.ServiceModel.BasicHttpBinding.MessageEncoding%2A?displayProperty=nameWithType> 属性应保留为其默认值，即 <xref:System.ServiceModel.WSMessageEncoding.Text?displayProperty=nameWithType> 。 ASP.NET Web 服务客户端不支持 WS 安全，因此 <xref:System.ServiceModel.BasicHttpBinding.Security%2A?displayProperty=nameWithType> 应将设置为 <xref:System.ServiceModel.BasicHttpSecurityMode.Transport> 。  
  
 若要使 WCF 服务的元数据可用于 ASP.NET Web 服务代理生成工具 (即， [Web 服务描述语言工具 ( # A0)](/previous-versions/dotnet/netframework-4.0/7h3ystb6(v=vs.100))、 [Web 服务发现工具 ( # A1)](/previous-versions/dotnet/netframework-4.0/cy2a3ybs(v=vs.100))以及 Visual Studio 中的 " **添加 Web 引用** " 功能) ，你应公开 HTTP/GET 元数据终结点。  
  
## <a name="add-an-endpoint-in-code"></a>在代码中添加终结点  
  
1. 创建一个新的 <xref:System.ServiceModel.BasicHttpBinding> 实例。  
  
2. （可选）将此服务终结点绑定的安全模式设置为 <xref:System.ServiceModel.BasicHttpSecurityMode.Transport>，从而为此绑定启用传输安全。 有关详细信息，请参阅 [传输安全性](transport-security.md)。  
  
3. 使用刚创建的绑定实例，向服务主机添加一个新的应用程序终结点。 有关如何在代码中添加服务终结点的详细信息，请参阅 [如何：在代码中创建服务终结点](how-to-create-a-service-endpoint-in-code.md)。  
  
4. 为服务启用一个 HTTP/GET 元数据终结点。 有关详细信息，请参阅 [如何：使用代码发布服务的元数据](how-to-publish-metadata-for-a-service-using-code.md)。  
  
## <a name="add-an-endpoint-in-a-configuration-file"></a>在配置文件中添加终结点  
  
1. 创建一个新的 <xref:System.ServiceModel.BasicHttpBinding> 绑定配置。 有关详细信息，请参阅 [如何：在配置中指定服务绑定](../how-to-specify-a-service-binding-in-configuration.md)。  
  
2. （可选）将此服务终结点绑定的安全模式设置为 <xref:System.ServiceModel.BasicHttpSecurityMode.Transport>，从而为此绑定配置启用传输安全。 有关详细信息，请参阅 [传输安全性](transport-security.md)。  
  
3. 使用刚创建的绑定配置，为服务配置一个新的应用程序终结点。 有关如何在配置文件中添加服务终结点的详细信息，请参阅 [如何：在配置中创建服务终结点](how-to-create-a-service-endpoint-in-configuration.md)。  
  
4. 为服务启用一个 HTTP/GET 元数据终结点。 有关详细信息，请参阅 [如何：使用配置文件发布服务的元数据](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例演示如何在代码中或在配置文件中添加与 ASP.NET Web 服务客户端兼容的 WCF 终结点。  
  
 [!code-csharp[C_HowTo-WCFServiceAndASMXClient#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/cs/program.cs#0)]
 [!code-vb[C_HowTo-WCFServiceAndASMXClient#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/vb/program.vb#0)]
 [!code-xml[C_HowTo-WCFServiceAndASMXClient#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/common/app.config#1)]
  
## <a name="see-also"></a>另请参阅

- [如何：在代码中创建服务终结点](how-to-create-a-service-endpoint-in-code.md)
- [如何：使用代码发布服务的元数据](how-to-publish-metadata-for-a-service-using-code.md)
- [如何：在配置中指定服务绑定](../how-to-specify-a-service-binding-in-configuration.md)
- [如何：在配置中创建服务终结点](how-to-create-a-service-endpoint-in-configuration.md)
- [如何：使用配置文件发布服务的元数据](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)
- [传输安全](transport-security.md)
- [使用元数据](using-metadata.md)
