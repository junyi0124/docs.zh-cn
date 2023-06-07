---
title: 自定义绑定安全性
ms.date: 03/30/2017
ms.assetid: a6383dff-4308-46d2-bc6d-acd4e18b4b8d
ms.openlocfilehash: ce4cd76a053b9b3611751fe081d0ca710240049d
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555700"
---
# <a name="custom-binding-security"></a>自定义绑定安全性

本示例演示如何使用自定义绑定配置安全性。 并演示如何使用自定义绑定实现消息级安全性和安全传输。 如果在客户端和服务之间传输消息时需要进行安全的传输，同时消息必须在消息级别上保持安全，这非常有用。 系统提供的绑定不支持此配置。

本示例由客户端控制台程序 (EXE) 和服务控制台程序 (EXE) 组成。 该服务实现双工协定。 该协定由 `ICalculatorDuplex` 接口定义，该接口公开数学运算（加、减、乘和除）。 `ICalculatorDuplex` 接口允许客户端执行数学运算，通过会话计算运行结果。 服务可以独立地在 `ICalculatorDuplexCallback` 接口上返回结果。 双工协定需要会话，因为必须建立上下文才能将客户端和服务之间发送的一组消息关联在一起。 定义的自定义绑定支持双工通信并且是安全的。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

服务配置可定义支持以下内容的自定义绑定：

- 使用 TLS/SSL 协议保护的 TCP 通信。

- Windows 消息安全。

自定义绑定配置通过同时启用消息级安全性来启用安全传输。 在定义自定义绑定时，绑定元素的排序非常重要，因为每个元素都表示通道堆栈中的一个层 (请参阅 [自定义绑定](../extending/custom-bindings.md)) 。 自定义绑定在服务和客户端配置文件中进行定义，如下面的示例配置所示。

```xml
<bindings>
  <!-- Configure a custom binding. -->
  <customBinding>
    <binding name="Binding1">
      <security authenticationMode="SecureConversation"
                 requireSecurityContextCancellation="true">
      </security>
      <textMessageEncoding messageVersion="Soap12WSAddressing10" writeEncoding="utf-8"/>
      <sslStreamSecurity requireClientCertificate="false"/>
      <tcpTransport/>
    </binding>
  </customBinding>
</bindings>
```

此自定义绑定使用服务证书在传输级别对服务进行身份验证，并且在客户端与服务之间传输消息时保护消息。 这是通过 `sslStreamSecurity` 绑定元素实现的。 服务证书使用服务行为进行配置，如下面的示例配置所示。

```xml
<behaviors>
    <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
        <serviceMetadata />
        <serviceDebug includeExceptionDetailInFaults="False" />
        <serviceCredentials>
        <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName"/>
        </serviceCredentials>
    </behavior>
    </serviceBehaviors>
</behaviors>
```

此外，此自定义绑定将消息安全性与 Windows 凭据类型（默认凭据类型）一起使用。 这是通过 `security` 绑定元素实现的。 如果 Kerberos 身份验证机制可用，则使用消息级安全性对客户端和服务进行身份验证。 如果在 Active Directory 环境中运行示例，则会发生这种情况。 如果 Kerberos 身份验证机制不可用，则使用 NTLM 身份验证。 NTLM 向服务对客户端进行身份验证，但不向客户端对服务进行身份验证。 `security`绑定元素配置为使用 `SecureConversation` `authenticationType` ，这将导致在客户端和服务上创建安全会话。 为了使服务的双工协定起作用，需要这么做。

