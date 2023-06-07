---
title: 令牌身份验证器
ms.date: 03/30/2017
ms.assetid: 84382f2c-f6b1-4c32-82fa-aebc8f6064db
ms.openlocfilehash: ab3f8486229416465d7eca0b9413475d81318d32
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264629"
---
# <a name="token-authenticator"></a>令牌身份验证器

此示例演示如何实现一个自定义令牌身份验证器。 Windows Communication Foundation 中的令牌身份验证器 (WCF) 用于验证消息所使用的令牌，验证该消息是否为自一致，并对与令牌关联的标识进行身份验证。

 自定义令牌身份验证器可用在各种场合，如：

- 当您希望重写与令牌相关联的默认身份验证机制时。

- 当您生成自定义令牌时。

 此示例对下列内容进行了说明：

- 客户端如何使用用户名/密码对来进行身份验证。

- 服务器如何使用自定义令牌身份验证器来验证客户端凭据。

- WCF 服务代码如何与自定义令牌身份验证器结合。

- 如何使用服务器的 X.509 证书对服务器进行身份验证。

 此示例还演示如何在自定义令牌身份验证过程之后从 WCF 访问调用方的标识。

 服务会公开单一终结点以便与使用 App.config 配置文件定义的服务进行通信。 终结点由地址、绑定和协定组成。 绑定是在安全模式设置为消息（这是 `wsHttpBinding` 的默认模式）的情况下用标准 `wsHttpBinding` 配置的。 此示例将标准 `wsHttpBinding`设置为使用客户端用户名身份验证。 服务还使用 `serviceCredentials` 行为来配置服务证书。 使用 `securityCredentials` 行为可以指定服务证书。 客户端使用服务证书对服务进行身份验证并提供消息保护。 以下配置引用了在示例设置过程中安装的 localhost 证书，如下面的设置说明中所述。

```xml
<system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- configure base address provided by host -->
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service" />
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
....        -->
          <serviceCredentials>
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>
```

 客户端终结点配置由配置名称、服务终结点的绝对地址、绑定和协定组成。 该客户端绑定是使用相应的 `Mode` 和 `clientCredentialType` 配置的。

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

 客户端实现设置要使用的用户名和密码。

```csharp
static void Main()
{
     ...
     client.ClientCredentials.UserNamePassword.UserName = username;
     client.ClientCredentials.UserNamePassword.Password = password;
     ...
}
```

## <a name="custom-token-authenticator"></a>自定义令牌身份验证器

 使用下列步骤来创建自定义令牌身份验证器：

1. 编写自定义令牌身份验证器。

     此示例实现一个自定义令牌身份验证器，用来验证用户名是否具有有效的电子邮件格式。 它派生 <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator>。 此类中最重要的方法是 <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator.ValidateUserNamePasswordCore%28System.String%2CSystem.String%29>。 在该方法中，身份验证器验证用户名的格式是否有效，以及主机名是否不来自恶意域。 如果用户名的格式有效，而且主机名不来自恶意域，则该示例会返回 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 实例的只读集合，这些实例随后将用来提供声明以表示存储在用户名令牌中的信息。

    ```csharp
    protected override ReadOnlyCollection<IAuthorizationPolicy> ValidateUserNamePasswordCore(string userName, string password)
    {
        if (!ValidateUserNameFormat(userName))
            throw new SecurityTokenValidationException("Incorrect UserName format");

        ClaimSet claimSet = new DefaultClaimSet(ClaimSet.System, new Claim(ClaimTypes.Name, userName, Rights.PossessProperty));
        List<IIdentity> identities = new List<IIdentity>(1);
        identities.Add(new GenericIdentity(userName));
        List<IAuthorizationPolicy> policies = new List<IAuthorizationPolicy>(1);
        policies.Add(new UnconditionalPolicy(ClaimSet.System, claimSet, DateTime.MaxValue.ToUniversalTime(), identities));
        return policies.AsReadOnly();
    }
    ```

