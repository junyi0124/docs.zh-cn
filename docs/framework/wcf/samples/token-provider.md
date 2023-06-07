---
title: 令牌提供程序
ms.date: 03/30/2017
ms.assetid: 947986cf-9946-4987-84e5-a14678d96edb
ms.openlocfilehash: e761f32ab26cf620b6ef1dddff2bcba53289af84
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262328"
---
# <a name="token-provider"></a>令牌提供程序

此示例演示如何实现自定义令牌提供程序。 Windows Communication Foundation 中的令牌提供程序 (WCF) 用于向安全基础结构提供凭据。 令牌提供程序一般检查目标并颁发相应的凭据，以使安全基础结构能够确保消息的安全。 WCF 附带了默认的凭据管理器令牌提供程序。 WCF 还附带了一个 CardSpace 令牌提供程序。 自定义令牌提供程序在下列情况下有用：

- 存在不能由这些令牌提供程序操作的凭据存储。

- 如果希望提供自己的自定义机制，以便在 WCF 客户端框架使用凭据时，从用户提供详细信息时开始转换凭据。

- 要生成一个自定义令牌。

 此示例演示如何生成一个自定义令牌提供程序，以便将用户的输入转换为另一种格式。

 总之，此示例将演示如下内容：

- 客户端如何使用用户名/密码对来进行身份验证。

- 如何使用自定义令牌提供程序对客户端进行配置。

- 服务器如何使用密码和自定义 <xref:System.IdentityModel.Selectors.UserNamePasswordValidator>（用来验证用户名和密码是否相匹配）来验证客户端凭据。

- 客户端如何使用服务器的 X.509 证书对服务器进行身份验证。

 此示例还演示在执行自定义令牌身份验证过程之后，如何访问调用方的标识。

 服务会公开单一终结点以便与使用 App.config 配置文件定义的服务进行通信。 终结点由地址、绑定和协定组成。 绑定是使用标准 `wsHttpBinding` 配置的，该元素在默认情况下使用消息安全性。 此示例将标准 `wsHttpBinding`设置为使用客户端用户名身份验证。 服务还使用 serviceCredentials 行为来配置服务证书。 使用 serviceCredentials 行为可以配置服务证书。 客户端使用服务证书对服务进行身份验证并提供消息保护。 以下配置引用了在示例设置过程中安装的 localhost 证书，如下面的设置说明中所述。

```xml
<system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- configure base address provided by host -->
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service"/>
          </baseAddresses>
        </host>
        <!-- use base address provided by host -->
        <endpoint address=""
                  binding="wsHttpBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
      </service>
    </services>

    <bindings>
      <wsHttpBinding>
        <binding name="Binding1">
          <security mode="Message">
            <message clientCredentialType="UserName" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>

    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceDebug includeExceptionDetailInFaults="False" />
          <!--
        The serviceCredentials behavior allows one to define a service certificate.
        A service certificate is used by a client to authenticate the service and provide message protection.
        This configuration references the "localhost" certificate installed during the setup instructions.
        -->
          <serviceCredentials>
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
```

 客户端终结点配置由配置名称、服务终结点的绝对地址、绑定和协定组成。 该客户端绑定是使用适当的 `Mode` 和消息 `clientCredentialType` 配置的。

```xml
<system.serviceModel>
  <client>
    <endpoint name=""
              address="http://localhost:8000/servicemodelsamples/service"
              binding="wsHttpBinding"
              bindingConfiguration="Binding1"
              contract="Microsoft.ServiceModel.Samples.ICalculator">
    </endpoint>
  </client>

  <bindings>
    <wsHttpBinding>
      <binding name="Binding1">
        <security mode="Message">
          <message clientCredentialType="UserName" />
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>
</system.serviceModel>
```

 以下步骤演示如何开发自定义令牌提供程序并将其与 WCF 安全框架集成：

1. 编写自定义令牌提供程序。

     此示例实现一个用来获取用户名和密码的自定义令牌提供程序。 密码必须与用户名相匹配。 这个自定义的令牌提供程序仅用于演示目的，不建议用在实际部署中。

     为了执行此任务，自定义令牌提供程序派生了 <xref:System.IdentityModel.Selectors.SecurityTokenProvider> 类，并重写了 <xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%28System.TimeSpan%29> 方法。 此方法创建并返回一个新的 `UserNameSecurityToken`。

    ```csharp
    protected override SecurityToken GetTokenCore(TimeSpan timeout)
    {
        // obtain username and password from the user using console window
        string username = GetUserName();
        string password = GetPassword();
        Console.WriteLine("username: {0}", username);

        // return new UserNameSecurityToken containing information obtained from user
        return new UserNameSecurityToken(username, password);
    }
    ```

