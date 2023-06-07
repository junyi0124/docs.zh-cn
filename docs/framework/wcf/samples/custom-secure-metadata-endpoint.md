---
title: 自定义安全元数据终结点
ms.date: 03/30/2017
ms.assetid: 9e369e99-ea4a-49ff-aed2-9fdf61091a48
ms.openlocfilehash: d699c8dedfec3068bf0fc1504b47d78fde1988dc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240896"
---
# <a name="custom-secure-metadata-endpoint"></a>自定义安全元数据终结点

此示例演示如何实现一个具有安全元数据终结点（该终结点使用一个非元数据交换绑定）的服务，以及如何配置 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 或客户端从这类元数据终结点获取元数据的安全元数据终结点。 有两个系统提供的绑定可供公开元数据终结点：mexHttpBinding 和 mexHttpsBinding。 mexHttpBinding 用于以非安全的方式，通过 HTTP 公开元数据终结点。 mexHttpsBinding 用于以安全的方式，通过 HTTP 公开元数据终结点。 本示例演示如何使用 <xref:System.ServiceModel.WSHttpBinding> 公开安全元数据终结点。 要更改绑定的安全设置但不想使用 HTTPS 时需要这样做。 如果使用 mexHttpsBinding，则元数据终结点是安全的，但无法修改绑定设置。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
## <a name="service"></a>服务  

 本示例中的服务有两个终结点。 应用程序终结点可用于 `ICalculator` 上的 `WSHttpBinding` 协定，其中启用了 `ReliableSession` 并且 `Message` 安全性为使用证书。 元数据终结点还使用具有相同安全设置但没有 `WSHttpBinding` 的 `ReliableSession`。 下面是相关的配置：  
  
```xml  
<services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
             behaviorConfiguration="CalculatorServiceBehavior">  
     <!-- use base address provided by host -->  
     <endpoint address=""  
       binding="wsHttpBinding"  
       bindingConfiguration="Binding2"  
       contract="Microsoft.ServiceModel.Samples.ICalculator" />  
     <endpoint address="mex"  
       binding="wsHttpBinding"  
       bindingConfiguration="Binding1"  
       contract="IMetadataExchange" />  
     </service>  
 </services>  
 <bindings>  
   <wsHttpBinding>  
     <binding name="Binding1">  
       <security mode="Message">  
         <message clientCredentialType="Certificate" />  
       </security>  
     </binding>  
     <binding name="Binding2">  
       <reliableSession inactivityTimeout="00:01:00" enabled="true" />  
       <security mode="Message">  
         <message clientCredentialType="Certificate" />  
       </security>  
     </binding>  
   </wsHttpBinding>  
 </bindings>  
```  
  
 在很多其他示例中，元数据终结点使用默认 `mexHttpBinding`，这样会不安全。 下面是使用具有 `WSHttpBinding` 安全性的 `Message` 来保护的元数据。 为了使元数据客户端能够检索此元数据，必须用匹配的绑定来配置这些客户端。 本示例演示两个此类客户端。  
  
 第一个客户端使用 Svcutil.exe 提取元数据并在设计时生成客户端代码和配置。 由于服务针对元数据使用非默认绑定，因此必须专门配置 Svcutil.exe 工具，以便此工具能够使用该绑定从服务中获取元数据。  
  
 第二个客户端使用 `MetadataResolver` 动态提取已知协定的元数据，然后在动态生成的客户端上调用操作。  
  
## <a name="svcutil-client"></a>Svcutil 客户端  

 当使用默认绑定承载您的 `IMetadataExchange` 终结点时，可以使用该终结点的地址运行 Svcutil.exe：  
  
```console  
svcutil http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 它正常工作。 但在本示例中，服务器使用非默认终结点承载元数据。 因此，必须指示 Svcutil.exe 使用正确的绑定。 使用 Svcutil.exe.config 文件可以实现此目标。  
  
 Svcutil.exe.config 文件类似于普通的客户端配置文件。 唯一不同之处是客户端终结点名称和协定：  
  
```xml  
<endpoint name="http"  
          binding="wsHttpBinding"  
          bindingConfiguration="Binding1"  
          behaviorConfiguration="ClientCertificateBehavior"  
          contract="IMetadataExchange" />  
```  
  
 终结点名称必须是承载该元数据的地址的方案名称，终结点协定必须为 `IMetadataExchange`。 因此，当使用如下所示的命令行运行 Svcutil.exe 时：  
  
```console  
svcutil http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 它查找名为“http”的终结点和 `IMetadataExchange` 协定，以使用元数据终结点配置通信交换的绑定和行为。 示例中的 Svcutil.exe.config 文件的其余部分指定绑定配置和行为凭据以与元数据终结点的服务器配置相匹配。  
  
 为了使 Svcutil.exe 能够在 Svcutil.exe.config 中选取配置，Svcutil.exe 必须与该配置文件位于同一目录中。 因此，您必须将 Svcutil.exe 从其安装位置复制到包含 Svcutil.exe.config 文件的目录中。 然后，从该目录中运行以下命令：  
  
```console  
.\svcutil.exe http://localhost/servicemodelsamples/service.svc/mex  
```  
  
 前导 ". \\ "确保在此目录中 Svcutil.exe 的副本 (具有相应 Svcutil.exe.config) 运行的副本。  
  
