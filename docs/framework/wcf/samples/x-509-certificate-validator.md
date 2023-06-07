---
title: X.509 证书验证程序
ms.date: 03/30/2017
ms.assetid: 3b042379-02c4-4395-b927-e57c842fd3e0
ms.openlocfilehash: 2d117dda8e6748407b6d58677ec55e4e8e4e4740
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554236"
---
# <a name="x509-certificate-validator"></a>X.509 证书验证程序

此示例演示如何实现自定义 X.509 证书验证程序。 当内置的 X.509 证书验证模式都不能满足应用程序的要求时，实现自定义证书验证程序很有用。 此示例演示了具有自定义验证程序的服务，该验证程序接受自行颁发的证书。 客户端使用此类证书对服务进行身份验证。

注意：因为任何人都可以构造自行颁发的证书，所以服务使用的自定义验证程序的安全性低于 ChainTrust X509CertificateValidationMode 提供的默认行为。 在成品代码中使用此验证逻辑之前，应慎重考虑这样做的安全隐患。

概括而言，此示例演示：

- 如何使用 X.509 证书对客户端进行身份验证。

- 服务器如何根据自定义 X509CertificateValidator 验证客户端凭据。

- 如何使用服务器的 X.509 证书对服务器进行身份验证。

服务公开一个终结点，以便与使用配置文件 App.config 定义的服务进行通信。终结点由地址、绑定和协定组成。 绑定配置为使用 `wsHttpBinding` 默认为使用 `WSSecurity` 和客户端证书身份验证的标准。 服务行为指定对客户端 X.509 证书进行验证的“自定义”模式以及验证程序类的类型。 该行为还使用 serviceCertificate 元素指定服务器证书。 服务器证书必须包含与中相同的值 `SubjectName` `findValue` [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) 。

```xml
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <!-- use host/baseAddresses to configure base address -->
        <!-- provided by host -->
        <host>
          <baseAddresses>
            <add baseAddress =
                "http://localhost:8001/servicemodelsamples/service" />
          </baseAddresses>
        </host>
        <!-- use base address specified above, provide one endpoint -->
        <endpoint address="certificate"
               binding="wsHttpBinding"
               bindingConfiguration="Binding"
               contract="Microsoft.ServiceModel.Samples.ICalculator" />
      </service>
    </services>
    <bindings>
      <wsHttpBinding>
        <!-- X509 certificate binding -->
        <binding name="Binding">
          <security mode="Message">
            <message clientCredentialType="Certificate" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceDebug includeExceptionDetailInFaults ="true"/>
          <serviceCredentials>
            <!-- The serviceCredentials behavior allows one -->
            <!-- to specify authentication constraints on -->
            <!-- client certificates. -->
            <clientCertificate>
              <!-- Setting the certificateValidationMode to -->
              <!-- Custom means that if the custom -->
              <!-- X509CertificateValidator does NOT throw -->
              <!-- an exception, then the provided certificate -->
              <!-- will be trusted without performing any -->
              <!-- validation beyond that performed by the custom -->
              <!-- validator. The security implications of this -->
              <!-- setting should be carefully considered before -->
              <!-- using Custom in production code. -->
              <authentication
                 certificateValidationMode="Custom"
                 customCertificateValidatorType =
"Microsoft.ServiceModel.Samples.CustomX509CertificateValidator, service" />
            </clientCertificate>
            <!-- The serviceCredentials behavior allows one to -->
            <!-- define a service certificate. -->
            <!-- A service certificate is used by a client to -->
            <!-- authenticate the service and provide message -->
            <!-- protection. This configuration references the -->
            <!-- "localhost" certificate installed during the setup -->
            <!-- instructions. -->
            <serviceCertificate findValue="localhost"
                 storeLocation="LocalMachine"
                 storeName="My" x509FindType="FindBySubjectName" />
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
      <!-- X509 certificate based endpoint -->
      <endpoint name="Certificate"
        address=
        "http://localhost:8001/servicemodelsamples/service/certificate"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                behaviorConfiguration="ClientCertificateBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator">
      </endpoint>
    </client>
    <bindings>
        <wsHttpBinding>
            <!-- X509 certificate binding -->
            <binding name="Binding">
                <security mode="Message">
                    <message clientCredentialType="Certificate" />
               </security>
            </binding>
       </wsHttpBinding>
    </bindings>
    <behaviors>
      <endpointBehaviors>
        <behavior name="ClientCertificateBehavior">
          <clientCredentials>
            <serviceCertificate>
              <!-- Setting the certificateValidationMode to -->
              <!-- PeerOrChainTrust means that if the certificate -->
              <!-- is in the user's Trusted People store, then it -->
              <!-- is trusted without performing a validation of -->
              <!-- the certificate's issuer chain. -->
              <!-- This setting is used here for convenience so -->
              <!-- that the sample can be run without having to -->
              <!-- have certificates issued by a certification -->
              <!-- authority (CA). This setting is less secure -->
              <!-- than the default, ChainTrust. The security -->
              <!-- implications of this setting should be -->
              <!-- carefully considered before using -->
              <!-- PeerOrChainTrust in production code.-->
              <authentication
                  certificateValidationMode="PeerOrChainTrust" />
            </serviceCertificate>
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>
  </system.serviceModel>
```

