---
title: 自定义令牌
ms.date: 03/30/2017
ms.assetid: e7fd8b38-c370-454f-ba3e-19759019f03d
ms.openlocfilehash: 1a8c312248b0c15bb2e366a3d9925014556b6dd8
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553156"
---
# <a name="custom-token"></a>自定义令牌

此示例演示如何将自定义令牌实现添加到 Windows Communication Foundation (WCF) 应用程序。 示例使用 `CreditCardToken` 将客户端的信用卡相关信息安全地传递到服务。 令牌在 WS-Security 消息头中传递，并连同消息正文和其他消息头一起使用对称安全绑定元素进行签名和加密。 当内置令牌不足时可以进行这样的操作。 本示例演示如何向服务提供自定义安全令牌而不必使用某个内置令牌。 该服务实现定义“请求-答复”通信模式的协定。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

 总之，此示例将演示如下内容：

- 客户端如何向服务传递自定义安全令牌。

- 服务如何使用和验证自定义安全令牌。

- WCF 服务代码如何获取有关接收到的安全令牌（包括自定义安全令牌）的信息。

- 如何使用服务器的 X.509 证书保护用于消息加密和签名的对称密钥。

## <a name="client-authentication-using-a-custom-security-token"></a>使用自定义安全令牌的客户端身份验证

 服务公开单个终结点，此终结点是使用 `BindingHelper` 和 `EchoServiceHost` 类以编程方式创建的。 终结点由地址、绑定和协定组成。 此绑定使用 `SymmetricSecurityBindingElement` 和 `HttpTransportBindingElement` 按照自定义绑定进行配置。 本示例将 `SymmetricSecurityBindingElement` 设置为使用服务的 X.509 证书在传输过程中保护对称密钥和在 WS-Security 消息头中传递自定义 `CreditCardToken` 作为签名和加密的安全令牌。 此行为指定用于客户端身份验证的服务凭据和有关服务 X.509 证书的信息。

```csharp
public static class BindingHelper
{
    public static Binding CreateCreditCardBinding()
    {
        var httpTransport = new HttpTransportBindingElement();

        // The message security binding element will be configured to require a credit card.
        // The token that is encrypted with the service's certificate.
        var messageSecurity = new SymmetricSecurityBindingElement();
        messageSecurity.EndpointSupportingTokenParameters.SignedEncrypted.Add(new CreditCardTokenParameters());
        X509SecurityTokenParameters x509ProtectionParameters = new X509SecurityTokenParameters();
        x509ProtectionParameters.InclusionMode = SecurityTokenInclusionMode.Never;
        messageSecurity.ProtectionTokenParameters = x509ProtectionParameters;
        return new CustomBinding(messageSecurity, httpTransport);
    }
}
```

 为了使用消息中的信用卡令牌，此示例使用自定义服务凭据来提供此功能。 服务凭据类位于 `CreditCardServiceCredentials` 类中，并使用 `EchoServiceHost.InitializeRuntime` 方法添加到服务主机的行为集合中。

```csharp
class EchoServiceHost : ServiceHost
{
    string creditCardFile;

    public EchoServiceHost(parameters Uri[] addresses)
        : base(typeof(EchoService), addresses)
    {
        creditCardFile = ConfigurationManager.AppSettings["creditCardFile"];
        if (string.IsNullOrEmpty(creditCardFile))
        {
            throw new ConfigurationErrorsException("creditCardFile not specified in service config");
        }

        creditCardFile = String.Format("{0}\\{1}", System.Web.Hosting.HostingEnvironment.ApplicationPhysicalPath, creditCardFile);
    }

    override protected void InitializeRuntime()
    {
        // Create a credit card service credentials and add it to the behaviors.
        CreditCardServiceCredentials serviceCredentials = new CreditCardServiceCredentials(this.creditCardFile);
        serviceCredentials.ServiceCertificate.SetCertificate("CN=localhost", StoreLocation.LocalMachine, StoreName.My);
        this.Description.Behaviors.Remove((typeof(ServiceCredentials)));
        this.Description.Behaviors.Add(serviceCredentials);

        // Register a credit card binding for the endpoint.
        Binding creditCardBinding = BindingHelper.CreateCreditCardBinding();
        this.AddServiceEndpoint(typeof(IEchoService), creditCardBinding, string.Empty);

        base.InitializeRuntime();
    }
}
```

 客户端终结点的配置方式与服务终结点类似。 客户端使用相同的 `BindingHelper` 类创建绑定。 设置的其余部分位于 `Client` 类中。 客户端还通过向客户端终结点行为集合添加具有适当数据的 `CreditCardToken` 实例来设置要包含在 `CreditCardClientCredentials` 中的信息和有关在设置代码中的服务 X.509 证书的信息。 此实例使用将主题名称设置为 `CN=localhost` 的 X.509 证书作为服务证书。

