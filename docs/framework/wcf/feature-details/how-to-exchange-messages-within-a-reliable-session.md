---
title: 如何：在可靠会话内交换消息
ms.date: 03/30/2017
ms.assetid: 87cd0e75-dd2c-44c1-8da0-7b494bbdeaea
ms.openlocfilehash: 97371f8572d5d0db633ab8dd1ca82067d9d55c3f
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90550184"
---
# <a name="how-to-exchange-messages-within-a-reliable-session"></a>如何：在可靠会话内交换消息

本主题概述了使用系统提供的绑定之一来启用可靠会话所需的步骤。这些绑定支持可靠会话，但默认情况下不支持。 可使用代码以强制方式或在配置文件中以声明方式启用可靠会话。 此过程使用客户端和服务配置文件来启用可靠会话，并规定消息按发送顺序到达的顺序。

此过程的关键部分是终结点配置元素包含一个 `bindingConfiguration` 属性，该属性引用名为的绑定配置 `Binding1` 。 [**\<binding>**](../../configure-apps/file-schema/wcf/bindings.md)配置元素通过将元素的属性设置为来引用此名称以启用可靠会话 `enabled` [**\<reliableSession>**](/previous-versions/dotnet/netframework-4.0/ms731302(v=vs.100)) `true` 。 通过将 `ordered` 属性设置为 `true`，可为可靠会话指定有序传送保证。

有关此示例的源副本，请参阅 [WS 可靠会话](../samples/ws-reliable-session.md)。

### <a name="configure-the-service-with-a-wshttpbinding-to-use-a-reliable-session"></a>使用 WSHttpBinding 配置服务以使用可靠会话

1. 为该类型的服务定义服务协定。

   [!code-csharp[c_HowTo_UseReliableSession#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/service.cs#1121)]

1. 在服务类中实现该服务协定。 请注意，在服务的实现内未指定地址或绑定信息。 无需编写代码即可从配置文件中检索地址或绑定信息。

   [!code-csharp[c_HowTo_UseReliableSession#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/service.cs#1122)]

1. 创建一个 *Web.config* 文件，以便为 `CalculatorService` 使用 <xref:System.ServiceModel.WSHttpBinding> 启用了可靠会话并按序传递所需的消息的终结点。

   [!code-xml[c_HowTo_UseReliableSession#2111](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/common/web.config#2111)]

1. 创建包含以下行的 *服务 .svc* 文件：

   ```aspx-csharp
   <%@ServiceHost language=c# Service="CalculatorService" %>
   ```

1. 将 *服务 .svc* 文件放入 INTERNET INFORMATION SERVICES (IIS) 虚拟目录。

### <a name="configure-the-client-with-a-wshttpbinding-to-use-a-reliable-session"></a>使用 WSHttpBinding 配置客户端以使用可靠会话

1. 使用 " ("，从命令行 [ *Svcutil.exe*) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) ，从服务元数据生成代码：

   ```console
   Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
   ```

1. 生成的客户端包含 `ICalculator` 定义客户端实现必须满足的服务协定的接口。

   [!code-csharp[C_HowTo_UseReliableSession#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1221)]

1. 生成的客户端应用程序还包含 `ClientCalculator` 的实现。 请注意，在服务的实现内部，未指定地址和绑定信息。 无需编写代码即可从配置文件中检索地址或绑定信息。

   [!code-csharp[C_HowTo_UseReliableSession#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1222)]

1. *Svcutil.exe* 还会生成使用类的客户端的配置 <xref:System.ServiceModel.WSHttpBinding> 。 使用 Visual Studio 时，将配置文件命名 *App.config* 。

   [!code-xml[C_HowTo_UseReliableSession#2211](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/common/app.config#2211)]

1. `ClientCalculator`在应用程序中创建的实例，然后调用服务操作。

   [!code-csharp[C_HowTo_UseReliableSession#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_usereliablesession/cs/client.cs#1223)]

1. 编译并运行客户端。

## <a name="example"></a>示例

默认情况下，有多种系统提供的绑定支持可靠会话。 这些方法包括：

- <xref:System.ServiceModel.WSDualHttpBinding>

- <xref:System.ServiceModel.NetNamedPipeBinding>

- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>

有关如何创建支持可靠会话的自定义绑定的示例，请参阅 [如何：使用 HTTPS 创建自定义可靠会话绑定](how-to-create-a-custom-reliable-session-binding-with-https.md)。

## <a name="see-also"></a>请参阅

- [可靠会话](reliable-sessions.md)