## <a name="metadataresolver-client"></a>MetadataResolver 客户端  

 如果客户端在设计时知道协定和如何与元数据交谈，客户端就可以使用 `MetadataResolver` 动态找到应用程序终结点的绑定和地址。 本示例客户端对此进行了演示，显示如何通过创建和配置 `MetadataResolver` 来配置 `MetadataExchangeClient` 使用的绑定和凭据。  
  
 可以在 `MetadataExchangeClient` 上强制指定 Svcutil.exe.config 中出现的相同绑定和证书信息：  
  
```csharp  
// Specify the Metadata Exchange binding and its security mode  
WSHttpBinding mexBinding = new WSHttpBinding(SecurityMode.Message);  
mexBinding.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;  
  
// Create a MetadataExchangeClient for retrieving metadata, and set the // certificate details  
MetadataExchangeClient mexClient = new MetadataExchangeClient(mexBinding);  
mexClient.SoapCredentials.ClientCertificate.SetCertificate(    StoreLocation.CurrentUser, StoreName.My,  
    X509FindType.FindBySubjectName, "client.com");  
mexClient.SoapCredentials.ServiceCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.PeerOrChainTrust;  
mexClient.SoapCredentials.ServiceCertificate.SetDefaultCertificate(    StoreLocation.CurrentUser, StoreName.TrustedPeople,  
    X509FindType.FindBySubjectName, "localhost");  
```  
  
 配置完 `mexClient`，我们可以枚举感兴趣的协定并使用 `MetadataResolver` 提取使用这些协定的终结点的列表：  
  
```csharp  
// The contract we want to fetch metadata for  
Collection<ContractDescription> contracts = new Collection<ContractDescription>();  
ContractDescription contract = ContractDescription.GetContract(typeof(ICalculator));  
contracts.Add(contract);  
// Find endpoints for that contract  
EndpointAddress mexAddress = new EndpointAddress(ConfigurationManager.AppSettings["mexAddress"]);  
ServiceEndpointCollection endpoints = MetadataResolver.Resolve(contracts, mexAddress, mexClient);  
```  
  
 最后，我们可以使用这些终结点中的信息初始化 `ChannelFactory`（用于创建与应用程序终结点通信的通道）的绑定和地址。  
  
```csharp  
ChannelFactory<ICalculator> cf = new ChannelFactory<ICalculator>(endpoint.Binding, endpoint.Address);  
```  
  
 本示例的关键之处在于演示：如果你要使用 `MetadataResolver`，则必须指定用于元数据交换通信的自定义绑定或行为，你可以使用 `MetadataExchangeClient` 指定这些自定义设置。  
  
#### <a name="to-set-up-and-build-the-sample"></a>设置和生成示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
#### <a name="to-run-the-sample-on-the-same-machine"></a>在同一计算机上运行示例  
  
1. 运行示例安装文件夹中的 Setup.bat。 这将安装运行示例所需的所有证书。 请注意，Setup.bat 使用 FindPrivateKey.exe 工具，该工具是通过 [在 Windows Communication Foundation 示例的一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)中运行 setupCertTool.bat 而安装的。  
  
2. 运行 \MetadataResolverClient\bin 或 \SvcutilClient\bin 中的客户端应用程序。 客户端活动将显示在客户端控制台应用程序上。  
  
3. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
4. 在运行完该示例后运行 Cleanup.bat 移除证书。 其他安全示例使用相同的证书。  
  
#### <a name="to-run-the-sample-across-machines"></a>跨计算机运行示例  
  
1. 在服务器上运行 `setup.bat service`。 `setup.bat`使用参数运行将 `service` 使用计算机的完全限定的域名创建一个服务证书，并将服务证书导出到名为 .cer 的文件中。  
  
2. 在服务器上，编辑 Web.config 以反映新证书名称。 也就是说，将 `findValue` 元素中的属性更改 [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) 为计算机的完全限定域名。  
  
3. 将服务目录中的 Service.cer 文件复制到客户端计算机上的客户端目录中。  
  
4. 在客户端上，运行 `setup.bat client`。 `setup.bat`使用参数运行将 `client` 创建一个名为 Client.com 的客户端证书，并将客户端证书导出到名为的文件。  
  
5. 在客户端计算机上的 `MetadataResolverClient` 的 App.config 文件中，更改 Mex 终结点的地址值以与服务的新地址相匹配。 通过使用服务器的完全限定域名替换 localhost 来执行此操作。 还要将 metadataResolverClient.cs 文件中出现的“localhost”更改为新的服务证书名称（服务器的完全限定域名）。 对 SvcutilClient 项目的 App.config 执行相同的操作。  
  
6. 将客户端目录中的 Client.cer 文件复制到服务器上的服务目录中。  
  
7. 在客户端上，运行 `ImportServiceCert.bat`。 这会将 Service.cer 文件中的服务证书导入 CurrentUser – TrustedPeople 存储区。  
  
8. 在服务器上，运行 `ImportClientCert.bat`，这会将 Client.cer 文件中的客户端证书导入 LocalMachine - TrustedPeople 存储区。  
  
9. 在服务计算机上，在 Visual Studio 中生成服务项目，在 Web 浏览器中选择帮助页可验证该项目是否在运行。  
  
10. 在客户端计算机上，运行 VS 中的 MetadataResolverClient 或 SvcutilClient。  
  
    1. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 运行完示例后运行示例文件夹中的 Cleanup.bat。  
  
    > [!NOTE]
    > 此脚本不会在跨计算机运行此示例时移除客户端上的服务证书。 如果已运行 Windows Communication Foundation 跨计算机使用证书 (WCF) 示例，请确保清除已安装在 CurrentUser-TrustedPeople 存储中的服务证书。 为此，请使用以下命令：`certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>`。 例如：`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Metadata\CustomMexEndpoint`