```csharp
Binding creditCardBinding = BindingHelper.CreateCreditCardBinding();
var serviceAddress = new EndpointAddress("http://localhost/servicemodelsamples/service.svc");

// Create a client with given client endpoint configuration.
channelFactory = new ChannelFactory<IEchoService>(creditCardBinding, serviceAddress);

// Configure the credit card credentials on the channel factory.
var credentials =
      new CreditCardClientCredentials(
      new CreditCardInfo(creditCardNumber, issuer, expirationTime));
// Configure the service certificate on the credentials.
credentials.ServiceCertificate.SetDefaultCertificate(
      "CN=localhost", StoreLocation.LocalMachine, StoreName.My);

// Replace ClientCredentials with CreditCardClientCredentials.
channelFactory.Endpoint.Behaviors.Remove(typeof(ClientCredentials));
channelFactory.Endpoint.Behaviors.Add(credentials);

client = channelFactory.CreateChannel();

Console.WriteLine($"Echo service returned: {client.Echo()}");

((IChannel)client).Close();
channelFactory.Close();
```

## <a name="custom-security-token-implementation"></a>自定义安全令牌实现

 若要在 WCF 中启用自定义安全令牌，请创建自定义安全令牌的对象表示形式。 此示例的 `CreditCardToken` 类中有此表示形式。 对象表示形式负责保存所有相关的安全令牌信息并负责提供包含在安全令牌中的安全密钥列表。 在本例中，信用卡安全令牌不包含任何安全密钥。

 下一部分介绍了必须执行哪些操作才能使自定义令牌通过网络传输并由 WCF 终结点使用。

```csharp
class CreditCardToken : SecurityToken
{
    CreditCardInfo cardInfo;
    DateTime effectiveTime = DateTime.UtcNow;
    string id;
    ReadOnlyCollection<SecurityKey> securityKeys;

    public CreditCardToken(CreditCardInfo cardInfo) : this(cardInfo, Guid.NewGuid().ToString()) { }

    public CreditCardToken(CreditCardInfo cardInfo, string id)
    {
        if (cardInfo == null)
            throw new ArgumentNullException(nameof(cardInfo));

        if (id == null)
            throw new ArgumentNullException(nameof(id));

        this.cardInfo = cardInfo;
        this.id = id;

        // The credit card token is not capable of any cryptography.
        this.securityKeys = new ReadOnlyCollection<SecurityKey>(new List<SecurityKey>());
    }

    public CreditCardInfo CardInfo { get { return this.cardInfo; } }

    public override ReadOnlyCollection<SecurityKey> SecurityKeys { get { return this.securityKeys; } }

    public override DateTime ValidFrom { get { return this.effectiveTime; } }
    public override DateTime ValidTo { get { return this.cardInfo.ExpirationDate; } }
    public override string Id { get { return this.id; } }
}
```