2. 提供由自定义令牌身份验证器返回的授权策略。

     此示例提供名为 <xref:System.IdentityModel.Policy.IAuthorizationPolicy> 的 `UnconditionalPolicy` 的自己的实现，在该示例的构造函数中，此策略返回一组传入该示例的声明和标识。

    ```csharp
    class UnconditionalPolicy : IAuthorizationPolicy
    {
        String id = Guid.NewGuid().ToString();
        ClaimSet issuer;
        ClaimSet issuance;
        DateTime expirationTime;
        IList<IIdentity> identities;

        public UnconditionalPolicy(ClaimSet issuer, ClaimSet issuance, DateTime expirationTime, IList<IIdentity> identities)
        {
            if (issuer == null)
                throw new ArgumentNullException("issuer");
            if (issuance == null)
                throw new ArgumentNullException("issuance");

            this.issuer = issuer;
            this.issuance = issuance;
            this.identities = identities;
            this.expirationTime = expirationTime;
        }

        public string Id
        {
            get { return this.id; }
        }

        public ClaimSet Issuer
        {
            get { return this.issuer; }
        }

        public DateTime ExpirationTime
        {
            get { return this.expirationTime; }
        }

        public bool Evaluate(EvaluationContext evaluationContext, ref object state)
        {
            evaluationContext.AddToTarget(this, this.issuance);

            if (this.identities != null)
            {
                object value;
                IList<IIdentity> contextIdentities;
                if (!evaluationContext.Properties.TryGetValue("Identities", out value))
                {
                    contextIdentities = new List<IIdentity>(this.identities.Count);
                    evaluationContext.Properties.Add("Identities", contextIdentities);
                }
                else
                {
                    contextIdentities = value as IList<IIdentity>;
                }
                foreach (IIdentity identity in this.identities)
                {
                    contextIdentities.Add(identity);
                }
            }

            evaluationContext.RecordExpirationTime(this.expirationTime);
            return true;
        }
    }
    ```

3. 编写自定义安全令牌管理器。

     使用 <xref:System.IdentityModel.Selectors.SecurityTokenManager>，可以为在 <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> 方法中传入该管理器的特定 <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> 对象创建 `CreateSecurityTokenAuthenticator`。 安全令牌管理器还用于创建令牌提供程序和令牌序列化程序，但是它们不包括在此示例中。 在此示例中，自定义安全令牌管理器继承自 <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> 类，并重写 `CreateSecurityTokenAuthenticator` 方法，这样，当所传递令牌的要求指示请求用户名身份验证器时，将返回自定义用户名令牌身份验证器。

    ```csharp
    public class MySecurityTokenManager : ServiceCredentialsSecurityTokenManager
    {
        MyUserNameCredential myUserNameCredential;

        public MySecurityTokenManager(MyUserNameCredential myUserNameCredential)
            : base(myUserNameCredential)
        {
            this.myUserNameCredential = myUserNameCredential;
        }

        public override SecurityTokenAuthenticator CreateSecurityTokenAuthenticator(SecurityTokenRequirement tokenRequirement, out SecurityTokenResolver outOfBandTokenResolver)
        {
            if (tokenRequirement.TokenType ==  SecurityTokenTypes.UserName)
            {
                outOfBandTokenResolver = null;
                return new MyTokenAuthenticator();
            }
            else
            {
                return base.CreateSecurityTokenAuthenticator(tokenRequirement, out outOfBandTokenResolver);
            }
        }
    }
    ```

4. 编写自定义服务凭据。

     使用服务凭据类，可以表示为服务配置的凭据，并创建一个用来获取令牌身份验证器、令牌提供程序和令牌序列化程序的安全令牌管理器。

    ```csharp
    public class MyUserNameCredential : ServiceCredentials
    {

        public MyUserNameCredential()
            : base()
        {
        }

        protected override ServiceCredentials CloneCore()
        {
            return new MyUserNameCredential();
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            return new MySecurityTokenManager(this);
        }

    }
    ```

