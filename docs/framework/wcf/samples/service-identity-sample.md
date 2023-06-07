---
title: 服务标识示例
ms.date: 03/30/2017
ms.assetid: 79fa8c1c-85bb-4b67-bc67-bfaf721303f8
ms.openlocfilehash: cc8edcd7bfee73b99225c19f20df8df1e062b7bf
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262536"
---
# <a name="service-identity-sample"></a>服务标识示例

此服务标识示例演示如何为服务设置标识。 客户端可以在设计时使用服务的元数据来检索标识，然后在运行时对服务的标识进行身份验证。 服务标识的概念是允许客户端在调用服务的任何操作之前对服务进行身份验证，从而保护客户端，防止进行未经身份验证的调用。 在安全连接中，服务还在允许客户端访问其之前对客户端的凭据进行身份验证，但这不是此示例要介绍的重点。 请参阅 [客户端](client.md) 中显示服务器身份验证的示例。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

 此示例介绍以下功能：

- 如何在服务的不同终结点上设置不同类型的标识。 每种标识具有不同的功能。 要使用哪种标识取决于终结点绑定中使用的安全凭据的类型。

- 可以在配置中通过声明方式设置标识，也可以在代码中强制设置标识。 通常，对于客户端和服务，应使用配置来设置标识。

- 如何在客户端设置自定义标识。 自定义标识通常是对现有标识类型的自定义，客户端可以通过它检查服务凭据中提供的其他声明信息，以便在调用服务之前做出身份验证决策。

    > [!NOTE]
    > 此示例检查称为 identity.com 的特定证书的标识以及此证书中包含的 RSA 密钥。 在客户端上的配置中使用证书和 RSA 标识类型时，获取这些值的简便方法是检查在其中序列化这些值的服务的 WSDL。

 下面的示例代码演示如何使用 WSHttpBinding 向证书的域名服务器 (DNS) 配置服务终结点的标识。

```csharp
//Create a service endpoint and set its identity to the certificate's DNS
WSHttpBinding wsAnonbinding = new WSHttpBinding (SecurityMode.Message);
// Client are Anonymous to the service
wsAnonbinding.Security.Message.ClientCredentialType = MessageCredentialType.None;
WServiceEndpoint ep = serviceHost.AddServiceEndpoint(typeof(ICalculator),wsAnonbinding, String.Empty);
EndpointAddress epa = new EndpointAddress(dnsrelativeAddress,EndpointIdentity.CreateDnsIdentity("identity.com"));
ep.Address = epa;
```

 还可以在 App.config 文件的配置中指定标识。 下面的示例演示如何为服务终结点设置 UPN（用户主要名称）标识。

```xml
<endpoint address="upnidentity"
        behaviorConfiguration=""
        binding="wsHttpBinding"
        bindingConfiguration="WSHttpBinding_Windows"
        name="WSHttpBinding_ICalculator_Windows"
        contract="Microsoft.ServiceModel.Samples.ICalculator">
  <!-- Set the UPN identity for this endpoint -->
  <identity>
      <userPrincipalName value="host\myservice.com" />
  </identity >
</endpoint>
```

 通过从 <xref:System.ServiceModel.EndpointIdentity> 和 <xref:System.ServiceModel.Security.IdentityVerifier> 类派生，可以在客户端设置自定义标识。 从概念上讲，可以将 <xref:System.ServiceModel.Security.IdentityVerifier> 类视为服务的 `AuthorizationManager` 类的客户端等效类。 下面的代码示例演示了 `OrgEndpointIdentity` 的实现，它用于存储组织名称，以便与服务器证书的主题名称相匹配。 对该组织名称的授权检查发生在 `CheckAccess` 类的 `CustomIdentityVerifier` 方法上。