## <a name="getting-the-custom-credit-card-token-to-and-from-the-message"></a>向消息中写入和从消息中获取自定义信用卡令牌

 WCF 中的安全令牌序列化程序负责根据消息中的 XML 创建安全令牌的对象表示形式，并创建安全令牌的 XML 格式。 安全令牌序列化程序还负责其他功能，如读取和写入指向安全令牌的密钥标识符，但本示例只使用与安全令牌相关的功能。 要启用自定义令牌，您必须实现您自己的安全令牌序列化程序。 本示例使用 `CreditCardSecurityTokenSerializer` 类来实现此目的。

 在服务上，自定义序列化程序读取 XML 形式的自定义令牌并据此创建自定义令牌的对象表示形式。

 在客户端上，`CreditCardSecurityTokenSerializer` 类将包含在安全令牌对象表示形式中的信息写入到 XML 编写器中。

```csharp
public class CreditCardSecurityTokenSerializer : WSSecurityTokenSerializer
{
    public CreditCardSecurityTokenSerializer(SecurityTokenVersion version) : base() { }

    protected override bool CanReadTokenCore(XmlReader reader)
    {
        XmlDictionaryReader localReader = XmlDictionaryReader.CreateDictionaryReader(reader);

        if (reader == null)
            throw new ArgumentNullException(nameof(reader));

        if (reader.IsStartElement(Constants.CreditCardTokenName, Constants.CreditCardTokenNamespace))
            return true;

        return base.CanReadTokenCore(reader);
    }

    protected override SecurityToken ReadTokenCore(XmlReader reader, SecurityTokenResolver tokenResolver)
    {
        if (reader == null)
            throw new ArgumentNullException(nameof(reader));

        if (reader.IsStartElement(Constants.CreditCardTokenName, Constants.CreditCardTokenNamespace))
        {
            string id = reader.GetAttribute(Constants.Id, Constants.WsUtilityNamespace);

            reader.ReadStartElement();

            // Read the credit card number.
            string creditCardNumber = reader.ReadElementString(Constants.CreditCardNumberElementName, Constants.CreditCardTokenNamespace);

            // Read the expiration date.
            string expirationTimeString = reader.ReadElementString(Constants.CreditCardExpirationElementName, Constants.CreditCardTokenNamespace);
            DateTime expirationTime = XmlConvert.ToDateTime(expirationTimeString, XmlDateTimeSerializationMode.Utc);

            // Read the issuer of the credit card.
            string creditCardIssuer = reader.ReadElementString(Constants.CreditCardIssuerElementName, Constants.CreditCardTokenNamespace);
            reader.ReadEndElement();

            var cardInfo = new CreditCardInfo(creditCardNumber, creditCardIssuer, expirationTime);

            return new CreditCardToken(cardInfo, id);
        }
        else
        {
            return WSSecurityTokenSerializer.DefaultInstance.ReadToken(reader, tokenResolver);
        }
    }

    protected override bool CanWriteTokenCore(SecurityToken token)
    {
        if (token is CreditCardToken)
            return true;
        return base.CanWriteTokenCore(token);
    }

    protected override void WriteTokenCore(XmlWriter writer, SecurityToken token)
    {
        if (writer == null)
            throw new ArgumentNullException(nameof(writer));
        if (token == null)
            throw new ArgumentNullException(nameof(token));

        CreditCardToken c = token as CreditCardToken;
        if (c != null)
        {
            writer.WriteStartElement(Constants.CreditCardTokenPrefix, Constants.CreditCardTokenName, Constants.CreditCardTokenNamespace);
            writer.WriteAttributeString(Constants.WsUtilityPrefix, Constants.Id, Constants.WsUtilityNamespace, token.Id);
            writer.WriteElementString(Constants.CreditCardNumberElementName, Constants.CreditCardTokenNamespace, c.CardInfo.CardNumber);
            writer.WriteElementString(Constants.CreditCardExpirationElementName, Constants.CreditCardTokenNamespace, XmlConvert.ToString(c.CardInfo.ExpirationDate, XmlDateTimeSerializationMode.Utc));
            writer.WriteElementString(Constants.CreditCardIssuerElementName, Constants.CreditCardTokenNamespace, c.CardInfo.CardIssuer);
            writer.WriteEndElement();
            writer.Flush();
        }
        else
        {
            base.WriteTokenCore(writer, token);
        }
    }
}
```

