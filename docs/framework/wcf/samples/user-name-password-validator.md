---
title: 用户名密码验证程序
ms.date: 03/30/2017
ms.assetid: 42f03841-286b-42d8-ba58-18c75422bc8e
ms.openlocfilehash: bf20c9baaec44ebdfed351b35c54ab14448c7644
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96294919"
---
# <a name="user-name-password-validator"></a>用户名密码验证程序

此示例演示如何实现自定义 UserNamePassword 验证程序。 这在以下情况中非常有用：所有内置 UserNamePassword 验证模式均不符合应用程序的需求；例如，当用户名/密码对存储在某个外部存储区（如数据库）中时。 此示例显示了一个具有自定义验证程序（可检查两个特定用户名/密码对）的服务。 客户端使用此种用户名/密码对来对服务进行身份验证。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Security\UserNamePasswordValidator`  
  
> [!NOTE]
> 由于任何人都可以构建使用自定义验证程序接受的用户名/密码对的用户名凭据，因此标准 UserNamePassword 验证程序提供的默认行为比服务更安全。 标准 UserNamePassword 验证程序尝试将提供的用户名/密码对映射到 Windows 帐户，如果此映射失败则身份验证失败。 此示例中的自定义 UserNamePassword 验证程序不得用于成品代码，仅作演示使用。

 概括而言，此示例演示：

- 如何使用用户名令牌对客户端进行身份验证。

- 服务器如何根据自定义 UserNamePasswordValidator 验证客户端凭据，以及如何将用户名和密码验证逻辑中的自定义错误传播到客户端。

- 如何使用服务器的 X.509 证书对服务器进行身份验证。

 服务公开一个终结点，以便与使用配置文件定义的服务进行通信，App.config。终结点由地址、绑定和协定组成。 绑定配置为使用 `wsHttpBinding` 默认为使用 WS-Security 和用户名身份验证的标准。 服务行为指定用以验证客户端用户名/密码对的 `Custom` 模式以及验证程序类的类型。 该行为还使用 `serviceCertificate` 元素指定服务器证书。 服务器证书必须包含与中相同的值 `SubjectName` `findValue` [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) 。

```xml
<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <!-- use host/baseAddresses to configure base address provided by host -->
      <host>
        <baseAddresses>
          <add baseAddress ="http://localhost:8001/servicemodelsamples/service" />
        </baseAddresses>
      </host>
      <!-- use base address specified above, provide one endpoint -->
      <endpoint address="username"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>

  <bindings>
    <wsHttpBinding>
      <!-- username binding -->
      <binding name="Binding">
        <security mode="Message">
          <message clientCredentialType="UserName" />
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>

  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceDebug includeExceptionDetailInFaults ="true"/>
        <serviceCredentials>
          <!--
          The serviceCredentials behavior allows one to
          specify a custom validator for username/password
          combinations.
          -->
          <userNameAuthentication userNamePasswordValidationMode="Custom"
                                  customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService+MyCustomUserNameValidator, service" />
          <!--
          The serviceCredentials behavior allows one to define a service certificate. A service certificate is used by a client to authenticate the service and provide message protection. You must specify a server certificate when passing username/passwords to encrypt the information as it is sent on the wire. Otherwise the username and password information would be sent as clear text. This configuration references the "localhost" certificate installed during the setup instructions.
          -->
          <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
        </serviceCredentials>
      </behavior>
    </serviceBehaviors>
  </behaviors>

</system.serviceModel>
```

 客户端终结点配置由配置名称、服务终结点的绝对地址、绑定和协定组成。 该客户端绑定是使用适当的模式和消息 `clientCredentialType` 配置的。

```xml
<system.serviceModel>

    <client>
      <!-- Username based endpoint -->
      <endpoint name="Username"
address="http://localhost:8001/servicemodelsamples/service/username"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                behaviorConfiguration="ClientCertificateBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator">
      </endpoint>
    </client>

    <bindings>
      <wsHttpBinding>
        <!-- Username binding -->
        <binding name="Binding">
          <security mode="Message">
            <message clientCredentialType="UserName" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <behaviors>
      <endpointBehaviors>
        <behavior name="ClientCertificateBehavior">
          <clientCredentials>
            <serviceCertificate>
              <!--
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
            is in the user's Trusted People store, then it will be trusted without performing a
            validation of the certificate's issuer chain. This setting is used here for convenience so that the
            sample can be run without having to have certificates issued by a certification authority (CA).
            This setting is less secure than the default, ChainTrust. The security implications of this
            setting should be carefully considered before using PeerOrChainTrust in production code.
            -->
              <authentication certificateValidationMode="PeerOrChainTrust" />
            </serviceCertificate>
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>

  </system.serviceModel>
