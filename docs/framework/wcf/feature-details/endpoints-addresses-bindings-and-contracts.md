---
title: 终结点：地址、绑定和协定
description: 了解如何通过服务终结点与 WCF 服务进行的所有通信，这些终结点为客户端提供对服务所提供的功能的访问权限。
ms.date: 03/30/2017
helpviewer_keywords:
- endpoints [WCF]
- Windows Communication Foundation [WCF], endpoints
- WCF [WCF], endpoints
ms.assetid: 9ddc46ee-1883-4291-9926-28848c57e858
ms.openlocfilehash: ce0874bfed716716b6fd1801b35a4266095cd752
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247307"
---
# <a name="endpoints-addresses-bindings-and-contracts"></a>终结点：地址、绑定和协定

与 Windows Communication Foundation （WCF）服务的所有通信都通过服务*终结点*进行。 终结点为客户端提供对 WCF 服务所提供的功能的访问权限。

每个终结点包含四个属性：

- 一个指示可以查找终结点的位置的地址。

- 一个指定客户端如何与终结点进行通信的绑定。

- 一个标识可用操作的协定。

- 一组指定终结点的本地实现细节的行为。

本主题讨论此终结点结构，并说明它在 WCF 对象模型中的表示方式。

## <a name="the-structure-of-an-endpoint"></a>终结点的结构

每个终结点由以下内容组成：

- 地址：地址唯一地标识终结点，并告诉服务的潜在客户其所在的位置。 它在 WCF 对象模型中由 <xref:System.ServiceModel.EndpointAddress> 类表示。 一个 <xref:System.ServiceModel.EndpointAddress> 类包含：

  - 一个表示服务地址的 <xref:System.ServiceModel.EndpointAddress.Uri%2A> 属性。

  - 一个表示服务安全标识和可选消息头集合的 <xref:System.ServiceModel.EndpointAddress.Identity%2A> 属性。 可选消息头用于提供其他更多详细寻址信息来标识终结点或与终结点交互。

  有关详细信息，请参阅[指定终结点地址](../specifying-an-endpoint-address.md)。

- 绑定：绑定指定如何与终结点进行通信。 这包括：

  - 要使用的传输协议（例如，TCP 或 HTTP）。

  - 要用于消息的编码（例如，文本或二进制）。

  - 必需的安全要求（例如，SSL 或 SOAP 消息安全）。

  有关详细信息，请参阅[WCF 绑定概述](../bindings-overview.md)。 绑定由抽象基类在 WCF 对象模型中表示 <xref:System.ServiceModel.Channels.Binding> 。 大多数情况下，用户可以使用系统提供的绑定之一。 有关详细信息，请参阅[系统提供的绑定](../system-provided-bindings.md)。

- 协定：协定概述了终结点向客户端公开的功能。 协定指定：

  - 客户端可以调用的操作。

  - 消息的窗体。

  - 调用操作所需的输入参数或数据的类型。

  - 客户端可以预期的处理或响应消息的类型。

  有关定义协定的详细信息，请参阅[设计服务协定](../designing-service-contracts.md)。

- 行为：可以使用终结点行为来自定义服务终结点的本地行为。 终结点行为是通过参与生成 WCF 运行时的过程来实现的。 终结点行为的一个示例是 <xref:System.ServiceModel.Description.ServiceEndpoint.ListenUri%2A> 属性，可以利用该属性指定与 SOAP 或 Web 服务描述语言 (WSDL) 地址不同的侦听地址。 有关详细信息，请参阅[ClientViaBehavior](../diagnostics/wmi/clientviabehavior.md)。

## <a name="defining-endpoints"></a>定义终结点

可以通过使用代码以强制方式或通过配置以声明方式指定服务的终结点。 有关详细信息，请参阅[如何：在配置中创建服务终结点](how-to-create-a-service-endpoint-in-configuration.md)和[如何：在代码中创建服务终结点](how-to-create-a-service-endpoint-in-code.md)。

## <a name="in-this-section"></a>本节内容

本节说明了绑定、终结点和地址的用途，演示了如何配置绑定和终结点，并演示了如何使用 `ClientVia` 行为和 `ListenUri` 属性。

[地址](endpoint-addresses.md)\
介绍如何在 WCF 中对终结点寻址。

[绑定](bindings.md)\
描述如何使用绑定指定客户端与服务相互通信所需的传输、编码和协议详细信息。

[签订](contracts.md)\
描述协定如何定义服务的方法。

[如何：在配置中创建服务终结点](how-to-create-a-service-endpoint-in-configuration.md)\
描述如何在配置中创建服务终结点。

[如何：在代码中创建服务终结点](how-to-create-a-service-endpoint-in-code.md)\
描述如何在代码中创建服务终结点。

[如何：使用 Svcutil.exe 验证已编译的服务代码](how-to-use-svcutil-exe-to-validate-compiled-service-code.md)\
介绍如何在不使用配置的[元数据实用工具（Svcutil.exe）](../servicemodel-metadata-utility-tool-svcutil-exe.md)托管服务的情况下检测服务实现和配置中的错误。

## <a name="see-also"></a>请参阅

- [正在配置服务](../configuring-services.md)
- [扩展绑定](../extending/extending-bindings.md)