## <a name="how-token-provider-and-token-authenticator-classes-are-created"></a>如何创建令牌提供程序和令牌身份验证器类

 客户端和服务凭据负责提供安全令牌管理器实例。 安全令牌管理器实例用于获取令牌提供程序、令牌身份验证器和令牌序列化程序。

 令牌提供程序会基于包含在客户端或服务凭据中的信息创建令牌的对象表示形式。 然后使用令牌序列化程序（在上一节中讨论）将令牌对象表示形式写入消息。

 令牌身份验证器验证在消息中到达的令牌。 通过令牌序列化程序创建传入令牌的对象表示形式。 然后将此对象表示形式传递给身份验证器进行验证。 在成功验证令牌后，令牌身份验证器会返回 `IAuthorizationPolicy` 对象的集合，这些对象表示包含在令牌中的信息。 在稍后处理消息的过程中将使用此信息来执行身份验证决定并为应用程序提供声明。 在本示例中，信用卡令牌身份验证器使用 `CreditCardTokenAuthorizationPolicy` 来实现此目的。

 令牌序列化程序负责通过网络获取令牌的对象表示形式。 此过程在上一节中进行讨论。

 在本示例中，由于只想在客户端到服务方向传输信用卡令牌，因此只在客户端上使用令牌提供程序并只在服务上使用令牌身份验证器。

 在客户端上，此功能位于 `CreditCardClientCredentials`、`CreditCardClientCredentialsSecurityTokenManager` 和 `CreditCardTokenProvider` 类中。

 在服务上，此功能位于 `CreditCardServiceCredentials`、`CreditCardServiceCredentialsSecurityTokenManager`, `CreditCardTokenAuthenticator` 和 `CreditCardTokenAuthorizationPolicy` 类中。