```

 客户端实现提示用户输入用户名和密码。

```csharp
// Get the username and password
Console.WriteLine("Username authentication required.");
Console.WriteLine("Provide a username.");
Console.WriteLine("   Enter username: (test1)");
string username = Console.ReadLine();
Console.WriteLine("   Enter password:");
string password = "";
ConsoleKeyInfo info = Console.ReadKey(true);
while (info.Key != ConsoleKey.Enter)
{
    if (info.Key != ConsoleKey.Backspace)
    {
        if (info.KeyChar != '\0')
        {
            password += info.KeyChar;
        }
        info = Console.ReadKey(true);
    }
    else if (info.Key == ConsoleKey.Backspace)
    {
        if (password != "")
        {
            password = password.Substring(0, password.Length - 1);
        }
        info = Console.ReadKey(true);
    }
}
for (int i = 0; i < password.Length; i++)
{
    Console.Write("*");
}
Console.WriteLine();
// Create a proxy with Certificate endpoint configuration
CalculatorProxy proxy = new CalculatorProxy("Username")
try
{
  proxy.ClientCredentials.Username.Username = username;
  proxy.ClientCredentials.Username.Password = password;
    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = proxy.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
  }
  catch (Exception e)
  {
      Console.WriteLine("Call failed:");
      while (e != null)
      {
          Console.WriteLine("\t{0}", e.Message);
          e = e.InnerException;
      }
      proxy.Abort();
  }
}
```

 此示例使用自定义 UserNamePasswordValidator 来验证用户名/密码对。 此示例实现从 `CustomUserNamePasswordValidator` 派生的 <xref:System.IdentityModel.Selectors.UserNamePasswordValidator>。 有关更多信息，请参阅 <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> 的文档。 此特定自定义验证程序示例实现 `Validate` 方法，以接受两个特定用户名/密码对，如以下代码所示。

```csharp
public class CustomUserNameValidator : UserNamePasswordValidator
{
 // This method validates users. It allows in two users,
 // test1 and test2 with passwords 1tset and 2tset respectively.
 // This code is for illustration purposes only and
 // MUST NOT be used in a production environment because it
 // is NOT secure.
 public override void Validate(string userName, string password)
 {
  if (null == userName || null == password)
  {
   throw new ArgumentNullException();
  }

  if (!(userName == "test1" && password == "1tset") && !(userName == "test2" && password == "2tset"))
  {
   throw new FaultException("Unknown Username or Incorrect Password");
   }
  }
 }
```

 在服务代码中实现验证程序后，必须通知服务主机关于要使用的验证程序实例的信息。 这是使用以下代码完成的。

```csharp
serviceHost.Credentials.UserNameAuthentication.UserNamePasswordValidationMode = UserNamePasswordValidationMode.Custom;
serviceHost.Credentials. UserNameAuthentication.CustomUserNamePasswordValidator = new CustomUserNamePasswordValidator();
```

 或者，您可以在如下配置中执行相同的操作。

```xml
<behaviors>
 <serviceBehaviors>
  <behavior name="CalculatorServiceBehavior">
  ...
   <serviceCredentials>
    <!--
    The serviceCredentials behavior allows one to specify authentication constraints on username / password combinations.
    -->
    <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService+CustomUserNameValidator, service" />
   ...
   </serviceCredentials>
  </behavior>
 </serviceBehaviors>
</behaviors>
```

 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 客户端应成功地调用所有方法。 在客户端窗口中按 Enter 可以关闭客户端。

## <a name="setup-batch-file"></a>设置批处理文件

 通过运行此示例随附的 Setup.bat 批处理文件，可以用相关的证书将服务器配置为运行需要基于服务器证书的安全性的自承载应用程序。 此批处理文件必须经过修改才能跨计算机或在非自承载情况下工作。

 下面提供了批处理文件不同节的简要概述，以便可以修改批处理文件从而在相应的配置中运行。

- 创建服务器证书：

     Setup.bat 批处理文件中的以下行创建将要使用的服务器证书。 %SERVER_NAME% 变量指定服务器名称。 更改此变量可以指定您自己的服务器名称。 默认值为 localhost。

    ```console
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

- 将服务器证书安装到客户端的受信任证书存储区中：

     Setup.bat 批处理文件中的以下行将服务器证书复制到客户端的受信任的人的存储区中。 因为客户端系统不隐式信任 Makecert.exe 生成的证书，所以需要执行此步骤。 如果您已经拥有一个证书，该证书来源于客户端的受信任根证书（例如由 Microsoft 颁发的证书），则不需要执行使用服务器证书填充客户端证书存储区这一步骤。

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

#### <a name="to-set-up-and-build-the-sample"></a>设置和生成示例

1. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

2. 若要用单一计算机配置或跨计算机配置来运行示例，请按照下列说明进行操作。

#### <a name="to-run-the-sample-on-the-same-machine"></a>在同一计算机上运行示例

1. 从 Visual Studio 2012 命令提示符下的示例安装文件夹中运行 Setup.bat。 这将安装运行示例所需的所有证书。

    > [!NOTE]
    > Setup.bat 批处理文件设计为在 Visual Studio 2012 命令提示符下运行。 在 Visual Studio 2012 命令提示符下设置的 PATH 环境变量指向包含 Setup.bat 脚本所需的可执行文件的目录。  
  
2. 启动 service\bin 中的 Service.exe。  
  
3. 启动 \client\bin 中的 Client.exe。 客户端活动将显示在客户端控制台应用程序上。  
  
4. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-run-the-sample-across-machines"></a>跨计算机运行示例  
  
1. 在服务计算机上为服务二进制文件创建一个目录。  
  
2. 将服务程序文件复制到服务计算机上的服务目录。 另外，将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。  
  
3. 需要有一个其主题名称中包含计算机的完全限定域名的服务器证书。 必须更新服务器的配置文件以反映此新证书名称。  
  
4. 将服务器证书复制到客户端的 CurrentUser-TrustedPeople 存储中。 只有当服务器证书不是由受信任的颁发者颁发的情况下才需要执行此操作。  
  
5. 在服务计算机的 App.config 文件中，更改基址的值以指定一个完全限定的计算机名称，而不是 localhost。  
  
6. 在服务计算机上，从命令提示符窗口中启动 Service.exe。  
  
7. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的客户端程序文件复制到客户端计算机上。  
  
8. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。  
  
9. 在客户端计算机上，从命令提示符窗口中启动 Client.exe。  
  
10. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
1. 运行完示例后运行示例文件夹中的 Cleanup.bat。 这将从证书存储区中移除服务器证书。
