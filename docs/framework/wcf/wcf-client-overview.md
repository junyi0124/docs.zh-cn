---
title: WCF 客户端概述
description: 了解客户端应用程序的作用，如何配置、创建和使用 WCF 客户端，以及如何保护客户端应用程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- clients [WCF], architecture
ms.assetid: f60d9bc5-8ade-4471-8ecf-5a07a936c82d
ms.openlocfilehash: 1330861b0f6c1d6e41fdd4bba3876654c57d89f7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240779"
---
# <a name="wcf-client-overview"></a>WCF 客户端概述

本部分介绍什么是客户端应用程序、如何配置、创建和使用 Windows Communication Foundation (WCF) 客户端，以及如何保护客户端应用程序。  
  
## <a name="using-wcf-client-objects"></a>使用 WCF 客户端对象  

 客户端应用程序是一种托管应用程序，它使用 WCF 客户端与其他应用程序进行通信。 为 WCF 服务创建客户端应用程序需要执行以下步骤：  
  
1. 获取服务终结点的服务协定、绑定以及地址信息。  
  
2. 使用此信息创建 WCF 客户端。  
  
3. 调用操作。  
  
4. 关闭 WCF 客户端对象。  
  
以下部分将讨论上述这些步骤，并简单介绍以下问题：  
  
- 处理错误。  
  
- 配置和保护客户端。  
  
- 为双工服务创建回调对象。  
  
- 异步调用服务。  
  
- 使用客户端通道调用服务。  
  
## <a name="obtain-the-service-contract-bindings-and-addresses"></a>获取服务协定、绑定和地址  

 在 WCF 中，服务和客户端使用托管属性、接口和方法为协定建模。 若要连接客户端应用程序中的服务，则需要获取该服务协定的类型信息。 通常情况下，你可以使用 " [)  ( ](servicemodel-metadata-utility-tool-svcutil-exe.md)"，通过 " 实用工具从服务下载元数据，将其转换为所选语言的托管源代码文件，并创建可用于配置 WCF 客户端对象的客户端应用程序配置文件。 例如，如果要创建一个 WCF 客户端对象来调用 `MyCalculatorService` ，并且知道该服务的元数据已在 `http://computerName/MyCalculatorService/Service.svc?wsdl` 中发布，则下面的代码示例演示如何使用 Svcutil.exe `ClientCode.vb` 在托管代码中获取包含服务协定的文件。  
  
```console  
svcutil /language:vb /out:ClientCode.vb /config:app.config http://computerName/MyCalculatorService/Service.svc?wsdl  
```  
  
 可以将此协定代码编译为客户端应用程序或另一个程序集，客户端应用程序随后可以使用该程序集创建 WCF 客户端对象。 可以使用配置文件配置客户端对象以与服务正确连接。  
  
 有关此过程的示例，请参阅 [如何：创建客户端](how-to-create-a-wcf-client.md)。 有关协定的更完整信息，请参阅 [约定](./feature-details/contracts.md)。  
  
## <a name="create-a-wcf-client-object"></a>创建一个 WCF 客户端对象  

 WCF 客户端是一个本地对象，该对象表示一个窗体中的 WCF 服务，客户端可以使用该对象与远程服务进行通信。 WCF 客户端类型实现目标服务协定，因此，当你创建一个协定并配置它时，你可以直接使用该客户端对象来调用服务操作。 WCF 运行时将方法调用转换为消息，将其发送到服务，侦听答复，并将这些值作为返回值或参数返回给 WCF 客户端对象 `out` `ref` 。  
  
 你还可以使用 WCF 客户端信道对象连接和使用服务。 有关详细信息，请参阅 [WCF 客户端体系结构](./feature-details/client-architecture.md)。  
  
#### <a name="creating-a-new-wcf-object"></a>创建一个新的 WCF 对象  

 为了演示如何使用 <xref:System.ServiceModel.ClientBase%601> 类，现假设已从服务应用程序生成了下面的简单服务协定。  
  