5. 将服务配置为使用自定义服务凭据。

     为了让服务使用自定义服务凭据，我们在捕获已在默认服务凭据中预先配置的服务证书之后删除了默认的服务凭据类，将新的服务凭据实例配置为使用预先配置的服务证书，并将这个新服务凭据实例添加到服务行为中。

    ```csharp
    ServiceCredentials sc = serviceHost.Credentials;
    X509Certificate2 cert = sc.ServiceCertificate.Certificate;
    MyUserNameCredential serviceCredential = new MyUserNameCredential();
    serviceCredential.ServiceCertificate.Certificate = cert;
    serviceHost.Description.Behaviors.Remove((typeof(ServiceCredentials)));
    serviceHost.Description.Behaviors.Add(serviceCredential);
    ```

 若要显示调用方信息，可以使用 <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A>，如下面的代码中所示。 <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> 包含有关当前调用方的声明信息。

```csharp
static void DisplayIdentityInformation()
{
    Console.WriteLine("\t\tSecurity context identity  :  {0}",
            ServiceSecurityContext.Current.PrimaryIdentity.Name);
     return;
}
```

 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

## <a name="setup-batch-file"></a>设置批处理文件

 通过运行此示例随附的 Setup.bat 批处理文件，可以用相关的证书将服务器配置为运行需要基于服务器证书的安全性的自承载应用程序。 必须修改此批处理文件，以便跨计算机或在非承载情况下工作。

 下面提供了批处理文件不同节的简要概述，以便可以修改批处理文件从而在相应的配置中运行。

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

- 将服务器证书安装到客户端的受信任证书存储区中。

     Setup.bat 批处理文件中的以下行将服务器证书复制到客户端的受信任的人的存储区中。 因为客户端系统不隐式信任 Makecert.exe 生成的证书，所以需要执行此步骤。 如果您已经拥有一个证书，该证书来源于客户端的受信任根证书（例如由 Microsoft 颁发的证书），则不需要执行使用服务器证书填充客户端证书存储区这一步骤。

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

    > [!NOTE]
    > Setup 批处理文件通过 Windows SDK 命令提示运行。 这要求 MSSDK 环境变量指向 SDK 的安装目录。 将在 Windows SDK 命令提示中自动设置此环境变量。

#### <a name="to-set-up-and-build-the-sample"></a>设置和生成示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

#### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例

1. 在使用管理员特权打开的 Visual Studio 2012 命令提示符下，从示例安装文件夹中运行 Setup.bat。 这将安装运行示例所需的所有证书。

    > [!NOTE]
    > Setup.bat 批处理文件设计为在 Visual Studio 2012 命令提示符下运行。 在 Visual Studio 2012 命令提示符下设置的 PATH 环境变量指向包含 Setup.bat 脚本所需的可执行文件的目录。  
  
2. 启动 service\bin 中的 service.exe。  
  
3. 启动 \client\bin 中的 client.exe。 客户端活动将显示在客户端控制台应用程序上。  
  
4. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在服务计算机上为服务二进制文件创建一个目录。  
  
2. 将服务程序文件复制到服务计算机上的服务目录。 另外，将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。  
  
3. 必须具有一个其主题名称中包含计算机的完全限定域名的服务器证书。 必须更新服务的 App.config 文件才能反映这个新证书名称。 如果您将 `%SERVER_NAME%` 变量设置为将在其上运行服务的计算机的完全限定主机名，则可以使用 Setup.bat 来创建一个这样的证书。 请注意，必须使用管理员权限打开 Visual Studio 开发人员命令提示的 setup.bat 文件。  
  
4. 将服务器证书复制到客户端的 CurrentUser-TrustedPeople 存储中。 除非服务器证书是由客户端的受信任颁发者颁发的，否则没有必要这样做。  
  
5. 在服务计算机的 App.config 文件中，更改基址的值以指定一个完全限定的计算机名称，而不是 localhost。  
  
6. 在服务计算机上，在命令提示符下运行 service.exe。  
  
7. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的客户端程序文件复制到客户端计算机上。  
  
8. 在客户端计算机上的 Client.exe.config 文件中，更改终结点的地址值，使其与服务的新地址相匹配。  
  
9. 在客户端计算机上，在命令提示符下启动 Client.exe。  
  
10. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
1. 运行完示例后运行示例文件夹中的 Cleanup.bat。
