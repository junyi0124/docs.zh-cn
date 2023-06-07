---
title: 匿名消息安全
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: c321cbf9-8c05-4cce-b5a5-4bf7b230ee03
ms.openlocfilehash: 4349b53ca86c0ed8bd7e0527ad1e903543f56631
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294395"
---
# <a name="message-security-anonymous"></a>匿名消息安全

消息安全匿名示例演示如何实现 Windows Communication Foundation (WCF) 应用程序，该应用程序使用不带客户端身份验证的消息级安全性，但需要使用服务器的 x.509 证书进行服务器身份验证。 客户端与服务器之间的所有应用程序消息均已进行签名和加密。 此示例基于 [WSHttpBinding](wshttpbinding.md) 示例。 此示例由客户端控制台程序 (.exe) 和 Internet 信息服务 (IIS) 所承载的服务库 (.dll) 组成。 该服务实现定义“请求-答复”通信模式的协定。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

 如果不对客户端进行身份验证，则本示例向返回 `True` 的计算器接口添加新运算。

```csharp
public class CalculatorService : ICalculator
{
    public bool IsCallerAnonymous()
    {
        // ServiceSecurityContext.IsAnonymous returns true if the caller is not authenticated.
        return ServiceSecurityContext.Current.IsAnonymous;
    }
    ...
}
```

 服务公开单一终结点，以便与使用配置文件 (Web.config) 定义的服务进行通信。 终结点由地址、绑定和协定组成。 此绑定是用 `wsHttpBinding` 绑定配置的。 `wsHttpBinding` 绑定的默认安全模式是 `Message`。 `clientCredentialType` 属性设置为 `None`。

```xml
<system.serviceModel>

  <protocolMapping>
    <add scheme="http" binding="wsHttpBinding" />
  </protocolMapping>

  <bindings>
    <wsHttpBinding>
     <!-- This configuration defines the security mode as Message and -->
     <!-- the clientCredentialType as None. This mode provides -->
     <!-- server authentication only using the service certificate. -->

     <binding>
       <security mode="Message">
         <message clientCredentialType="None" />
       </security>
     </binding>
    </wsHttpBinding>
  </bindings>
  ...
</system.serviceModel>
```

 要用于服务身份验证的凭据在中指定 [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) 。 服务器证书包含的 `SubjectName` 的值必须与为 `findValue` 属性指定的值相同，如下面的示例代码所示。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior>
      <!--
    The serviceCredentials behavior allows you to define a service certificate.
    A service certificate is used by a client to authenticate the service and provide message protection.
    This configuration references the "localhost" certificate installed during the setup instructions.
    -->
      <serviceCredentials>
        <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
      </serviceCredentials>
      <serviceMetadata httpGetEnabled="True"/>
      <serviceDebug includeExceptionDetailInFaults="False" />
    </behavior>
  </serviceBehaviors>
</behaviors>
```

 客户端终结点配置由服务终结点的绝对地址、绑定和协定组成。 `wsHttpBinding` 绑定的客户端安全模式为 `Message`。 `clientCredentialType` 属性设置为 `None`。

```xml
<system.serviceModel>
  <client>
    <endpoint name=""
             address="http://localhost/servicemodelsamples/service.svc"
             binding="wsHttpBinding"
             behaviorConfiguration="ClientCredentialsBehavior"
             bindingConfiguration="Binding1"
             contract="Microsoft.ServiceModel.Samples.ICalculator" />
  </client>

  <bindings>
    <wsHttpBinding>
      <!--This configuration defines the security mode as -->
      <!--Message and the clientCredentialType as None. -->
      <binding name="Binding1">
        <security mode = "Message">
          <message clientCredentialType="None"/>
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>
  ...
</system.serviceModel>
```

 本示例将 <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A> 设置为 <xref:System.ServiceModel.Security.X509CertificateValidationMode.PeerOrChainTrust> 以便对服务的证书进行身份验证。 这是在`behaviors`一节中的客户端的 App.config 文件中实现的。 这意味着如果证书在用户的“受信任人”存储中，则信任此证书，而不对证书的颁发者链执行身份验证。 此处使用的设置是为了方便起见，使示例可以不需要证书颁发机构 (CA) 颁发的证书就能运行。 此设置没有默认设置 ChainTrust 安全。 在生产代码中使用 `PeerOrChainTrust` 之前，应仔细考虑此设置的安全含义。

 客户端实现添加对方法的调用 `IsCallerAnonymous` ，否则不同于 [WSHttpBinding](wshttpbinding.md) 示例。

```csharp
// Create a client with a client endpoint configuration.
CalculatorClient client = new CalculatorClient();

// Call the GetCallerIdentity operation.
Console.WriteLine("IsCallerAnonymous returned: {0}", client.IsCallerAnonymous());

// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = client.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

...

//Closing the client gracefully closes the connection and cleans up resources.
client.Close();

