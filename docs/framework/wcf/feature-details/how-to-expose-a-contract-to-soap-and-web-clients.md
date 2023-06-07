---
title: 如何：向 SOAP 和 Web 客户端公开协定
description: 了解如何使 WFC 服务器终结点可用于 SOAP 和非 SOAP 客户端。 默认情况下，终结点仅可用于 SOAP 客户端。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: bb765a48-12f2-430d-a54d-6f0c20f2a23a
ms.openlocfilehash: b1bdb7af51e0e2795c36865058fbeb34a716e3e2
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246969"
---
# <a name="how-to-expose-a-contract-to-soap-and-web-clients"></a>如何：向 SOAP 和 Web 客户端公开协定

默认情况下，Windows Communication Foundation （WCF）使终结点仅可用于 SOAP 客户端。 [如何：创建基本 WCF WEB HTTP 服务](how-to-create-a-basic-wcf-web-http-service.md)，终结点可供非 SOAP 客户端使用。 有时可能需要使相同的协定可以同时作为 Web 终结点和 SOAP 终结点使用。 本主题演示了如何实现此目的的示例。

## <a name="to-define-the-service-contract"></a>定义服务协定

1. 使用用标记的接口和特性定义服务协定 <xref:System.ServiceModel.ServiceContractAttribute> ， <xref:System.ServiceModel.Web.WebInvokeAttribute> <xref:System.ServiceModel.Web.WebGetAttribute> 如下面的代码所示：

    [!code-csharp[htSoapWeb#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#0)]
    [!code-vb[htSoapWeb#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#0)]

    > [!NOTE]
    > 默认情况下，<xref:System.ServiceModel.Web.WebInvokeAttribute> 将 POST 调用映射到操作。 但是，可以通过指定“method=”参数指定要映射到操作的方法。 <xref:System.ServiceModel.Web.WebGetAttribute> 不具有“method=”参数，仅将 GET 调用映射到服务操作。

2. 实现服务协定，如下面的代码所示：

     [!code-csharp[htSoapWeb#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#1)]
     [!code-vb[htSoapWeb#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#1)]

## <a name="to-host-the-service"></a>承载服务

1. 创建一个 <xref:System.ServiceModel.ServiceHost> 对象，如下面的代码所示：

     [!code-csharp[htSoapWeb#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#2)]
     [!code-vb[htSoapWeb#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#2)]

2. 为 <xref:System.ServiceModel.Description.ServiceEndpoint> <xref:System.ServiceModel.BasicHttpBinding> SOAP 终结点添加一个，如下面的代码所示：

     [!code-csharp[htSoapWeb#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#3)]
     [!code-vb[htSoapWeb#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#3)]

3. 为 <xref:System.ServiceModel.Description.ServiceEndpoint> <xref:System.ServiceModel.WebHttpBinding> 非 SOAP 终结点添加，并将添加 <xref:System.ServiceModel.Description.WebHttpBehavior> 到终结点，如下面的代码所示：

     [!code-csharp[htSoapWeb#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#4)]
     [!code-vb[htSoapWeb#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#4)]

4. 对 `Open()` 实例调用 <xref:System.ServiceModel.ServiceHost> 以打开服务主机，如下面的代码所示：

     [!code-csharp[htSoapWeb#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#5)]
     [!code-vb[htSoapWeb#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#5)]

## <a name="to-call-service-operations-mapped-to-get-in-internet-explorer"></a>在 Internet Explorer 中调用映射到 GET 的服务操作

1. 打开 Internet Explorer 并键入 " `http://localhost:8000/Web/EchoWithGet?s=Hello, world!` "，然后按 enter。 URL 包含服务的基址（ `http://localhost:8000/` ）、终结点的相对地址（""）、要调用的服务操作（"EchoWithGet"）和一个问号，后跟一个用 "and" 符分隔的命名参数列表（&）。

## <a name="to-call-service-operations-on-the-web-endpoint-in-code"></a>使用代码在 Web 终结点上调用服务操作

1. 在 <xref:System.ServiceModel.Web.WebChannelFactory%601> 块中创建 `using` 的实例，如下面的代码所示。

     [!code-csharp[htSoapWeb#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#6)]
     [!code-vb[htSoapWeb#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#6)]

> [!NOTE]
> 将自动在 `Close()` 块末尾处的通道上调用 `using`。

1. 创建通道并调用服务，如下面的代码所示。

     [!code-csharp[htSoapWeb#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#8)]
     [!code-vb[htSoapWeb#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#8)]

## <a name="to-call-service-operations-on-the-soap-endpoint"></a>在 SOAP 终结点上调用服务操作

1. 在 <xref:System.ServiceModel.ChannelFactory> 块中创建 `using` 的实例，如下面的代码所示。

    [!code-csharp[htSoapWeb#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#10)]
    [!code-vb[htSoapWeb#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#10)]

2. 创建通道并调用服务，如下面的代码所示。

    [!code-csharp[htSoapWeb#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#11)]
    [!code-vb[htSoapWeb#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#11)]

## <a name="to-close-the-service-host"></a>关闭服务主机

1. 关闭服务主机，如下面的代码所示。

    [!code-csharp[htSoapWeb#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#9)]
    [!code-vb[htSoapWeb#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#9)]

## <a name="example"></a>示例

下面是此主题的完整代码列表：

[!code-csharp[htSoapWeb#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/htsoapweb/cs/program.cs#13)]
[!code-vb[htSoapWeb#13](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htsoapweb/vb/program.vb#13)]

## <a name="compiling-the-code"></a>编译代码

 编译 Service.cs 时，请参考 System.ServiceModel.dll 和 System.ServiceModel.Web.dll。

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
- <xref:System.ServiceModel.Web.WebInvokeAttribute>
- <xref:System.ServiceModel.Web.WebServiceHost>
- <xref:System.ServiceModel.ChannelFactory>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- [WCF Web HTTP 编程模型](wcf-web-http-programming-model.md)