客户端实现设置要使用的客户端证书。

```csharp
// Create a client with Certificate endpoint configuration
CalculatorClient client = new CalculatorClient("Certificate");
try
{
    client.ClientCredentials.ClientCertificate.SetCertificate(StoreLocation.CurrentUser, StoreName.My, X509FindType.FindBySubjectName, "test1");

    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = client.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

    // Call the Subtract service operation.
    value1 = 145.00D;
    value2 = 76.54D;
    result = client.Subtract(value1, value2);
    Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);

    // Call the Multiply service operation.
    value1 = 9.00D;
    value2 = 81.25D;
    result = client.Multiply(value1, value2);
    Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);

    // Call the Divide service operation.
    value1 = 22.00D;
    value2 = 7.00D;
    result = client.Divide(value1, value2);
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);
    client.Close();
}
catch (TimeoutException e)
{
    Console.WriteLine("Call timed out : {0}", e.Message);
    client.Abort();
}
catch (CommunicationException e)
{
    Console.WriteLine("Call failed : {0}", e.Message);
    client.Abort();
}
catch (Exception e)
{
    Console.WriteLine("Call failed : {0}", e.Message);
    client.Abort();
}
```

此示例使用自定义 X509CertificateValidator 来验证证书。 此示例实现从 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 派生的 CustomX509CertificateValidator。 有关更多信息，请参见有关 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 的文档。 这个特定的自定义验证程序示例实现了 Validate 方法，可以接受自行颁发的任何 X.509 证书，如下面的代码所示。

```csharp
public class CustomX509CertificateValidator : X509CertificateValidator
{
  public override void Validate ( X509Certificate2 certificate )
  {
   // Only accept self-issued certificates
   if (certificate.Subject != certificate.Issuer)
     throw new Exception("Certificate is not self-issued");
   }
}
```

 在服务代码中实现验证程序后，必须通知服务主机关于要使用的验证程序实例的信息。 这是使用以下代码完成的。

```csharp
serviceHost.Credentials.ClientCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.Custom;
serviceHost.Credentials.ClientCertificate.Authentication.CustomCertificateValidator = new CustomX509CertificateValidator();
```

 或者，您可以在如下配置中执行相同的操作。