```csharp
    public class CreditCardClientCredentials : ClientCredentials
    {
        CreditCardInfo creditCardInfo;

        public CreditCardClientCredentials(CreditCardInfo creditCardInfo)
            : base()
        {
            if (creditCardInfo == null)
                throw new ArgumentNullException(nameof(creditCardInfo));

            this.creditCardInfo = creditCardInfo;
        }

        public CreditCardInfo CreditCardInfo
        {
            get { return this.creditCardInfo; }
        }

        protected override ClientCredentials CloneCore()
        {
            return new CreditCardClientCredentials(this.creditCardInfo);
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            return new CreditCardClientCredentialsSecurityTokenManager(this);
        }
    }

    public class CreditCardClientCredentialsSecurityTokenManager : ClientCredentialsSecurityTokenManager
    {
        CreditCardClientCredentials creditCardClientCredentials;

        public CreditCardClientCredentialsSecurityTokenManager(CreditCardClientCredentials creditCardClientCredentials)
            : base (creditCardClientCredentials)
        {
            this.creditCardClientCredentials = creditCardClientCredentials;
        }

        public override SecurityTokenProvider CreateSecurityTokenProvider(SecurityTokenRequirement tokenRequirement)
        {
            // Handle this token for Custom.
            if (tokenRequirement.TokenType == Constants.CreditCardTokenType)
                return new CreditCardTokenProvider(this.creditCardClientCredentials.CreditCardInfo);
            // Return server cert.
            else if (tokenRequirement is InitiatorServiceModelSecurityTokenRequirement)
            {
                if (tokenRequirement.TokenType == SecurityTokenTypes.X509Certificate)
                {
                    return new X509SecurityTokenProvider(creditCardClientCredentials.ServiceCertificate.DefaultCertificate);
                }
            }

            return base.CreateSecurityTokenProvider(tokenRequirement);
        }

        public override SecurityTokenSerializer CreateSecurityTokenSerializer(SecurityTokenVersion version)
        {

            return new CreditCardSecurityTokenSerializer(version);
        }

    }

    class CreditCardTokenProvider : SecurityTokenProvider
    {
        CreditCardInfo creditCardInfo;

        public CreditCardTokenProvider(CreditCardInfo creditCardInfo) : base()
        {
            if (creditCardInfo == null)
                throw new ArgumentNullException(nameof(creditCardInfo));

            this.creditCardInfo = creditCardInfo;
        }

        protected override SecurityToken GetTokenCore(TimeSpan timeout)
        {
            SecurityToken result = new CreditCardToken(this.creditCardInfo);
            return result;
        }
    }

    public class CreditCardServiceCredentials : ServiceCredentials
    {
        string creditCardFile;

        public CreditCardServiceCredentials(string creditCardFile)
            : base()
        {
            if (creditCardFile == null)
                throw new ArgumentNullException(nameof(creditCardFile));

            this.creditCardFile = creditCardFile;
        }

        public string CreditCardDataFile
        {
            get { return this.creditCardFile; }
        }

        protected override ServiceCredentials CloneCore()
        {
            return new CreditCardServiceCredentials(this.creditCardFile);
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            return new CreditCardServiceCredentialsSecurityTokenManager(this);
        }
    }

    public class CreditCardServiceCredentialsSecurityTokenManager : ServiceCredentialsSecurityTokenManager
    {
        CreditCardServiceCredentials creditCardServiceCredentials;

        public CreditCardServiceCredentialsSecurityTokenManager(CreditCardServiceCredentials creditCardServiceCredentials)
            : base(creditCardServiceCredentials)
        {
            this.creditCardServiceCredentials = creditCardServiceCredentials;
        }

        public override SecurityTokenAuthenticator CreateSecurityTokenAuthenticator(SecurityTokenRequirement tokenRequirement, out SecurityTokenResolver outOfBandTokenResolver)
        {
            if (tokenRequirement.TokenType == Constants.CreditCardTokenType)
            {
                outOfBandTokenResolver = null;
                return new CreditCardTokenAuthenticator(creditCardServiceCredentials.CreditCardDataFile);
            }

            return base.CreateSecurityTokenAuthenticator(tokenRequirement, out outOfBandTokenResolver);
        }

        public override SecurityTokenSerializer CreateSecurityTokenSerializer(SecurityTokenVersion version)
        {
            return new CreditCardSecurityTokenSerializer(version);
        }
    }

    class CreditCardTokenAuthenticator : SecurityTokenAuthenticator
    {
        string creditCardsFile;
        public CreditCardTokenAuthenticator(string creditCardsFile)
        {
            this.creditCardsFile = creditCardsFile;
        }

        protected override bool CanValidateTokenCore(SecurityToken token)
        {
            return (token is CreditCardToken);
        }

        protected override ReadOnlyCollection<IAuthorizationPolicy> ValidateTokenCore(SecurityToken token)
        {
            CreditCardToken creditCardToken = token as CreditCardToken;

            if (creditCardToken.CardInfo.ExpirationDate < DateTime.UtcNow)
                throw new SecurityTokenValidationException("The credit card has expired.");
            if (!IsCardNumberAndExpirationValid(creditCardToken.CardInfo))
                throw new SecurityTokenValidationException("Unknown or invalid credit card.");

            // the credit card token has only 1 claim - the card number. The issuer for the claim is the
            // credit card issuer

            var cardIssuerClaimSet = new DefaultClaimSet(new Claim(ClaimTypes.Name, creditCardToken.CardInfo.CardIssuer, Rights.PossessProperty));
            var cardClaimSet = new DefaultClaimSet(cardIssuerClaimSet, new Claim(Constants.CreditCardNumberClaim, creditCardToken.CardInfo.CardNumber, Rights.PossessProperty));
            var policies = new List<IAuthorizationPolicy>(1);
            policies.Add(new CreditCardTokenAuthorizationPolicy(cardClaimSet));
            return policies.AsReadOnly();
        }

        /// <summary>
        /// Helper method to check if a given credit card entry is present in the User DB
        /// </summary>
        private bool IsCardNumberAndExpirationValid(CreditCardInfo cardInfo)
        {
            try
            {
                using (var myStreamReader = new StreamReader(this.creditCardsFile))
                {
                    string line = "";
                    while ((line = myStreamReader.ReadLine()) != null)
                    {
                        string[] splitEntry = line.Split('#');
                        if (splitEntry[0] == cardInfo.CardNumber)
                        {
                            string expirationDateString = splitEntry[1].Trim();
                            DateTime expirationDateOnFile = DateTime.Parse(expirationDateString, System.Globalization.DateTimeFormatInfo.InvariantInfo, System.Globalization.DateTimeStyles.AdjustToUniversal);
                            if (cardInfo.ExpirationDate == expirationDateOnFile)
                            {
                                string issuer = splitEntry[2];
                                return issuer.Equals(cardInfo.CardIssuer, StringComparison.InvariantCultureIgnoreCase);
                            }
                            else
                            {
                                return false;
                            }
                        }
                    }
                    return false;
                }
            }
            catch (Exception e)
            {
                throw new Exception("BookStoreService: Error while retrieving credit card information from User DB " + e.ToString());
            }
        }
    }

    public class CreditCardTokenAuthorizationPolicy : IAuthorizationPolicy
    {
        string id;
        ClaimSet issuer;
        IEnumerable<ClaimSet> issuedClaimSets;

        public CreditCardTokenAuthorizationPolicy(ClaimSet issuedClaims)
        {
            if (issuedClaims == null)
                throw new ArgumentNullException(nameof(issuedClaims));
            this.issuer = issuedClaims.Issuer;
            this.issuedClaimSets = new ClaimSet[] { issuedClaims };
            this.id = Guid.NewGuid().ToString();
        }

        public ClaimSet Issuer { get { return this.issuer; } }

        public string Id { get { return this.id; } }

        public bool Evaluate(EvaluationContext context, ref object state)
        {
            foreach (ClaimSet issuance in this.issuedClaimSets)
            {
                context.AddClaimSet(this, issuance);
            }

            return true;
        }
    }
```