> [!NOTE]
> 如果使用 Visual Studio 创建 WCF 客户端，则在你向项目添加服务引用时，会自动将对象加载到对象浏览器中。  
  
 [!code-csharp[C_GeneratedCodeFiles#12](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#12)]  
  
 如果未使用 Visual Studio，请检查生成的协定代码以查找扩展的类型 <xref:System.ServiceModel.ClientBase%601> 和服务协定接口 `ISampleService` 。 在这种情况下，该类型看上去类似下列代码：  
  
 [!code-csharp[C_GeneratedCodeFiles#14](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#14)]  
  
 可以通过使用其中一个构造函数将此类创建为一个本地对象，并对该本地对象进行配置，然后使用它连接到 `ISampleService` 类型的服务。  
  
 建议先创建 WCF 客户端对象，然后使用该对象，并在单个 try/catch 块内将其关闭。 不要使用 `using` `Using` Visual Basic) 中 (语句，因为它可以在某些失败模式下屏蔽异常。 有关详细信息，请参阅以下各节，并 [使用 Close 和 Abort 释放 WCF 客户端资源](./samples/use-close-abort-release-wcf-client-resources.md)。  
  
### <a name="contracts-bindings-and-addresses"></a>协定、绑定和地址  

 必须先配置客户端对象，然后才能创建 WCF 客户端对象。 具体来说，它必须具有要使用的服务 *终结点* 。 终结点由服务协定、绑定和地址组成。  (有关终结点的详细信息，请参阅 [终结点：地址、绑定和协定](./feature-details/endpoints-addresses-bindings-and-contracts.md)。 ) 通常，此信息位于 [\<endpoint>](../configure-apps/file-schema/wcf/endpoint-of-client.md) 客户端应用程序配置文件中的元素中，如 Svcutil.exe 工具生成的内容，并在创建客户端对象时自动加载。 这两个 WCF 客户端类型还具有重载，使你能够以编程方式指定此信息。  
  
 例如，上述示例中所使用的 `ISampleService` 的生成的配置文件包含以下终结点信息。  
  
 [!code-xml[C_GeneratedCodeFiles#19](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/common/client.exe.config#19)]  
  
 此配置文件在 `<client>` 元素中指定目标终结点。 有关使用多个目标终结点的详细信息，请参阅 <xref:System.ServiceModel.ClientBase%601.%23ctor%2A> 或 <xref:System.ServiceModel.ChannelFactory%601.%23ctor%2A> 构造函数。  
  
## <a name="calling-operations"></a>调用操作  

 创建并配置客户端对象后，创建一个 try/catch 块，调用操作的方式与在本地对象时相同，并关闭 WCF 客户端对象。 当客户端应用程序调用第一个操作时，WCF 将自动打开基础通道，并在回收对象时关闭基础通道。 （或者，还可以在调用其他操作之前或之后显式打开和关闭该通道。）  
  
 例如，如果您具有以下服务协定：  
  
```csharp  
namespace Microsoft.ServiceModel.Samples  
{  
    using System;  
    using System.ServiceModel;  
  
    [ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
    public interface ICalculator  
   {  
        [OperationContract]  
        double Add(double n1, double n2);  
        [OperationContract]  
        double Subtract(double n1, double n2);  
        [OperationContract]  
        double Multiply(double n1, double n2);  
        [OperationContract]  
        double Divide(double n1, double n2);  
    }  
}  
```  
  
```vb  
Namespace Microsoft.ServiceModel.Samples  
  
    Imports System  
    Imports System.ServiceModel  
  
    <ServiceContract(Namespace:= _  
    "http://Microsoft.ServiceModel.Samples")> _
   Public Interface ICalculator  
        <OperationContract> _
        Function Add(n1 As Double, n2 As Double) As Double  
        <OperationContract> _
        Function Subtract(n1 As Double, n2 As Double) As Double  
        <OperationContract> _
        Function Multiply(n1 As Double, n2 As Double) As Double  
        <OperationContract> _
     Function Divide(n1 As Double, n2 As Double) As Double  
End Interface  
```  
  
 可以通过创建 WCF 客户端对象并调用其方法来调用操作，如下面的代码示例所示。 WCF 客户端对象的打开、调用和关闭发生在单个 try/catch 块中。 有关详细信息，请参阅 [使用 Wcf 客户端访问服务](./feature-details/accessing-services-using-a-client.md) 和 [使用关闭和中止来释放 WCF 客户端资源](./samples/use-close-abort-release-wcf-client-resources.md)。  
  
 [!code-csharp[C_GeneratedCodeFiles#20](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#20)]  
  
## <a name="handling-errors"></a>处理错误  

 当打开基础客户端通道（无论是通过显式打开还是通过调用操作自动打开）、使用客户端或通道对象调用操作，或关闭基础客户端通道时，都会在客户端应用程序中出现异常。 除了由操作返回的 SOAP 错误导致引发的任何 <xref:System.TimeoutException?displayProperty=nameWithType> 对象外，建议您至少将应用程序设置为能够处理可能的 <xref:System.ServiceModel.CommunicationException?displayProperty=nameWithType> 和 <xref:System.ServiceModel.FaultException?displayProperty=nameWithType> 异常。 操作协定中指定的 SOAP 错误将作为 <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType> 在客户端应用程序中引发，此异常中的类型参数为 SOAP 错误的详细信息类型。 有关在客户端应用程序中处理错误条件的详细信息，请参阅 [发送和接收](sending-and-receiving-faults.md)错误。 有关演示如何在客户端中处理错误的完整示例，请参阅 [预期异常](./samples/expected-exceptions.md)。  
  
## <a name="configuring-and-securing-clients"></a>配置和保护客户端  

 若要配置客户端，请首先为客户端或通道对象加载目标终结点信息，通常是从配置文件中加载该信息，但是也可以使用客户端构造函数和属性以编程方式加载。 但是，若要启用特定的客户端行为或实施一些安全方案还需要执行其他配置步骤。  
  
 例如，服务协定的安全需求已在服务协定接口中声明，并且如果 Svcutil.exe 已创建了一个配置文件，则该文件通常会包含一个能够支持服务安全需求的绑定。 但是在某些情况中，可能需要更多的安全配置，例如配置客户端凭据。 有关 WCF 客户端安全配置的完整信息，请参阅 [保护客户端](securing-clients.md)。  
  
 此外，在客户端应用程序中还可以启用一些自定义修改，例如自定义运行时行为。 有关如何配置自定义客户端行为的详细信息，请参阅 [配置客户端行为](configuring-client-behaviors.md)。  
  
## <a name="creating-callback-objects-for-duplex-services"></a>为双工服务创建回调对象  

 双工服务指定一个回调协定，客户端应用程序必须实现该协定以便提供一个该服务能够根据协定要求调用的回调对象。 虽然回调对象不是完整的服务（例如，您无法使用回调对象启动一个通道），但是为了实现和配置，这些回调对象可以被视为一种服务。  
  
 双工服务的客户端必须：  
  
- 实现一个回调协定类。  
  
- 创建回调协定实现类的实例，并使用它来创建要 <xref:System.ServiceModel.InstanceContext?displayProperty=nameWithType> 传递给 WCF 客户端构造函数的对象。  
  
- 调用操作并处理操作回调。  
  
 双工 WCF 客户端对象的功能类似于它们的非双工，但它们公开支持回调所需的功能，包括回调服务的配置。  
  
 例如，可以通过使用回调类的 <xref:System.ServiceModel.CallbackBehaviorAttribute?displayProperty=nameWithType> 属性 (Attribute) 的属性 (Property)，控制回调对象运行时行为的各个方面。 另一个示例是使用 <xref:System.ServiceModel.Description.CallbackDebugBehavior?displayProperty=nameWithType> 类将异常信息返回给调用回调对象的服务。 有关详细信息，请参阅 [双工服务](./feature-details/duplex-services.md)。 有关完整示例，请参阅 [双工](./samples/duplex.md)。  
  
 在运行 Internet 信息服务 (IIS) 5.1 的 Windows XP 计算机上，双工客户端必须使用 <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType> 类指定一个客户端基址，否则会引发异常。 下面的代码示例演示如何在代码中执行此操作。  
  
 [!code-csharp[S_DualHttp#8](../../../samples/snippets/csharp/VS_Snippets_CFX/s_dualhttp/cs/program.cs#8)]
 [!code-vb[S_DualHttp#8](../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_dualhttp/vb/module1.vb#8)]  
  
 下面的代码演示如何在配置文件中执行此操作。  
  
 [!code-csharp[S_DualHttp#134](../../../samples/snippets/csharp/VS_Snippets_CFX/s_dualhttp/cs/program.cs#134)]  
  
## <a name="calling-services-asynchronously"></a>异步调用服务  

 如何调用操作完全取决于客户端开发人员。 这是因为当在托管代码中表示组成操作的消息时，这些消息可以映射到同步或异步方法中。 因此，如果想要生成异步调用操作的客户端，则可以使用 Svcutil.exe 通过 `/async` 选项生成异步客户端代码。 有关详细信息，请参阅 [如何：异步调用服务操作](./feature-details/how-to-call-wcf-service-operations-asynchronously.md)。  
  
## <a name="calling-services-using-wcf-client-channels"></a>使用 WCF 客户端通道调用服务  

 WCF 客户端类型扩展 <xref:System.ServiceModel.ClientBase%601> ，它本身派生自 <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> 接口以公开基础通道系统。 可以同时使用目标服务协定和 <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> 类来调用服务。 有关详细信息，请参阅 [WCF 客户端体系结构](./feature-details/client-architecture.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType>
