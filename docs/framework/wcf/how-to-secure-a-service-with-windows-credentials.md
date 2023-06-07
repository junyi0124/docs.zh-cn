---
title: 如何：使用 Windows 凭据保护服务的安全
description: 了解如何在驻留于 Windows 域中并由同一域中的客户端调用的 WCF 服务上启用传输安全。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, security
ms.assetid: d171b5ca-96ef-47ff-800c-c138023cf76e
ms.openlocfilehash: 8ef164e1475bfd5f047a99426a2bed43a7aa7353
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85244628"
---
# <a name="how-to-secure-a-service-with-windows-credentials"></a>如何：使用 Windows 凭据保护服务的安全

本主题演示如何在驻留于 Windows 域中并由同一域中的客户端调用的 Windows Communication Foundation （WCF）服务上启用传输安全。 有关此方案的详细信息，请参阅[采用 Windows 身份验证的传输安全](./feature-details/transport-security-with-windows-authentication.md)。 有关示例应用程序，请参阅[WSHttpBinding](./samples/wshttpbinding.md)示例。

本主题假定您已定义一个现有的协定接口和实现及其加载项。 您还可以修改一个现有的服务和客户端。

你完全可以在代码中使用 Windows 凭据来保护服务。 或者，也可以通过使用配置文件省略某些代码。 本主题演示了这两种方法。 请务必仅使用其中一种方法，而不是同时使用两种方法。

前三个过程演示如何使用代码保护服务。 第四个和第五个过程演示如何使用配置文件保护服务。

## <a name="using-code"></a>使用代码

服务和客户端的完整代码位于本主题末尾的“示例”一节中。

第一个过程演练如何在代码中创建和配置 <xref:System.ServiceModel.WSHttpBinding> 类。 该绑定使用 HTTP 传输。 在客户端上使用了同一绑定。

#### <a name="to-create-a-wshttpbinding-that-uses-windows-credentials-and-message-security"></a>创建使用 Windows 凭据和消息安全的 WSHttpBinding

1. 在“示例”一节的服务代码中，将此过程的代码插入到 `Run` 类的 `Test` 方法开头。

2. 创建 <xref:System.ServiceModel.WSHttpBinding> 类的一个实例。

3. 将 <xref:System.ServiceModel.WSHttpSecurity.Mode%2A> 类的 <xref:System.ServiceModel.WSHttpSecurity> 属性设置为 <xref:System.ServiceModel.SecurityMode.Message>。

4. 将 <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A> 类的 <xref:System.ServiceModel.MessageSecurityOverHttp> 属性设置为 <xref:System.ServiceModel.MessageCredentialType.Windows>。