## <a name="displaying-the-callers-information"></a>显示调用方信息

 若要显示调用方信息，请使用 `ServiceSecurityContext.Current.AuthorizationContext.ClaimSets`，如下面的示例代码所示。 `ServiceSecurityContext.Current.AuthorizationContext.ClaimSets` 包含与当前调用方关联的授权声明。 声明由 `CreditCardToken` 类在其 `AuthorizationPolicies` 集合中提供。

```csharp
bool TryGetStringClaimValue(ClaimSet claimSet, string claimType, out string claimValue)
{
    claimValue = null;
    IEnumerable<Claim> matchingClaims = claimSet.FindClaims(claimType, Rights.PossessProperty);
    if (matchingClaims == null)
        return false;
    IEnumerator<Claim> enumerator = matchingClaims.GetEnumerator();
    enumerator.MoveNext();
    claimValue = (enumerator.Current.Resource == null) ? null :
        enumerator.Current.Resource.ToString();
    return true;
}

string GetCallerCreditCardNumber()
{
     foreach (ClaimSet claimSet in
         ServiceSecurityContext.Current.AuthorizationContext.ClaimSets)
     {
         string creditCardNumber = null;
         if (TryGetStringClaimValue(claimSet,
             Constants.CreditCardNumberClaim, out creditCardNumber))
             {
                 string issuer;
                 if (!TryGetStringClaimValue(claimSet.Issuer,
                        ClaimTypes.Name, out issuer))
                 {
                     issuer = "Unknown";
                 }
                 return $"Credit card '{creditCardNumber}' issued by '{issuer}'";
        }
    }
    return "Credit card is not known";
}
```

 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

