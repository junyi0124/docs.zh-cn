---
title: 消息安全证书
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: 909333b3-35ec-48f0-baff-9a50161896f6
ms.openlocfilehash: 164a939fa7ee0112e1ceae24755854b09dc72603
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283986"
---
# <a name="message-security-certificate"></a>消息安全证书

此示例演示如何实现一个应用程序，该应用程序对客户端使用 WS 安全性和 X.509 v3 证书身份验证，并要求使用服务器的 X.509 v3 证书进行服务器身份验证。 此示例使用默认设置，以便客户端和服务器之间的所有应用程序消息都经过签名和加密。 此示例基于 [WSHttpBinding](wshttpbinding.md) ，由 INTERNET INFORMATION SERVICES (IIS) 中承载的客户端控制台程序和服务库组成。 该服务实现定义“请求-答复”通信模式的协定。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 此示例演示如何使用配置来控制身份验证，以及如何从安全上下文中获取调用方的标识，如下面的示例代码所述。  

```csharp
public class CalculatorService : ICalculator  
{  
    public string GetCallerIdentity()  
    {  
        // The client certificate is not mapped to a Windows identity by default.  
        // ServiceSecurityContext.PrimaryIdentity is populated based on the information  
        // in the certificate that the client used to authenticate itself to the service.  
        return ServiceSecurityContext.Current.PrimaryIdentity.Name;  
    }  
    ...  
}  
```  
  
 服务公开两个终结点，一个用来与使用配置文件 (Web.config) 定义的服务进行通信，另一个用来使用 WS-MetadataExchange 协议来公开服务的 WSDL 文档。 终结点由地址、绑定和协定组成。 绑定是使用标准元素配置的 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) ，该元素默认为使用消息安全性。 此示例将 `clientCredentialType` 属性设置为 Certificate 以要求进行客户端身份验证。  
  
```xml  
<system.serviceModel>  
    <protocolMapping>  
      <add scheme="http" binding="wsHttpBinding"/>  
    </protocolMapping>  
    <bindings>  
      <wsHttpBinding>  
        <!--   
        This configuration defines the security mode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding>  
          <security mode ="Message">  
            <message clientCredentialType="Certificate" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by the service to authenticate itself to its clients and to provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
```  
  
 此行为指定在客户端向服务进行身份验证时使用服务凭据。 服务器证书使用者名称是在元素的属性中指定的 `findValue` [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) 。  
  
```xml  
<!--For debugging purposes, set the includeExceptionDetailInFaults attribute to true.-->  
<behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by the service to authenticate itself to its clients and to provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
```  
  
 客户端终结点配置由服务终结点的绝对地址、绑定和协定组成。 客户端绑定是用相应的安全模式和身份验证模式配置的。 在跨计算机方案中运行时，请确保相应地更改服务终结点地址。  
  
```xml  
<system.serviceModel>  
    <client>  
      <!-- Use a behavior to configure the client certificate to present to the service. -->  
      <endpoint address="http://localhost/servicemodelsamples/service.svc" binding="wsHttpBinding" bindingConfiguration="Binding1" behaviorConfiguration="ClientCertificateBehavior" contract="Microsoft.Samples.Certificate.ICalculator"/>  
    </client>  
  
    <bindings>  
      <wsHttpBinding>  
        <!--   
        This configuration defines the security mode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="Certificate"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
...  
</system.serviceModel>  
```  
  
 客户端实现可以通过配置文件或通过代码来设置要使用的证书。 下面的示例演示如何设置要在配置文件中使用的证书。  
  
```xml  
<system.serviceModel>  
  ...  
  
<behaviors>  
      <endpointBehaviors>  
        <behavior name="ClientCertificateBehavior">  
          <!--   
        The clientCredentials behavior allows one to define a certificate to present to a service.  
        A certificate is used by a client to authenticate itself to the service and provide message integrity.  
        This configuration references the "client.com" certificate installed during the setup instructions.  
        -->  
          <clientCredentials>  
            <clientCertificate findValue="client.com" storeLocation="CurrentUser" storeName="My" x509FindType="FindBySubjectName"/>  
            <serviceCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certificate authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust"/>  
            </serviceCertificate>  
          </clientCredentials>  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
  
</system.serviceModel>  
```  
  
 下面的示例演示如何在程序中调用服务。  

