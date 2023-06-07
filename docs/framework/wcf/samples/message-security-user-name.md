---
title: 用户名消息安全
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: c63cfc87-6b20-4949-93b3-bcd4b732b0a2
ms.openlocfilehash: 8e1cf38a6906ca1c15c0dc00fc8866955a326f3d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294360"
---
# <a name="message-security-user-name"></a>用户名消息安全

本示例演示如何实现一个应用程序，该应用程序对客户端使用具有用户名身份验证的 WS-Security，并要求使用服务器的 X.509v3 证书对服务器进行身份验证。 客户端与服务器之间的所有应用程序消息均已进行签名和加密。 默认情况下，使用客户端提供的用户名和密码登录有效的 Windows 帐户。 此示例基于 [WSHttpBinding](wshttpbinding.md)。 本示例由客户端控制台程序 (Client.exe) 和 Internet 信息服务 (IIS) 所承载的服务库 (Service.dll) 组成。 该服务实现定义“请求-答复”通信模式的协定。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 本示例还演示：  
  
- 到 Windows 帐户的默认映射，以便可以执行其他授权。  
  
- 如何访问服务代码中的调用方标识信息。  
  
 服务公开一个终结点以便与服务进行通信，该终结点是使用配置文件 Web.config 来定义的。终结点由地址、绑定和协定组成。 绑定是使用标准配置的 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) ，默认情况下使用消息安全性。 此示例将标准设置 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 为使用客户端用户名身份验证。 该行为指定使用用户凭据对服务进行身份验证。 服务器证书必须包含与中的属性相同的使用者名称值 `findValue` [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) 。  
  
```xml  
<system.serviceModel>  
  <protocolMapping>  
    <add scheme="http" binding="wsHttpBinding" />  
  </protocolMapping>  
  <bindings>  
    <wsHttpBinding>  
      <!--   
      This configuration defines the security mode as Message and   
      the clientCredentialType as Username.  
      By default, Username authentication attempts to authenticate the provided  
      username as a Windows computer or domain account.  
      -->  
      <binding>  
        <security mode="Message">  
          <message clientCredentialType="UserName"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true.-->  
  <behaviors>  
    <serviceBehaviors>  
      <behavior>  
        <!--   
      The serviceCredentials behavior allows one to define a service certificate.  
      A service certificate is used by the service to authenticate itself to the client and to provide message protection.  
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
</system.serviceModel>  
```  
  
 客户端终结点配置由服务终结点的绝对地址、绑定和协定组成。 该客户端绑定是使用相应的 `securityMode` 和 `authenticationMode` 配置的。 在跨计算机方案中运行时，必须相应更改服务终结点地址。  
  
```xml  
<system.serviceModel>  
  <client>  
    <endpoint address="http://localhost/servicemodelsamples/service.svc"
              binding="wsHttpBinding"
              bindingConfiguration="Binding1"
              behaviorConfiguration="ClientCredentialsBehavior"  
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
  </client>  
  
  <bindings>  
    <wsHttpBinding>  
      <!--   
      This configuration defines the security mode as Message and   
      the clientCredentialType as Username.  
      -->  
      <binding name="Binding1">  
        <security mode="Message">  
          <message clientCredentialType="UserName"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true.-->  
  <behaviors>  
    <endpointBehaviors>  
      <behavior name="ClientCredentialsBehavior">  
        <!--   
      Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
      is in the user's Trusted People store, then it is trusted without performing a  
      validation of the certificate's issuer chain. This setting is used here for convenience so that the   
      sample can be run without having to have certificates issued by a certification authority (CA).  
      This setting is less secure than the default, ChainTrust. The security implications of this   
      setting should be carefully considered before using PeerOrChainTrust in production code.   
      -->  
        <clientCredentials>  
          <serviceCertificate>  
            <authentication certificateValidationMode="PeerOrChainTrust" />  
          </serviceCertificate>  
        </clientCredentials>  
      </behavior>  
    </endpointBehaviors>  
  </behaviors>  
</system.serviceModel>  
```  
  
 客户端实现设置要使用的用户名和密码。  

```csharp
// Create a client.  
CalculatorClient client = new CalculatorClient();  
  
// Configure client with valid computer or domain account (username,password).  
client.ClientCredentials.UserName.UserName = username;  
client.ClientCredentials.UserName.Password = password.ToString();  
  
// Call GetCallerIdentity service operation.  
Console.WriteLine(client.GetCallerIdentity());  
...  
//Closing the client gracefully closes the connection and cleans up resources.  
client.Close();  
```

 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。  
  