## <a name="setup-batch-file"></a>设置批处理文件

 通过运行此示例随附的 Setup.bat 批处理文件，可以用相关的证书将服务器配置为运行需要基于服务器证书的安全性的 IIS 承载的应用程序。 必须修改此批处理文件，以便跨计算机或在非承载情况下工作。

 下面提供了批处理文件不同节的简要概述，以便可以修改批处理文件从而在相应的配置中运行。

- 创建服务器证书：

     `Setup.bat` 批处理文件中的以下行创建将要使用的服务器证书。 `%SERVER_NAME%`变量指定服务器名称。 更改此变量可以指定您自己的服务器名称。 此批处理文件中的默认值为 localhost。 如果更改 `%SERVER_NAME%` 变量，则必须浏览 Client.cs 和 Service.cs 文件并用 Setup.bat 脚本中使用的服务器名来替换 localhost 的所有实例。

     证书存储在 `LocalMachine` 存储位置下的 My（个人）存储区中。 对于 IIS 承载的服务，证书存储在 LocalMachine 存储区中。 对于自承载服务，应该通过用 CurrentUser 替换字符串 LocalMachine 来修改批处理文件，以便将客户端证书存储在 CurrentUser 存储位置。

    ```bat
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

    ```bat
    echo ************
    echo copying server cert to client's TrustedPeople store
    echo ************
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

- 若要从 IIS 承载的服务启用对证书私钥的访问，必须为 IIS 承载的进程运行时所使用的用户帐户授予对该私钥的适当权限。 这将由 Setup.bat 脚本中的最后步骤来完成。

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
> Setup.bat 批处理文件设计为在 Visual Studio 2012 命令提示符下运行。 在 Visual Studio 2012 命令提示符下设置的 PATH 环境变量指向包含 Setup.bat 脚本所需的可执行文件的目录。

#### <a name="to-set-up-and-build-the-sample"></a>设置和生成示例

1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

#### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例

1. 使用管理员权限打开 Visual Studio 2012 命令提示符窗口，然后从示例安装文件夹中运行 Setup.bat。 这将安装运行示例所需的所有证书。请确保路径包括 Makecert.exe 所在的文件夹。

> [!NOTE]
> 确保在运行完该示例后运行 Cleanup.bat 移除证书。 其他安全示例使用相同的证书。  
  
1. 从 client\bin 目录启动 Client.exe。 客户端活动将显示在客户端控制台应用程序上。  
  
2. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-run-the-sample-across-computer"></a>跨计算机运行示例  
  
1. 在服务计算机上为服务二进制文件创建一个目录。  
  
2. 将服务程序文件复制到服务计算机上的服务目录。 不要忘记复制 CreditCardFile.txt，否则信用卡身份验证器将不能验证从客户端发送的信用卡信息。 另外，将 Setup.bat 和 Cleanup.bat 文件复制到服务计算机上。  
  
3. 必须具有一个其主题名称中包含计算机的完全限定域名的服务器证书。 如果您将 `%SERVER_NAME%` 变量更改为承载服务的计算机的完全限定的名称，您可以使用 Setup.bat 来创建一个这样的证书。 请注意，Setup.bat 文件必须在以管理员权限打开的 Visual Studio 开发人员命令提示中运行。  
  
4. 将服务器证书复制到客户端上的 CurrentUser-TrustedPeople 存储区中。 只有当服务器证书不是由受信任的颁发者颁发的情况下才需要执行此操作。  
  
5. 在 EchoServiceHost.cs 文件中，更改证书主题名称的值以指定一个完全限定的计算机名，而不是 localhost。  
  
6. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的客户端程序文件复制到客户端计算机上。  
  
7. 在 Client.cs 文件中，更改终结点的地址值以与服务的新地址相匹配。  
  
8. 在 Client.cs 文件中，更改服务 X.509 证书的主题名称以与远程主机的完全限定的计算机名（而不是 localhost）相匹配。  
  
9. 在客户端计算机上，从命令提示窗口中启动 Client.exe。  
  
10. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
1. 运行完示例后运行示例文件夹中的 Cleanup.bat。