2. 编写自定义安全令牌管理器。

     使用 <xref:System.IdentityModel.Selectors.SecurityTokenManager>，可以为在 <xref:System.IdentityModel.Selectors.SecurityTokenProvider> 方法中传入该管理器的特定 <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> 创建 `CreateSecurityTokenProvider`。 安全令牌管理器还用于创建令牌身份验证器和令牌序列化程序，但它们不包括在此示例中。 在此示例中，自定义安全令牌管理器继承自 <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> 类并重写 `CreateSecurityTokenProvider` 方法，这样，当所传递令牌的要求指示需要用户名提供程序时，将返回自定义用户名令牌提供程序。

    ```csharp
    public class MyUserNameSecurityTokenManager : ClientCredentialsSecurityTokenManager
    {
        MyUserNameClientCredentials myUserNameClientCredentials;

        public MyUserNameSecurityTokenManager(MyUserNameClientCredentials myUserNameClientCredentials)
            : base(myUserNameClientCredentials)
        {
            this.myUserNameClientCredentials = myUserNameClientCredentials;
        }

        public override SecurityTokenProvider CreateSecurityTokenProvider(SecurityTokenRequirement tokenRequirement)
        {
            // if token requirement matches username token return custom username token provider
            // otherwise use base implementation
            if (tokenRequirement.TokenType == SecurityTokenTypes.UserName)
            {
                return new MyUserNameTokenProvider();
            }
            else
            {
                return base.CreateSecurityTokenProvider(tokenRequirement);
            }
        }
    }
    ```

3. 编写自定义客户端凭据。

     客户端凭据类用于表示为客户端代理配置的凭据并创建一个安全令牌管理器，该管理器用于获取令牌身份验证器、令牌提供程序和令牌序列化程序。

    ```csharp
    public class MyUserNameClientCredentials : ClientCredentials
    {
        public MyUserNameClientCredentials()
            : base()
        {
        }

        protected override ClientCredentials CloneCore()
        {
            return new MyUserNameClientCredentials();
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            // return custom security token manager
            return new MyUserNameSecurityTokenManager(this);
        }
    }
    ```

4. 配置客户端以使用自定义客户端凭据。

     为了让客户端使用自定义客户端凭据，此示例删除了默认的客户端凭据类并提供了新的客户端凭据类。

    ```csharp
    static void Main()
    {
        // ...
           // Create a client with given client endpoint configuration
          CalculatorClient client = new CalculatorClient();

          // set new credentials
           client.ChannelFactory.Endpoint.Behaviors.Remove(typeof(ClientCredentials));
         client.ChannelFactory.Endpoint.Behaviors.Add(new MyUserNameClientCredentials());
       // ...
    }
    ```

 在服务上，若要显示调用方信息，请使用 <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A>，如下面的代码示例中所示。 <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> 包含有关当前调用方的声明信息。

```csharp
static void DisplayIdentityInformation()
{
    Console.WriteLine("\t\tSecurity context identity  :  {0}",
        ServiceSecurityContext.Current.PrimaryIdentity.Name);
}
```

 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

## <a name="setup-batch-file"></a>设置批处理文件

 通过运行此示例随附的 Setup.bat 批处理文件，可以用相关的证书将服务器配置为运行需要基于服务器证书的安全性的自承载应用程序。 必须修改此批处理文件，以便跨计算机或在非承载情况下工作。

 下面提供了批处理文件不同节的简要概述，以便可以修改批处理文件从而在相应的配置中运行：

- 创建服务器证书。

     Setup.bat 批处理文件中的以下行创建将要使用的服务器证书。 `%SERVER_NAME%`变量指定服务器名称。 更改此变量可以指定您自己的服务器名称。 此批处理文件中的默认值为 localhost。

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

> [!NOTE]
> Setup.bat 批处理文件设计为通过 Windows SDK 命令提示运行。 这要求 MSSDK 环境变量指向 SDK 的安装目录。 将在 Windows SDK 命令提示中自动设置此环境变量。

#### <a name="to-set-up-and-build-the-sample"></a>设置和生成示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

#### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例

1. 在使用管理员特权打开的 Visual Studio 2012 命令提示符下，从示例安装文件夹中运行 Setup.bat。 这将安装运行示例所需的所有证书。

    > [!NOTE]
    > Setup.bat 批处理文件设计为在 Visual Studio 2012 命令提示符下运行。 在 Visual Studio 2012 命令提示符下设置的 PATH 环境变量指向包含 Setup.bat 脚本所需的可执行文件的目录。  
  
2. 启动 service\bin 中的 service.exe。  
  
3. 启动 \client\bin 中的 Client.exe。 客户端活动将显示在客户端控制台应用程序上。  
  
4. 在用户名提示下，键入一个用户名。  
  
5. 在密码提示下，使用已在用户名提示下键入的字符串。  
  
6. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在服务计算机上为服务二进制文件创建一个目录。  
  
2. 将服务程序文件复制到服务计算机上的服务目录。 另外，将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。  
  
3. 必须具有一个其主题名称中包含计算机的完全限定域名的服务器证书。 必须更新 Service.exe.config 文件以反映此新证书名称。 可以通过修改 Setup.bat 批处理文件来创建服务器证书。 请注意，必须使用管理员权限打开 Visual Studio 开发人员命令提示的 setup.bat 文件。 必须将 `%SERVER_NAME%` 变量设置为用于承载服务的计算机的完全限定的主机名。  
  
4. 将服务器证书复制到客户端的 CurrentUser-TrustedPeople 存储中。 当服务器证书是由客户端的受信任颁发者颁发时，不必执行此操作。  
  
5. 在服务计算机的 Service.exe.config 文件中，更改基址的值以指定一个完全限定的计算机名称，而不是 localhost。  
  
6. 在服务计算机上，在命令提示符下运行 service.exe。  
  
7. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的客户端程序文件复制到客户端计算机上。  
  
8. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。  
  
9. 在客户端计算机上，从命令提示窗口中启动 `Client.exe`。  
  
10. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
1. 运行完示例后运行示例文件夹中的 Cleanup.bat。