```console  
MyMachine\TestAccount  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
 通过运行消息安全示例随附的 Setup.bat 批处理文件，您可以用相关的证书将服务器配置为运行承载的应用程序，这些应用程序需要基于证书的安全性。 可以在两种模式中运行此批处理文件。 若要在单一计算机模式下运行该批处理文件，请在命令行键入 `setup.bat`。 若要在服务模式下运行该批处理文件，请键入 `setup.bat service`。 跨计算机运行示例时，请使用此模式。 有关详细信息，请参见本主题末尾的设置过程。  
  
 下面提供该批处理文件不同部分的简要概述。  
  
- 创建服务器证书  
  
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
  
     %SERVER_NAME% 变量指定服务器名称。 该证书存储在 LocalMachine 存储区中。 如果用服务自变量（如 `setup.bat service`）运行 Setup.bat 批处理文件，则 %SERVER_NAME% 包含计算机的完全限定域名。  否则，它默认为 localhost。  
  
- 将服务器证书安装到客户端的受信任证书存储区中  
  
     以下行将服务器证书复制到客户端的受信任人存储中。 因为客户端系统不隐式信任 Makecert.exe 生成的证书，所以需要执行此步骤。 如果已经拥有一个证书，该证书来源于客户端的受信任根证书（例如由 Microsoft 颁发的证书），则不需要执行使用服务器证书填充客户端证书存储区这一步骤。  
  
    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
- 授予对证书私钥的权限  
  
     Setup.bat 批处理文件中的以下行使 ASP.NET 工作进程帐户可以访问存储在 LocalMachine 存储区中的服务器证书。  
  
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
    > 如果你使用的是非美国Windows 英文版你必须编辑 Setup.bat 文件，并将 `NT AUTHORITY\NETWORK SERVICE` 帐户名称替换为你的区域性等效项。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例  
  
1. 请确保路径包括 Makecert.exe 和 FindPrivateKey.exe 所在的文件夹。  
  
2. 从 Visual Studio 的开发人员命令提示中的示例安装文件夹运行 Setup.bat，并使用管理员权限打开 Visual Studio。 这将安装运行示例所需的所有证书。  
  
    > [!NOTE]
    > Setup.bat 批处理文件设计为从 Visual Studio 的开发人员命令提示运行。 这要求路径环境变量指向 SDK 的安装目录。 在 Visual Studio 开发人员命令提示中自动设置此环境变量。  
  
3. 通过输入地址来验证使用浏览器访问服务 `http://localhost/servicemodelsamples/service.svc` 。
  
4. 启动 \client\bin 中的 Client.exe。 客户端活动将显示在客户端控制台应用程序上。  
  
5. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在服务计算机上创建目录。 使用 Internet Information Services 管理工具为此目录创建名为 servicemodelsamples 的虚拟应用程序。  
  
2. 将服务程序文件从 \inetpub\wwwroot\servicemodelsamples 复制到服务计算机上的虚拟目录中。 确保复制 \bin 子目录中的文件。 另外，将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。  
  
3. 在客户端计算机上为这些客户端二进制文件创建一个目录。  
  
4. 将客户端程序文件复制到客户端计算机上的客户端目录中。 另外，将 Setup.bat、Cleanup.bat 和 ImportServiceCert.bat 文件复制到客户端上。  
  
5. 在服务器上， `setup.bat service` 在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行。 使用参数运行将使用 `setup.bat` `service` 计算机的完全限定的域名创建一个服务证书，并将服务证书导出到名为的文件。  
  
6. 编辑 Web.config 以反映新的证书名称（在 serviceCertificate 元素的 findValue 属性中），该名称与计算机的完全限定域名相同。  
  
7. 将服务目录中的 Service.cer 文件复制到客户端计算机上的客户端目录中。  
  
8. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。  
  
9. 在客户端上，在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行 ImportServiceCert.bat。 这会将 Service.cer 文件中的服务证书导入 CurrentUser – TrustedPeople 存储区。  
  
10. 在客户端计算机上，在命令提示符下启动 Client.exe。 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 运行完示例后运行示例文件夹中的 Cleanup.bat。  
  
    > [!NOTE]
    > 此脚本不会在跨计算机运行此示例时移除客户端上的服务证书。 如果你已运行 Windows Communication Foundation 跨计算机使用证书 (WCF) 示例，请确保清除已安装在 CurrentUser-TrustedPeople 存储中的服务证书。 为此，请使用以下命令：`certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>`，例如：`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`。
