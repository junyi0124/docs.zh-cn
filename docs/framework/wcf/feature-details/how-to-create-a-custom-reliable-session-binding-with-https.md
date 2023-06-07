---
title: 如何：使用 HTTPS 创建自定义可靠会话绑定
ms.date: 03/30/2017
ms.assetid: fa772232-da1f-4c66-8c94-e36c0584b549
ms.openlocfilehash: 70f8f4f33626ab0d1705e03750bfd9baa324e60a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598991"
---
# <a name="how-to-create-a-custom-reliable-session-binding-with-https"></a>如何：使用 HTTPS 创建自定义可靠会话绑定

本主题演示如何对可靠会话使用安全套接字层 (SSL) 传输安全。 若要通过 HTTPS 使用可靠会话，必须创建使用可靠会话和 HTTPS 传输协议的自定义绑定。 可以通过使用代码以强制方式或在配置文件中以声明方式启用可靠会话。 此过程使用客户端和服务配置文件来启用可靠会话和 [**\<httpsTransport>**](../../configure-apps/file-schema/wcf/httpstransport.md) 元素。

此过程的关键部分是 **\<endpoint>** 配置元素包含一个 `bindingConfiguration` 属性，该属性引用名为的自定义绑定配置 `reliableSessionOverHttps` 。 [**\<binding>**](../../configure-apps/file-schema/wcf/bindings.md)配置元素引用此名称以指定通过包括和元素来使用可靠会话和 HTTPS 传输 **\<reliableSession>** **\<httpsTransport>** 。

有关此示例的源副本，请参阅[通过 HTTPS 自定义绑定可靠会话](../samples/custom-binding-reliable-session-over-https.md)。

### <a name="configure-the-service-with-a-custombinding-to-use-a-reliable-session-with-https"></a>使用 CustomBinding 配置服务，以便将可靠会话与 HTTPS 一起使用

1. 为该类型的服务定义服务协定。

   [!code-csharp[c_HowTo_CreateReliableSessionHTTPS#1121](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/service.cs#1121)]

1. 在服务类中实现该服务协定。 请注意，在服务的实现内未指定地址或绑定信息。 无需编写代码即可从配置文件中检索地址或绑定信息。

   [!code-csharp[c_HowTo_CreateReliableSessionHTTPS#1122](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/service.cs#1122)]

1. 创建一个*web.config*文件，以便 `CalculatorService` 使用一个名为的自定义绑定（ `reliableSessionOverHttps` 使用可靠会话和 HTTPS 传输）为设置终结点。

   [!code-xml[c_HowTo_CreateReliableSessionHTTPS#2111](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/common/web.config#2111)]

1. 创建包含以下行的*服务 .svc*文件：

   `<%@ServiceHost language=c# Service="CalculatorService" %>`

1. 将*服务 .svc*文件放在 INTERNET INFORMATION SERVICES （IIS）虚拟目录中。

### <a name="configure-the-client-with-a-custombinding-to-use-a-reliable-session-with-https"></a>使用 CustomBinding 配置客户端以使用与 HTTPS 的可靠会话

1. 使用[*Svcutil.exe*元数据实用工具（）](../servicemodel-metadata-utility-tool-svcutil-exe.md)从命令行生成服务元数据中的代码。

   ```console
   Svcutil.exe <Metadata Exchange (MEX) address or HTTP GET address>
   ```

1. 生成的客户端包含 `ICalculator` 定义客户端实现必须满足的服务协定的接口。

   [!code-csharp[C_HowTo_CreateReliableSessionHTTPS#1221](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/client.cs#1221)]

1. 生成的客户端应用程序还包含 `ClientCalculator` 的实现。 请注意，在服务的实现内未指定地址和绑定信息。 不需要编写代码来检索配置文件中的地址和绑定信息。

   [!code-csharp[C_HowTo_CreateReliableSessionHTTPS#1222](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/client.cs#1222)]

1. 将名为的自定义绑定配置 `reliableSessionOverHttps` 为使用 HTTPS 传输和可靠会话。

   [!code-xml[C_HowTo_CreateReliableSessionHTTPS#2211](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/common/app.config#2211)]

1. 在应用程序中创建 `ClientCalculator` 的实例，然后调用服务操作。

   [!code-csharp[C_HowTo_CreateReliableSessionHTTPS#1223](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto_createreliablesessionhttps/cs/client.cs#1223)]

1. 编译并运行客户端。  

## <a name="net-framework-security"></a>.NET Framework 安全性

由于本示例中使用的证书是用*Makecert*创建的测试证书，因此当你尝试从浏览器访问 HTTPS 地址（例如）时，会出现安全警报。 `https://localhost/servicemodelsamples/service.svc`

## <a name="see-also"></a>另请参阅

- [可靠会话](reliable-sessions.md)