```csharp
// This custom EndpointIdentity stores an organization name
public class OrgEndpointIdentity : EndpointIdentity
{
    private string orgClaim;
    public OrgEndpointIdentity(string orgName)
    {
        orgClaim = orgName;
    }
    public string OrganizationClaim
    {
        get { return orgClaim; }
        set { orgClaim = value; }
    }
}

//This custom IdentityVerifier uses the supplied OrgEndpointIdentity to
//check that X.509 certificate's distinguished name claim contains
//the organization name e.g. the string value "O=Contoso"
class CustomIdentityVerifier : IdentityVerifier
{
    public override bool CheckAccess(EndpointIdentity identity, AuthorizationContext authContext)
    {
        bool returnvalue = false;
        foreach (ClaimSet claimset in authContext.ClaimSets)
        {
            foreach (Claim claim in claimset)
            {
                if (claim.ClaimType == "http://schemas.microsoft.com/ws/2005/05/identity/claims/x500distinguishedname")
                {
                    X500DistinguishedName name = (X500DistinguishedName)claim.Resource;
                    if (name.Name.Contains(((OrgEndpointIdentity)identity).OrganizationClaim))
                    {
                        Console.WriteLine("Claim Type: {0}",claim.ClaimType);
                        Console.WriteLine("Right: {0}", claim.Right);
                        Console.WriteLine("Resource: {0}",claim.Resource);
                        Console.WriteLine();
                        returnvalue = true;
                    }
                }
            }
        }
        return returnvalue;
    }
}
```

 此示例使用称为 identity.com 的证书，此证书位于特定于语言的 Identity 解决方案文件夹中。

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例

1. 在 Windows XP 或 Windows Vista 上，使用 MMC 管理单元工具将标识解决方案文件夹中的 Identity 证书文件导入到 LocalMachine/My (个人) 证书存储区。 此文件受密码保护。 在导入过程中，将提示您输入密码。 `xyz`在 "密码" 框中键入。 有关详细信息，请参阅 [如何：通过 MMC 管理单元查看证书](../feature-details/how-to-view-certificates-with-the-mmc-snap-in.md) 主题。 完成此操作后，请使用管理员权限在 Visual Studio 开发人员命令提示中运行 Setup.bat，这会将此证书复制到 CurrentUser/受信任的人员存储区，以便在客户端上使用。

2. 在 Windows Server 2003 上，使用管理员权限在 Visual Studio 2012 命令提示符下从示例安装文件夹运行 Setup.bat。 这将安装运行示例所需的所有证书。

    > [!NOTE]
    > Setup.bat 批处理文件设计为在 Visual Studio 2012 命令提示符下运行。 在 Visual Studio 2012 命令提示符下设置的 PATH 环境变量指向包含 Setup.bat 脚本所需的可执行文件的目录。 请确保在运行完该示例后通过运行 Cleanup.bat 来移除证书。 其他安全示例使用相同的证书。  
  
3. 启动 \service\bin 目录中的 Service.exe。 确保该服务指示它已准备就绪，并显示按 \<Enter> 以终止服务的提示。  
  
4. 启动 \client\bin 目录中的 Client.exe，或在 Visual Studio 中按 F5 以生成并运行。 客户端活动将显示在客户端控制台应用程序上。  
  
5. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在生成该示例的客户端部分之前，请确保在 `CallServiceCustomClientIdentity` 方法中更改 Client.cs 文件中的服务终结点地址值。 然后生成该示例。  
  
2. 在服务计算机上创建目录。  
  
3. 将 service\bin 中的服务程序文件复制到服务计算机上的目录中。 另外，将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。  
  
4. 在客户端计算机上为这些客户端二进制文件创建一个目录。  
  
5. 将客户端程序文件复制到客户端计算机上的客户端目录中。 另外，将 Setup.bat、Cleanup.bat 和 ImportServiceCert.bat 文件复制到客户端上。  
  
6. 在服务上， `setup.bat service` 在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行。 使用参数运行将使用 `setup.bat` `service` 计算机的完全限定的域名创建一个服务证书，并将服务证书导出到名为的文件。  
  
7. 将服务目录中的 Service.cer 文件复制到客户端计算机上的客户端目录中。  
  
8. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。 必须更改多个实例。  
  
9. 在客户端上，在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行 ImportServiceCert.bat。 这会将 Service.cer 文件中的服务证书导入 CurrentUser – TrustedPeople 存储区。  
  
10. 在服务计算机上，在命令提示符下启动 Service.exe。  
  
11. 在客户端计算机上，在命令提示符下启动 Client.exe。 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 运行完示例后运行示例文件夹中的 Cleanup.bat。  
  
    > [!NOTE]
    > 此脚本不会在跨计算机运行此示例时移除客户端上的服务证书。 如果你已运行 Windows Communication Foundation 跨计算机使用证书 (WCF) 示例，请确保清除已安装在 CurrentUser-TrustedPeople 存储中的服务证书。 为此，请使用以下命令：`certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>`，例如：`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`。