```csharp
// Create a client.  
CalculatorClient client = new CalculatorClient();  
  
// Call the GetCallerIdentity service operation.  
Console.WriteLine(client.GetCallerIdentity());  
...  
//Closing the client gracefully closes the connection and cleans up resources.  
client.Close();  
```
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
CN=client.com  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
 通过运行消息安全示例随附的 Setup.bat 批处理文件，可以用相关的证书将客户端和服务器配置为运行承载应用程序，该应用程序需要基于证书的安全性。 该批处理文件可以在三个模式下运行。 若要在单一计算机模式下运行，请在 Visual Studio 的开发人员命令提示中 **setup.bat** 对于服务模式，请键入 **setup.bat 服务**;对于客户端模式类型 **setup.bat 客户端**。 在跨计算机运行示例时，可以使用客户端模式和服务器模式。 有关详细信息，请参见本主题末尾的设置过程。 下面提供了批处理文件不同节的简要概述，您可以修改这些批处理文件，使其能够在相应的配置中运行：  
  
- 创建客户端证书。  
  
     批处理文件中的以下行用于创建客户端证书。 创建的证书的主题名称中会使用指定的客户端名称。 证书存储在 `My` 存储位置下的 `CurrentUser` 存储区中。  
  
    ```bat
    echo ************  
    echo making client cert  
    echo ************  
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%CLIENT_NAME% -sky exchange -pe  
    ```  
  
- 将客户端证书安装到服务器的受信任证书存储区中。  
  
     批处理文件中的以下行将客户端证书复制到服务器的 TrustedPeople 存储中，以使服务器能够做出相关的信任或不信任决定。 为了使安装在 TrustedPeople 存储中的证书受 Windows Communication Foundation (WCF) 服务信任，必须将客户端证书验证模式设置为 `PeerOrChainTrust` 或 `PeerTrust` 。 请参见前面的服务配置示例，了解如何使用配置文件完成此操作。  
  
    ```bat
    echo ************  
    echo copying client cert to server's LocalMachine store  
    echo ************  
    certmgr.exe -add -r CurrentUser -s My -c -n %CLIENT_NAME% -r LocalMachine -s TrustedPeople
    ```  
  
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
  
     %SERVER_NAME% 变量指定服务器名称。 该证书存储在 LocalMachine 存储区中。 如果使用服务 (的参数（如）运行 Setup.bat 批处理文件， **setup.bat 服务**) % SERVER_NAME% 包含计算机的完全限定域名。 否则，它默认为 localhost。  
  
- 将服务器证书安装到客户端的受信任证书存储区中。  
  
     以下行将服务器证书复制到客户端的受信任人存储中。 因为客户端系统不隐式信任 Makecert.exe 生成的证书，所以需要执行此步骤。 如果已经拥有一个证书，该证书来源于客户端的受信任根证书（例如由 Microsoft 颁发的证书），则不需要执行使用服务器证书填充客户端证书存储区这一步骤。  
  
    ```console  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
- 授予对证书私钥的权限。  
  
     Setup.bat 文件中的以下行使 ASP.NET 工作进程帐户可以访问存储在 LocalMachine 存储区中的服务器证书。  
  
    ```bat
    echo ************  
    echo setting privileges on server certificates  
    echo ************  
    for /F "delims=" %%i in ('"%ProgramFiles%\ServiceModelSampleTools\FindPrivateKey.exe" My LocalMachine -n CN^=%SERVER_NAME% -a') do set PRIVATE_KEY_FILE=%%i  
    set WP_ACCOUNT=NT AUTHORITY\NETWORK SERVICE  
    (ver | findstr /C:"5.1") && set WP_ACCOUNT=%COMPUTERNAME%\ASPNET  
    echo Y|cacls.exe "%PRIVATE_KEY_FILE%" /E /G "%WP_ACCOUNT%":R  
    iisreset  
    ```  
  
    > [!NOTE]
    > 如果你使用的是非美国英语版本的 Windows 中，必须编辑 Setup.bat 文件，并将 "NT AUTHORITY\NETWORK SERVICE" 帐户名称替换为你的区域性等效项。  
  
> [!NOTE]
> 该批处理文件中使用的工具位于 C:\Program Files\Microsoft Visual Studio 8\Common7\tools 或 C:\Program Files\Microsoft SDKs\Windows\v6.0\bin 中。 这两个目录中必须有一个位于系统路径中。 如果安装了 Visual Studio，则在路径中获取此目录的最简单方法是打开 Visual Studio 的开发人员命令提示。 单击 " **开始**"，然后选择 " **所有程序**"、" **Visual Studio 2012**" 和 " **工具**"。 此命令提示中已经配置了相应的路径。 否则，您必须向路径中手动添加相应的目录。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\MessageSecurity`  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例  
  