Console.WriteLine();
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
IsCallerAnonymous returned: True
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714
Press <ENTER> to terminate client.
```

 “匿名消息安全”示例随附的 Setup.bat 批处理文件使您可以用相关的证书来配置服务器，以运行所承载的需要基于证书的安全的应用程序。 可以在两种模式中运行此批处理文件。 若要在单一计算机模式下运行该批处理文件，请在命令行键入 `setup.bat`。 若要在服务模式中运行此文件，请键入 `setup.bat service`。 跨计算机运行示例时，请使用此模式。 有关详细信息，请参见本主题末尾的设置过程。

 下面提供了有关此批处理文件的不同部分的简要概述：

- 创建服务器证书。

     Setup.bat 批处理文件中的以下行创建将要使用的服务器证书。

    ```bat
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

     %SERVER_NAME% 变量指定服务器名称。 该证书存储在 LocalMachine 存储区中。 如果用服务参数（如 `setup.bat service`）运行 setup 批处理文件，则 %SERVER_NAME% 包含计算机的完全限定域名。 否则，它默认为 localhost。

- 将服务器证书安装到客户端的受信任证书存储区中。

     以下行将服务器证书复制到客户端的受信任人存储中。 因为客户端系统不隐式信任 Makecert.exe 生成的证书，所以需要执行此步骤。 如果已经拥有一个证书，该证书来源于客户端的受信任根证书（例如由 Microsoft 颁发的证书），则不需要执行使用服务器证书填充客户端证书存储区这一步骤。

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

- 授予对证书私钥的权限。

     Setup.bat 批处理文件中的以下行使 ASP.NET 工作进程帐户可以访问存储在 LocalMachine 存储区中的服务器证书。

    ```bat
    echo ************
    echo setting privileges on server certificates
    echo ************
    for /F "delims=" %%i in ('"%MSSDK%\bin\FindPrivateKey.exe" My LocalMachine -n CN^=%SERVER_NAME% -a') do set PRIVATE_KEY_FILE=%%i set WP_ACCOUNT=NT AUTHORITY\NETWORK SERVICE
    (ver | findstr "5.1") && set WP_ACCOUNT=%COMPUTERNAME%\ASPNET
    echo Y|cacls.exe "%PRIVATE_KEY_FILE%" /E /G "%WP_ACCOUNT%":R
    iisreset
    ```

> [!NOTE]
> 如果你使用的是非美国Windows 英文版你必须编辑 Setup.bat 文件，并将 `NT AUTHORITY\NETWORK SERVICE` 帐户名称替换为你的区域性等效项。

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例

1. 请确保路径包括 Makecert.exe 和 FindPrivateKey.exe 所在的文件夹。

2. 从 Visual Studio 的开发人员命令提示中的示例安装文件夹运行 Setup.bat，并使用管理员权限运行。 这将安装运行示例所需的所有证书。

    > [!NOTE]
    > 安装批处理文件设计为从 Visual Studio 的开发人员命令提示中运行。 这要求路径环境变量指向 SDK 的安装目录。 在 Visual Studio 开发人员命令提示中自动设置此环境变量。  
  
3. 通过输入地址来验证使用浏览器访问服务 `http://localhost/servicemodelsamples/service.svc` 。  
  
4. 启动 \client\bin 中的 Client.exe。 客户端活动将显示在客户端控制台应用程序上。  
  
5. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在服务计算机上创建目录。 使用 Internet Information Services (IIS) 管理工具为此目录创建名为 servicemodelsamples 的虚拟应用程序。  
  
2. 将服务程序文件从 \inetpub\wwwroot\servicemodelsamples 复制到服务计算机上的虚拟目录中。 确保复制 \bin 子目录中的文件。 另外，将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。  
  
3. 在客户端计算机上为这些客户端二进制文件创建一个目录。  
  
4. 将客户端程序文件复制到客户端计算机上的客户端目录中。 另外，将 Setup.bat、Cleanup.bat 和 ImportServiceCert.bat 文件复制到客户端上。  
  
5. 在服务器上， `setup.bat service` 在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行。 使用参数运行将使用 `setup.bat` `service` 计算机的完全限定的域名创建一个服务证书，并将服务证书导出到名为的文件。  
  
6. 编辑 Web.config，以反映) 的属性中 (新的证书名称，该名称与 `findValue` [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) 计算机的完全限定域名相同。  
  
7. 将服务目录中的 Service.cer 文件复制到客户端计算机上的客户端目录中。  
  
8. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。  
  
9. 在客户端上，在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行 ImportServiceCert.bat。 这会将 Service.cer 文件中的服务证书导入 CurrentUser – TrustedPeople 存储区。  
  
10. 在客户端计算机上，在命令提示符下启动 Client.exe。 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 运行完示例后运行示例文件夹中的 Cleanup.bat。  
  
> [!NOTE]
> 此脚本不会在跨计算机运行此示例时移除客户端上的服务证书。 如果你已运行 Windows Communication Foundation 跨计算机使用证书 (WCF) 示例，请确保清除已安装在 CurrentUser-TrustedPeople 存储中的服务证书。 为此，请使用以下命令：`certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>`，例如：`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com.`。