运行示例时，操作请求和响应将显示在客户端的控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
Press <ENTER> to terminate client.
Result(100)
Result(50)
Result(882.5)
Result(441.25)
Equation(0 + 100 - 50 * 17.65 / 2 = 441.25)
```

当运行示例时，在回调接口上可以看到从服务发送的、返回到客户端的消息。 显示每个中间结果，最后在所有操作完成时显示整个公式。 按 Enter 可关闭客户端。

通过运行所包含的 Setup.bat 文件，可以用相关的服务证书配置客户端和服务器，使其可以运行需要基于证书的安全性的承载应用程序。 必须修改此批处理文件，以便跨计算机或在非承载情况下工作。

下面提供了批处理文件中适用于本示例的各个节的简要概述，以便可以修改批处理文件从而在相应的配置下运行：

- 创建服务器证书。

  Setup.bat 文件中的以下行创建将要使用的服务器证书。 `%SERVER_NAME%`变量指定服务器名称。 更改此变量可以指定您自己的服务器名称。 此批处理文件将服务器名默认为 localhost。

  证书存储在 Web 承载的服务的 CurrentUser 存储中。

  ```bat
  echo ************
  echo Server cert setup starting
  echo %SERVER_NAME%
  echo ************
  echo making server cert
  echo ************
  makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
  ```

- 将服务器证书安装到客户端的受信任证书存储区中。

  Setup.bat 文件中的以下行将服务器证书复制到客户端的受信任人存储中。 因为客户端系统不隐式信任 Makecert.exe 生成的证书，所以需要执行此步骤。 如果已经拥有一个证书，该证书来源于客户端的受信任根证书（例如由 Microsoft 颁发的证书），则不需要执行使用服务器证书填充客户端证书存储区这一步骤。

  ```console
  certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
  ```

  > [!NOTE]
  > Setup.bat 批处理文件设计为通过 Visual Studio 2010 命令提示运行。 这要求 MSSDK 环境变量指向 SDK 的安装目录。 将在 Visual Studio 2010 命令提示中自动设置此环境变量。

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例

1. 使用管理员权限打开 Visual Studio 窗口的开发人员命令提示，并从示例安装文件夹中运行 Setup.bat。 这将安装运行示例所需的所有证书。

    > [!NOTE]
    > Setup.bat 批处理文件设计为在 Visual Studio 2012 命令提示符下运行。 在 Visual Studio 2012 命令提示符下设置的 PATH 环境变量指向包含 Setup.bat 脚本所需的可执行文件的目录。

2. 启动 \service\bin 中的 Service.exe。

3. 启动 \client\bin 中的 Client.exe。 客户端活动将显示在客户端控制台应用程序上。

4. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。

### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例

1. 在服务计算机上：

    1. 在服务计算机上创建一个名为 servicemodelsamples 的虚拟目录。

    2. 将服务程序文件从 \inetpub\wwwroot\servicemodelsamples 复制到服务计算机上的虚拟目录中。 确保复制 \bin 子目录中的文件。

    3. 将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。

    4. 在使用管理员权限打开的 Visual Studio 开发人员命令提示中运行以下命令： `Setup.bat service` 。 这会用与运行批处理文件的计算机的名称匹配的主题名称创建服务证书。

        > [!NOTE]
        > Setup.bat 批处理文件设计为通过 Visual Studio 2010 命令提示运行。 这要求路径环境变量指向 SDK 的安装目录。 将在 Visual Studio 2010 命令提示中自动设置此环境变量。

    5. 更改 [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) Service.exe.config 文件中的，以反映在上一步中生成的证书的使用者名称。

    6. 在命令提示符下运行 Service.exe。

2. 在客户端计算机上执行以下操作：

    1. 将 \client\bin\ 文件夹中的客户端程序文件复制到客户端计算机上。 还要复制 Cleanup.bat 文件。

    2. 运行 Cleanup.bat 以移除先前示例中使用的所有旧证书。

    3. 通过使用管理特权打开 Visual Studio 的开发人员命令提示，并在服务计算机上运行以下命令来导出服务的证书 (替换 `%SERVER_NAME%` 为运行服务的计算机的完全限定名称) ：

        ```console
        certmgr -put -r LocalMachine -s My -c -n %SERVER_NAME% %SERVER_NAME%.cer
        ```

    4. 将 %SERVER_NAME%.cer 复制到客户端计算机（用运行服务的计算机的完全限定名称替换 %SERVER_NAME%）。

    5. 通过使用管理特权打开 Visual Studio 的开发人员命令提示，然后在客户端计算机上运行以下命令来导入服务的证书 (使用运行该服务的计算机的完全限定名称替换% SERVER_NAME%) ：

        ```console
        certmgr.exe -add -c %SERVER_NAME%.cer -s -r CurrentUser TrustedPeople
        ```

        如果证书是由可信颁发者颁发的，则不需要执行步骤 c、d 和 e。

    6. 按如下所示修改客户端的 App.config 文件：

        ```xml
        <client>
            <endpoint name="default"
                address="net.tcp://ReplaceThisWithServiceMachineName:8000/ServiceModelSamples/Service"
                binding="customBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex"
                behaviorConfiguration="CalculatorClientBehavior" />
        </client>
        ```

    7. 如果在域环境中 NetworkService 或 LocalSystem 以外的帐户下运行服务，则可能需要修改客户端的 App.config 文件中服务终结点的终结点标识，以便基于用于运行服务的帐户来设置相应的 UPN 或 SPN。 有关终结点标识的详细信息，请参阅 [服务标识和身份验证](../feature-details/service-identity-and-authentication.md) 主题。

    8. 在命令提示符下运行 Client.exe。

### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理

- 运行完示例后运行示例文件夹中的 Cleanup.bat。