```xml
<behaviors>
    <serviceBehaviors>
     <behavior name="CalculatorServiceBehavior">
       ...
   <serviceCredentials>
    <!--The serviceCredentials behavior allows one to specify -->
    <!--authentication constraints on client certificates.-->
    <clientCertificate>
    <!-- Setting the certificateValidationMode to Custom means -->
    <!--that if the custom X509CertificateValidator does NOT -->
    <!--throw an exception, then the provided certificate will -->
    <!--be trusted without performing any validation beyond that -->
    <!--performed by the custom validator. The security -->
    <!--implications of this setting should be carefully -->
    <!--considered before using Custom in production code. -->
    <authentication certificateValidationMode="Custom"
       customCertificateValidatorType =
"Microsoft.ServiceModel.Samples. CustomX509CertificateValidator, service" />
   </clientCertificate>
   </serviceCredentials>
   ...
  </behavior>
 </serviceBehaviors>
</behaviors>
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 客户端应成功地调用所有方法。 在客户端窗口中按 Enter 可以关闭客户端。

## <a name="setup-batch-file"></a>设置批处理文件

通过运行此示例随附的 Setup.bat 批处理文件，可以用相关的证书将服务器配置为运行需要基于服务器证书的安全性的自承载应用程序。 必须修改此批处理文件，以便跨计算机或在非承载情况下工作。

下面提供了批处理文件不同节的简要概述，以便可以修改批处理文件从而在相应的配置中运行：

- 创建服务器证书：

     Setup.bat 批处理文件中的以下行创建将要使用的服务器证书。 %SERVER_NAME% 变量指定服务器名称。 更改此变量可以指定您自己的服务器名称。 默认值为 localhost。

    ```bash
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

    ```bash
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

- 创建客户端证书：

     Setup.bat 批处理文件中的以下行创建将要使用的客户端证书。 %USER_NAME% 变量指定客户端名称。 此值设置为“test1”，因为这是客户端代码查找的名称。 如果更改 %USER_NAME% 的值，必须更改 Client.cs 源文件中对应的值并重新生成客户端。

     证书存储在 CurrentUser 存储位置下的 My（个人）存储区中。

    ```bash
    echo ************
    echo Client cert setup starting
    echo %USER_NAME%
    echo ************
    echo making client cert
    echo ************
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%USER_NAME% -sky exchange -pe
    ```

- 将客户端证书安装到服务器的受信任证书存储区中：

     Setup.bat 批处理文件中的以下行将客户端证书复制到受信任的人的存储区中。 因为服务器系统不隐式信任 Makecert.exe 生成的证书，所以需要执行此步骤。 如果您已经拥有一个证书，该证书来源于受信任的根证书（例如由 Microsoft 颁发的证书），则不需要执行使用客户端证书填充服务器证书存储区这一步骤。

    ```bash
    certmgr.exe -add -r CurrentUser -s My -c -n %USER_NAME% -r LocalMachine -s TrustedPeople
    ```

#### <a name="to-set-up-and-build-the-sample"></a>设置和生成示例

1. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

2. 若要用单一计算机配置或跨计算机配置来运行示例，请按照下列说明进行操作。

#### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例

1. 在使用管理员特权打开的 Visual Studio 2012 命令提示符下，从示例安装文件夹运行 Setup.bat。 这将安装运行示例所需的所有证书。

    > [!IMPORTANT]
    > Setup.bat 批处理文件设计为在 Visual Studio 2012 命令提示符下运行。 在 Visual Studio 2012 命令提示符下设置的 PATH 环境变量指向包含 Setup.bat 脚本所需的可执行文件的目录。

2. 启动 service\bin 中的 Service.exe。

3. 启动 \client\bin 中的 Client.exe。 客户端活动将显示在客户端控制台应用程序上。

4. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。

#### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例

1. 在服务计算机上创建目录。

2. 从 \service\bin 中将服务程序文件复制到服务计算机上的虚拟目录中。 另外，将 Setup.bat、Cleanup.bat、GetComputerName.vbs 和 ImportClientCert.bat 文件复制到服务计算机上。

3. 在客户端计算机上为这些客户端二进制文件创建一个目录。

4. 将客户端程序文件复制到客户端计算机上的客户端目录中。 另外，将 Setup.bat、Cleanup.bat 和 ImportServiceCert.bat 文件复制到客户端上。

5. 在服务器上， `setup.bat service` 在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行。 使用参数运行将使用 `setup.bat` `service` 计算机的完全限定的域名创建一个服务证书，并将服务证书导出到名为的文件。

6. 编辑 Service.exe.config，以反映) 的属性中 (新的证书名称，该名称与 `findValue` [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) 计算机的完全限定域名相同。 还要将元素中的计算机名 \<service> / \<baseAddresses> 从 localhost 更改为服务计算机的完全限定名称。

7. 将服务目录中的 Service.cer 文件复制到客户端计算机上的客户端目录中。

8. 在客户端上， `setup.bat client` 在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行。 如果使用 `setup.bat` 自变量运行 `client`，则会创建一个名为 client.com 的客户端证书，并将此客户端证书导出到名为 Client.cer 的文件中。

9. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。 通过用服务器的完全限定域名替换 localhost 来执行此操作。

10. 将客户端目录中的 Client.cer 文件复制到服务器上的服务目录中。

11. 在客户端上，在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行 ImportServiceCert.bat。 这会将 Service.cer 文件中的服务证书导入 CurrentUser – TrustedPeople 存储区。

12. 在服务器上，在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行 ImportClientCert.bat。 这会将 Client.cer 文件中的客户端证书导入 LocalMachine - TrustedPeople 存储区。

13. 在服务器计算机上，从命令提示窗口中启动 Service.exe。

14. 在客户端计算机上，从命令提示窗口中启动 Client.exe。 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。

#### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理

1. 运行完示例后运行示例文件夹中的 Cleanup.bat。 这将从证书存储区中移除服务器和客户端证书。

> [!NOTE]
> 此脚本不会在跨计算机运行此示例时移除客户端上的服务证书。 如果你已运行 Windows Communication Foundation 跨计算机使用证书 (WCF) 示例，请确保清除已安装在 CurrentUser-TrustedPeople 存储中的服务证书。 为此，请使用以下命令：`certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>`，例如：`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`。