5. 此过程的代码如下：

    [!code-csharp[c_SecureWindowsService#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#1)]
    [!code-vb[c_SecureWindowsService#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsservice/vb/secureservice.vb#1)]

### <a name="using-the-binding-in-a-service"></a>在服务中使用该绑定

这是第二个过程，演示如何在自承载服务中使用该绑定。 有关托管服务的详细信息，请参阅[托管服务](hosting-services.md)。

##### <a name="to-use-a-binding-in-a-service"></a>在服务中使用绑定

1. 在上一过程的代码之后插入此过程的代码。

2. 创建一个名为 <xref:System.Type> 的 `contractType` 变量，并为其分配接口类型 (`ICalculator`)。 使用 Visual Basic 时，请使用 `GetType` 运算符; 使用 c # 时，请使用 `typeof` 关键字。

3. 创建另一个名为 <xref:System.Type> 的 `serviceType` 变量，并为其分配所实现的协定的类型 (`Calculator`)。

4. 使用该服务的基址创建 <xref:System.Uri> 类的一个实例，该实例名为 `baseAddress`。 基址必须具有与传输匹配的方案。 在这种情况下，传输方案为 HTTP，地址包含特殊的统一资源标识符（URI） "localhost" 和端口号（8036）以及基本终结点地址（"serviceModelSamples/）：" `http://localhost:8036/serviceModelSamples/` 。

5. 使用 <xref:System.ServiceModel.ServiceHost> 和 `serviceType` 变量创建 `baseAddress` 类的一个实例。

6. 使用 `contractType`、绑定和终结点名称 (secureCalculator) 将一个终结点添加到服务中。 在启动对服务的调用时，客户端必须将基址和终结点名称连接起来。

7. 调用 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> 方法以启动该服务。 此过程的代码如下所示：

    [!code-csharp[c_SecureWindowsService#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#2)]
    [!code-vb[c_SecureWindowsService#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsservice/vb/secureservice.vb#2)]

### <a name="using-the-binding-in-a-client"></a>在客户端中使用该绑定

此过程演示如何生成与服务进行通信的代理。 该代理使用的[元数据实用工具（Svcutil.exe）](servicemodel-metadata-utility-tool-svcutil-exe.md)生成，该工具使用服务元数据来创建代理。

此过程还创建 <xref:System.ServiceModel.WSHttpBinding> 类的实例以便与服务进行通信，然后调用服务。

本示例仅使用代码来创建客户端。 另一种方法是使用配置文件，如此过程之后的一节所示。

#### <a name="to-use-a-binding-in-a-client-with-code"></a>通过代码在客户端中使用绑定

1. 使用 SvcUtil.exe 工具根据服务的元数据生成代理代码。 有关详细信息，请参阅[如何：创建客户端](how-to-create-a-wcf-client.md)。 生成的代理代码是从类继承的 <xref:System.ServiceModel.ClientBase%601> ，这可确保每个客户端都具有所需的构造函数、方法和属性以与 WCF 服务进行通信。 在本示例中，生成的代码包括 `CalculatorClient` 类，该类实现了 `ICalculator` 接口，以便与服务代码兼容。

2. 在客户端程序的 `Main` 方法的开头插入此过程的代码。

3. 创建 <xref:System.ServiceModel.WSHttpBinding> 类的一个实例，并且将其安全模式设置为 `Message`，将其客户端凭据类型设置为 `Windows`。 本示例将变量命名为 `clientBinding`。

4. 创建 <xref:System.ServiceModel.EndpointAddress> 类的一个实例，该实例名为 `serviceAddress`。 将基址与终结点名称连接起来以初始化该实例。

5. 使用 `serviceAddress` 和 `clientBinding` 变量创建所生成的客户端类的一个实例。

6. 调用 <xref:System.ServiceModel.ClientBase%601.Open%2A> 方法，如下面的代码所示。

7. 调用服务并显示结果。

    [!code-csharp[c_secureWindowsClient#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#1)]
    [!code-vb[c_secureWindowsClient#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsclient/vb/secureclient.vb#1)]

## <a name="using-the-configuration-file"></a>使用配置文件

如果不用程序代码创建绑定，还可以使用下面所示的配置文件绑定节的代码。

如果还没有定义服务，请参阅[设计和实现服务](designing-and-implementing-services.md)和[配置服务](configuring-services.md)。

> [!NOTE]
> 此配置代码在服务和客户端配置文件中使用。

#### <a name="to-enable-transfer-security-on-a-service-in-a-windows-domain-using-configuration"></a>使用配置在 Windows 域中的服务上启用传输安全

1. 将一个 [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) 元素添加到 [\<bindings>](../configure-apps/file-schema/wcf/bindings.md) 配置文件的元素部分。

2. 将 <`binding`> 元素添加到 <`WSHttpBinding`> 元素，并将 `configurationName` 属性设置为适合于应用程序的值。

3. 添加一个 <`security`> 元素，并将 `mode` 属性设置为 Message。

4. 添加一个 <`message`> 元素，并将 `clientCredentialType` 属性设置为 Windows。

5. 在服务的配置文件中，使用下面的代码替换 `<bindings>` 节。 如果还没有服务配置文件，请参阅[使用绑定配置服务和客户端](using-bindings-to-configure-services-and-clients.md)。

    ```xml
    <bindings>
      <wsHttpBinding>
       <binding name = "wsHttpBinding_Calculator">
         <security mode="Message">
           <message clientCredentialType="Windows"/>
         </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    ```

### <a name="using-the-binding-in-a-client"></a>在客户端中使用该绑定

此过程演示如何生成两个文件：一个与服务进行通信的代理和一个配置文件。 此过程还描述对客户端程序所做的更改，这是在客户端使用的第三个文件。

#### <a name="to-use-a-binding-in-a-client-with-configuration"></a>通过配置在客户端中使用绑定

1. 使用 SvcUtil.exe 工具根据服务的元数据生成代理代码和配置文件。 有关详细信息，请参阅[如何：创建客户端](how-to-create-a-wcf-client.md)。

2. [\<bindings>](../configure-apps/file-schema/wcf/bindings.md)将生成的配置文件的部分替换为上一节中的配置代码。

3. 在客户端程序的 `Main` 方法的开头插入程序代码。

4. 通过将配置文件中的绑定名称作为输入参数进行传递，创建所生成的客户端类的实例。

5. 调用 <xref:System.ServiceModel.ClientBase%601.Open%2A> 方法，如下面的代码所示。

6. 调用服务并显示结果。

    [!code-csharp[c_secureWindowsClient#2](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#2)]

## <a name="example"></a>示例

[!code-csharp[c_SecureWindowsService#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsservice/cs/secureservice.cs#0)]

[!code-csharp[c_SecureWindowsClient#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_securewindowsclient/cs/secureclient.cs#0)]
[!code-vb[c_SecureWindowsClient#0](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_securewindowsclient/vb/secureclient.vb#0)]

## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WSHttpBinding>
- [ServiceModel 元数据实用工具 (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [如何：创建客户端](how-to-create-a-wcf-client.md)
- [保证服务的安全](securing-services.md)
- [安全性概述](./feature-details/security-overview.md)
