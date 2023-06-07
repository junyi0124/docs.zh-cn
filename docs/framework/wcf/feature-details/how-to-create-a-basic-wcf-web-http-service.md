---
title: 如何：创建基本 WCF Web HTTP 服务
description: 了解如何创建在 WCF 中公开 web 终结点的服务。 Web 终结点使用 XML 或 JSON 发送数据。 没有 SOAP 信封。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 877662d3-d372-4e08-b417-51f66a0095cd
ms.openlocfilehash: 7481367f27d973ba809dff5ca1c4a4f168fbbb98
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247099"
---
# <a name="how-to-create-a-basic-wcf-web-http-service"></a>如何：创建基本 WCF Web HTTP 服务

Windows Communication Foundation （WCF）允许您创建公开 Web 终结点的服务。 Web 终结点通过 XML 或 JSON 发送数据，没有 SOAP 信封。 本主题演示如何公开这类终结点。

> [!NOTE]
> 确保 Web 终结点安全的唯一方式是使用传输安全通过 HTTPS 将其公开。 使用基于消息的安全性时，通常将安全信息放置在 SOAP 标头中，原因是发送到非 SOAP 终结点的信息不包含 SOAP 信封，没有地方放置安全信息，因而必须依赖于传输安全性。

## <a name="to-create-a-web-endpoint"></a>创建 Web 终结点

1. 使用以 <xref:System.ServiceModel.ServiceContractAttribute>, <xref:System.ServiceModel.Web.WebInvokeAttribute> 和 <xref:System.ServiceModel.Web.WebGetAttribute> 属性标记的接口来定义服务协定。

     [!code-csharp[htBasicService#0](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#0)]
     [!code-vb[htBasicService#0](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#0)]

    > [!NOTE]
    > 默认情况下，<xref:System.ServiceModel.Web.WebInvokeAttribute> 将 POST 调用映射到操作。 但是，可以通过指定“method=”参数指定要映射到操作的 HTTP 方法（例如，HEAD、PUT 或 DELETE）。 <xref:System.ServiceModel.Web.WebGetAttribute> 不具有“method=”参数，仅将 GET 调用映射到服务操作。

2. 实现服务协定。

     [!code-csharp[htBasicService#1](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#1)]
     [!code-vb[htBasicService#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#1)]

## <a name="to-host-the-service"></a>承载服务

1. 创建 <xref:System.ServiceModel.Web.WebServiceHost> 对象。

     [!code-csharp[htBasicService#2](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#2)]
     [!code-vb[htBasicService#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#2)]

2. 添加一个带有 <xref:System.ServiceModel.Description.ServiceEndpoint> 的 <xref:System.ServiceModel.Description.WebHttpBehavior>。

     [!code-csharp[htBasicService#3](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#3)]
     [!code-vb[htBasicService#3](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#3)]

    > [!NOTE]
    > 如果不添加终结点，则 <xref:System.ServiceModel.Web.WebServiceHost> 自动创建默认终结点。 <xref:System.ServiceModel.Web.WebServiceHost> 还添加 <xref:System.ServiceModel.Description.WebHttpBehavior> 并禁用 HTTP 帮助页和 Web 服务描述语言 (WSDL) GET 功能，因此元数据终结点不会妨碍默认的 HTTP 终结点。
    >
    >  当尝试调用终结点上的操作时，添加 URL 为 "" 的非 SOAP 终结点会导致意外行为。 出现这种情况的原因是，终结点的侦听 URI 与帮助页的 URI 相同（浏览到 WCF 服务的基址时显示的页）。

     要防止此类情况发生，可以执行下列操作之一：

    - 总是为非 SOAP 终结点指定非空白 URI。

    - 关闭帮助页。 可以通过以下代码完成此操作：

     [!code-csharp[htBasicService#4](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/snippets.cs#4)]
     [!code-vb[htBasicService#4](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/snippets.vb#4)]

3. 打开服务主机并等待用户按 Enter。

     [!code-csharp[htBasicService#5](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/snippets.cs#5)]
     [!code-vb[htBasicService#5](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/snippets.vb#5)]

     此示例演示如何使用控制台应用程序来承载 Web 样式服务。 还可以在 IIS 内承载这类服务。 为此，在 .svc 文件中指定 <xref:System.ServiceModel.Activation.WebServiceHostFactory> 类，如以下代码所示。

    ```text
    <%ServiceHost
        language=c#
        Debug="true"
        Service="Microsoft.Samples.Service"
        Factory=System.ServiceModel.Activation.WebServiceHostFactory%>
    ```

## <a name="to-call-service-operations-mapped-to-get-in-internet-explorer"></a>在 Internet Explorer 中调用映射到 GET 的服务操作

1. 打开 Internet Explorer 并键入 " `http://localhost:8000/EchoWithGet?s=Hello, world!` "，然后按 enter。 URL 包含服务的基址（ `http://localhost:8000/` ）、终结点的相对地址（""）、要调用的服务操作（"EchoWithGet"）和一个问号，后跟一个用 "and" 符分隔的命名参数列表（&）。

## <a name="to-call-service-operations-in-code"></a>在代码中调用服务操作

1. 创建 <xref:System.ServiceModel.ChannelFactory%601> 块内的 `using` 的实例。

     [!code-csharp[htBasicService#6](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#6)]
     [!code-vb[htBasicService#6](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#6)]

2. 将 <xref:System.ServiceModel.Description.WebHttpBehavior> 添加到 <xref:System.ServiceModel.ChannelFactory%601> 调用的终结点。

     [!code-csharp[htBasicService#7](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#7)]
     [!code-vb[htBasicService#7](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#7)]

3. 创建通道并调用服务。

     [!code-csharp[htBasicService#8](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#8)]
     [!code-vb[htBasicService#8](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#8)]

4. 关闭 <xref:System.ServiceModel.Web.WebServiceHost>。

     [!code-csharp[htBasicService#9](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#9)]
     [!code-vb[htBasicService#9](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#9)]

## <a name="example"></a>示例

下面列出了此示例的完整代码。

[!code-csharp[htBasicService#10](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#10)]
[!code-vb[htBasicService#10](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#10)]

## <a name="compiling-the-code"></a>编译代码

编译 Service.cs 时，请引用 System.ServiceModel.dll 和 System.ServiceModel.Web.dll。

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
- <xref:System.ServiceModel.Web.WebInvokeAttribute>
- <xref:System.ServiceModel.Web.WebServiceHost>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- [WCF Web HTTP 编程模型](wcf-web-http-programming-model.md)