1. 使用管理员权限打开 Visual Studio 开发人员命令提示，并从示例安装文件夹中运行 Setup.bat。 这将安装运行示例所需的所有证书。  
  
    > [!NOTE]
    > Setup.bat 批处理文件设计为从 Visual Studio 的开发人员命令提示运行。 这要求路径环境变量指向 SDK 的安装目录。 在 Visual Studio (2010) 的开发人员命令提示中自动设置此环境变量。  
  
2. 通过输入地址来验证使用浏览器访问服务 `http://localhost/servicemodelsamples/service.svc` 。  
  
3. 启动 \client\bin 中的 Client.exe。 客户端活动将显示在客户端控制台应用程序上。  
  
4. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在服务计算机上创建目录。 使用 Internet Information Services (IIS) 管理工具为此目录创建名为 servicemodelsamples 的虚拟应用程序。  
  
2. 将服务程序文件从 \inetpub\wwwroot\servicemodelsamples 复制到服务计算机上的虚拟目录中。 确保复制 \bin 子目录中的文件。 另外，将 Setup.bat、Cleanup.bat 和 ImportClientCert.bat 文件复制到服务计算机上。  
  
3. 在客户端计算机上为这些客户端二进制文件创建一个目录。  
  
4. 将客户端程序文件复制到客户端计算机上的客户端目录中。 另外，将 Setup.bat、Cleanup.bat 和 ImportServiceCert.bat 文件复制到客户端上。  
  
5. 在服务器上，使用管理员权限在 Visual Studio 开发人员命令提示中运行 **setup.bat 服务** 。 使用 **服务** 参数运行 **setup.bat** 将使用计算机的完全限定域名创建服务证书，并将服务证书导出到名为的文件。  
  
6. 编辑 Web.config，以反映) 的属性中 (新的证书名称，该名称与 `findValue` [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) 计算机的完全限定域名相同。  
  
7. 将服务目录中的 Service.cer 文件复制到客户端计算机上的客户端目录中。  
  
8. 在客户端上，使用管理员权限在 Visual Studio 的开发人员命令提示中运行 **setup.bat 客户端** 。 使用 **客户端** 参数运行 **setup.bat** 会创建一个名为 client.com 的客户端证书，并将客户端证书导出到名为的文件。  
  
9. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。 通过用服务器的完全限定域名替换 localhost 来执行此操作。  
  
10. 将客户端目录中的 Client.cer 文件复制到服务器上的服务目录中。  
  
11. 在客户端上，以具有管理权限的 Visual Studio 开发人员命令提示运行 ImportServiceCert.bat。 这会将 Service.cer 文件中的服务证书导入 CurrentUser – TrustedPeople 存储区。  
  
12. 在服务器上，以具有管理权限的 Visual Studio 开发人员命令提示运行 ImportClientCert.bat。 这会将 Client.cer 文件中的客户端证书导入 LocalMachine - TrustedPeople 存储区。  
  
13. 在客户端计算机上，从命令提示窗口中启动 Client.exe。 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 运行完示例后运行示例文件夹中的 Cleanup.bat。  
  
    > [!NOTE]
    > 此脚本不会在跨计算机运行此示例时移除客户端上的服务证书。 如果你已运行 Windows Communication Foundation 跨计算机使用证书 (WCF) 示例，请确保清除已安装在 CurrentUser-TrustedPeople 存储中的服务证书。 为此，请使用以下命令：`certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>`，例如：`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`。
