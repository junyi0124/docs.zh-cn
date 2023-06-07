---
title: .NET Framework 中的新增功能
description: 了解不同版本 .NET Framework 中的新变化。 阅读每个版本的主要新功能和改进的摘要。
ms.date: 10/21/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- what's new [.NET Framework]
ms.assetid: 1d971dd7-10fc-4692-8dac-30ca308fc0fa
ms.openlocfilehash: 3421afee304125413f4fcade6b20df990e922f58
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95704794"
---
# <a name="whats-new-in-net-framework"></a>.NET Framework 中的新增功能

[!INCLUDE [net-framework-future](../../../includes/net-framework-future.md)]

本文总结了以下版本的 .NET Framework 中的主要新功能和改进：

- [.NET Framework 4.8](#v48)
- [.NET Framework 4.7.2](#v472)
- [.NET Framework 4.7.1](#v471)
- [.NET Framework 4.7](#v47)
- [.NET Framework 4.6.2](#v462)
- [.NET Framework 4.6.1](#v461)
- [.NET 2015 和 .NET Framework 4.6](#v46)
- [.NET Framework 4.5.2](#v452)
- [.NET Framework 4.5.1](#v451)
- [.NET Framework 4.5](#v45)

本文不提供有关每项新增功能的完整信息，并有可能会发生更改。 有关 .NET Framework 的常规信息，请参阅[入门](../get-started/index.md)。 有关支持的平台，请参阅[系统要求](../get-started/system-requirements.md)。 有关下载链接和安装说明，请参阅[安装指南](../install/guide-for-developers.md)。

> [!NOTE]
> .NET Framework 团队还发布 NuGet 带外功能以扩展平台支持并引入新功能，如不可变集合和启用了 SIMD 的矢量类型。 有关详细信息，请参阅[其他类库和 API](../additional-apis/index.md) 以及 [.NET Framework 和带外版本](../get-started/the-net-framework-and-out-of-band-releases.md)。
> 请参阅用于 .NET Framework 的 [完整 NuGet 包列表](https://www.nuget.org/profiles/dotnetframework)。

<a name="v48"></a>

## <a name="introducing-net-framework-48"></a>.NET Framework 4.8 简介

.NET Framework 4.8 在 .NET Framework 4.x 早期版本的基础之上构建而成，新增了许多修补程序和功能，同时很好地保持了产品的稳定性。

### <a name="download-and-install-net-framework-48"></a>下载和安装 .NET Framework 4.8

可以从下列位置下载 .NET Framework 4.8：

- [.NET Framework 4.8 Web 安装程序](https://dotnet.microsoft.com/download/dotnet-framework/net48)

- [NET Framework 4.8 脱机安装程序](https://dotnet.microsoft.com/download/dotnet-framework/net48)

可以在 Windows 10、Windows 8.1、Windows 7 SP1 和对应的服务器平台（版本不低于 Windows Server 2008 R2 SP1）上安装 .NET Framework 4.8。 可以使用 Web 安装程序或脱机安装程序来安装 .NET Framework 4.8。 适用于大多数用户的建议方法是使用 Web 安装程序。

可以通过安装 [.NET Framework 4.8 开发人员工具包](https://go.microsoft.com/fwlink/?LinkId=2085167)，在 Visual Studio 2012 或更高版本中定位 .NET Framework 4.8。

### <a name="whats-new-in-net-framework-48"></a>.NET Framework 4.8 中的新增功能

.NET Framework 4.8 在以下几个领域引入了新功能：

- [基类](#core48)
- [Windows Communication Foundation (WCF)](#wcf48)
- [Windows Presentation Foundation (WPF)](#wpf48)
- [公共语言运行时](#clr48)

改进了辅助功能，使应用程序能为辅助技术的用户提供最佳体验，这仍是 .NET Framework 4.8 的重点。 有关 .NET Framework 4.8 中辅助功能改进的信息，请参阅 [.NET Framework 中辅助功能的新增功能](whats-new-in-accessibility.md)。

<a name="core48"></a>

#### <a name="base-classes"></a>基类

**减少 FIPS 对加密的影响**。 在 .NET framework 的早期版本中，当在“FIPS 模式”下配置系统加密库时，<xref:System.Security.Cryptography.SHA256Managed> 等托管加密提供程序类会引发 <xref:System.Security.Cryptography.CryptographicException>。 引发这些异常的原因是加密提供程序类的托管版本尚未进行 FIPS（联邦信息处理标准）140-2 认证，这与系统加密库不同。 由于几个开发人员使其开发计算机处于 FIPS 模式，因此通常会在生产系统中引发异常。

默认情况下，在面向 .NET Framework 4.8 的应用程序中，以下托管加密类在这种情况下不再引发 <xref:System.Security.Cryptography.CryptographicException>：

- <xref:System.Security.Cryptography.MD5Cng>
- <xref:System.Security.Cryptography.MD5CryptoServiceProvider>
- <xref:System.Security.Cryptography.RC2CryptoServiceProvider>
- <xref:System.Security.Cryptography.RijndaelManaged>
- <xref:System.Security.Cryptography.RIPEMD160Managed>
- <xref:System.Security.Cryptography.SHA256Managed>

相反，这些类会将加密操作重定向到系统加密库。 此更改可有效地删除开发人员环境和生产环境之间可能令人混淆的差异，并使本机组件和托管组件采用相同的加密策略运行。 依赖于这些异常的应用程序可以通过将 AppContext 开关 `Switch.System.Security.Cryptography.UseLegacyFipsThrow` 设置为 `true` 来还原以前的行为。 有关详细信息，请参阅[托管加密类不会在 FIPS 模式下引发 CryptographyException](../migration-guide/retargeting/4.7.2-4.8.md#managed-cryptography-classes-do-not-throw-a-cryptographyexception-in-fips-mode)。

**使用 ZLib 的更新版本**

从 .NET Framework 4.5 开始，clrcompression.dll 程序集使用 [ZLib](https://www.zlib.net)（即，数据压缩的本机外部库），以便提供 deflate 算法实现。 在 .NET Framework 4.8 版本中，clrcompression.dll 更新为使用 ZLib 版本 1.2.11，其中包括几个主要的改进和修补程序。

<a name="wcf48"></a>

#### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

**ServiceHealthBehavior 简介**

运行状况终结点由业务流程工具广泛使用以基于其运行状况状态来管理服务。 运行状况检查还可由监视工具使用以跟踪并提供有关服务的可用性和性能的通知。

ServiceHealthBehavior 是一个 WCF 服务行为，该行为可扩展 <xref:System.ServiceModel.Description.IServiceBehavior>。  添加到 <xref:System.ServiceModel.Description.ServiceDescription.Behaviors?displayProperty=nameWithType> 集合后，服务行为会执行以下操作：

- 返回带有 HTTP 响应代码的服务运行状况状态。 可以在查询字符串中指定 HTTP/GET 运行状况探测请求的 HTTP 状态代码。

- 发布有关服务运行状况的信息。 服务特定的详细信息，包括可以使用带有 `?health` 查询字符串的 HTTP/GET 请求显示的服务状态、限制计数和容量。 对行为不正常的 WCF 服务进行故障排除时，可以轻松访问此类信息则很重要。

可通过两种方式公开运行状况终结点并发布 WCF 服务运行状况信息：

- 通过代码。 例如：

  ```csharp
  ServiceHost host = new ServiceHost(typeof(Service1),
                     new Uri("http://contoso:81/Service1"));
  ServiceHealthBehavior healthBehavior =
      host.Description.Behaviors.Find<ServiceHealthBehavior>();
  healthBehavior ??= new ServiceHealthBehavior();
  host.Description.Behaviors.Add(healthBehavior);
  ```

  ```vb
  Dim host As New ServiceHost(GetType(Service1),
              New Uri("http://contoso:81/Service1"))
  Dim healthBehavior As ServiceHealthBehavior =
     host.Description.Behaviors.Find(Of ServiceHealthBehavior)()
  If healthBehavior Is Nothing Then
     healthBehavior = New ServiceHealthBehavior()
  End If
  host.Description.Behaviors.Add(healthBehavior)
  ```

- 通过使用配置文件。 例如：

  ```xml
  <behaviors>
    <serviceBehaviors>
      <behavior name="DefaultBehavior">
        <serviceHealth httpsGetEnabled="true"/>
      </behavior>
    </serviceBehaviors>
  </behaviors>
  ```

可以使用查询参数（例如，`OnServiceFailure`、`OnDispatcherFailure`、`OnListenerFailure`、`OnThrottlePercentExceeded`）查询服务的运行状况状态，并且可以为每个查询参数指定 HTTP 响应代码。 如果省略了查询参数的 HTTP 响应代码，则默认使用 503 HTTP 响应代码。 例如：

- OnServiceFailure：`https://contoso:81/Service1?health&OnServiceFailure=450`

  当 [ServiceHost.State](xref:System.ServiceModel.Channels.CommunicationObject.State) 大于 <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType> 时，将返回 450 HTTP 响应状态代码。

查询参数和示例：

- OnDispatcherFailure：`https://contoso:81/Service1?health&OnDispatcherFailure=455`

  当任意通道调度程序的状态大于 <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType> 时，将返回 455 HTTP 响应状态代码。

- OnListenerFailure：`https://contoso:81/Service1?health&OnListenerFailure=465`

  当任意通道侦听器的状态大于 <xref:System.ServiceModel.CommunicationState.Opened?displayProperty=nameWithType> 时，将返回 465 HTTP 响应状态代码。

- OnThrottlePercentExceeded：`https://contoso:81/Service1?health&OnThrottlePercentExceeded= 70:350,95:500`

  指定触发响应及其 HTTP 响应代码 {200 – 599} 的百分比 {1 – 100}。 在此示例中：

  - 如果百分比大于 95，则返回 500 HTTP 响应代码。

  - 如果百分比介于 70 和 95 之间，则返回 350。

  - 否则将返回 200。

服务运行状况状态可以通过指定查询字符串（如 `https://contoso:81/Service1?health`）以 HTML 格式显示，或通过指定查询字符串（如 `https://contoso:81/Service1?health&Xml`）以 XML 格式显示。 查询字符串（如 `https://contoso:81/Service1?health&NoContent`）返回空 HTML 页。

<a name="wpf48"></a>

#### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**高 DPI 增强功能**

在 .NET Framework 4.8 中，WPF 添加了对按监视器 V2 DPI 感知和混合模式 DPI 缩放的支持。 有关高 DPI 开发的其他信息，请参阅[在 Windows 上开发高 DPI 桌面应用程序](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows)。

.NET framework 4.8 改进了对支持混合模式 DPI 缩放的平台上的高 DPI WPF 应用程序中的寄宿 HWND 和 Windows 窗体互操作的支持（从 Windows 10 2018 年 4 月更新开始）。 通过调用 [SetThreadDpiHostingBehavior](/windows/desktop/api/winuser/nf-winuser-setthreaddpihostingbehavior) 和 [SetThreadDpiAwarenessContext](/windows/desktop/api/winuser/nf-winuser-setthreaddpiawarenesscontext) 将寄宿 HWND 或 Windows 窗体控件创建为混合模式 DPI 缩放窗口时，它们可以托管在按监视器 V2 WPF 应用程序中，并且相应地调整大小和缩放。 此类托管内容不以本机 DPI 呈现；相反，操作系统将托管内容缩放到合适大小。 对按监视器 v2 DPI 感知模式的支持还允许 WPF 控件托管（即，设置为父级）在高 DPI 应用程序的本机窗口中。

若要启用对混合模式高 DPI 缩放的支持，可以设置以下 [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 切换应用程序配置文件：

```xml
<runtime>
   <AppContextSwitchOverrides value = "Switch.System.Windows.DoNotScaleForDpiChanges=false; Switch.System.Windows.DoNotUsePresentationDpiCapabilityTier2OrGreater=false"/>
</runtime>
```

<a name="clr48"></a>

#### <a name="common-language-runtime"></a>公共语言运行时

.NET Framework 4.8 中的运行时包含以下更改和改进：

**JIT 编译器的改进**。 .NET Framework 4.8 中的实时 (JIT) 编译器基于 .NET Core 2.1 中的 JIT 编译器。 对 .NET Core 2.1 JIT 编译器所做的多个优化和所有 bug 修复都包含在 .NET Framework 4.8 JIT 编译器中。

**NGEN 改进**。 运行时改进了[本机映像生成器](../tools/ngen-exe-native-image-generator.md) (NGEN) 映像的内存管理，以便从 NGEN 映像映射的数据不驻留在内存中。 这将缩减可受到攻击的外围应用，攻击方法为试图通过修改将执行的内存来执行任意代码。

**所有程序集的反恶意软件扫描**。 在 .NET Framework 的早期版本中，运行时使用 Windows Defender 或第三方反恶意软件扫描从磁盘加载的所有程序集。 但是，从其他源加载的程序集（例如，通过 <xref:System.Reflection.Assembly.Load(System.Byte[])?displayProperty=nameWithType> 方法）不会进行扫描，并且可能包含未检测到的恶意软件。 从 Windows 10 上运行的 .NET Framework 4.8 开始，运行时通过实现[反恶意软件扫描界面 (AMSI)](/windows/desktop/AMSI/antimalware-scan-interface-portal) 的反恶意软件解决方案来触发扫描。

<a name="v472"></a>

## <a name="whats-new-in-net-framework-472"></a>.NET Framework 4.7.2 中的新增功能

.NET Framework 4.7.2 在以下几个领域新增了功能：

- [基类](#core-472)
- [ASP.NET](#asp-net472)
- [网络连接](#net472)
- [SQL](#sql472)
- [WPF](#wpf472)
- [ClickOnce](#clickonce)

.NET Framework 4.7.2 持续关注的重点是辅助功能的改进，使应用程序能为使用辅助技术的用户提供最佳体验。 有关 .NET Framework 4.7.2 中辅助功能改进的信息，请参阅 [.NET Framework 中辅助功能的新增功能](whats-new-in-accessibility.md)。

<a name="core-472"></a>

#### <a name="base-classes"></a>基类

.NET Framework 4.7.2 提供大量的加密增强功能、对 ZIP 存档更好的解压缩支持以及额外的集合 API。

**RSA.Create 和 DSA.Create 的新重载**

利用 <xref:System.Security.Cryptography.DSA.Create(System.Security.Cryptography.DSAParameters)?displayProperty=nameWithType> 和 <xref:System.Security.Cryptography.RSA.Create(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType> 方法，可以在实例化新的 <xref:System.Security.Cryptography.DSA> 或 <xref:System.Security.Cryptography.RSA> 密钥时提供密钥参数。 它们允许你替换如下所示的代码：

```csharp
// Before .NET Framework 4.7.2
using (RSA rsa = RSA.Create())
{
   rsa.ImportParameters(rsaParameters);
   // Other code to execute using the RSA instance.
}
```

```vb
' Before .NET Framework 4.7.2
Using rsa = RSA.Create()
   rsa.ImportParameters(rsaParameters)
   ' Other code to execute using the rsa instance.
End Using
```

采用类似如下所示的代码：

```csharp
// Starting with .NET Framework 4.7.2
using (RSA rsa = RSA.Create(rsaParameters))
{
   // Other code to execute using the rsa instance.
}
```

```vb
' Starting with .NET Framework 4.7.2
Using rsa = RSA.Create(rsaParameters)
   ' Other code to execute using the rsa instance.
End Using
```

<xref:System.Security.Cryptography.DSA.Create(System.Int32)?displayProperty=nameWithType> 和 <xref:System.Security.Cryptography.RSA.Create(System.Int32)?displayProperty=nameWithType> 方法允许生成具有特定密钥大小的 <xref:System.Security.Cryptography.DSA> 或 <xref:System.Security.Cryptography.RSA> 密钥。 例如：

```csharp
using (DSA dsa = DSA.Create(2048))
{
   // Other code to execute using the dsa instance.
}
```

```vb
Using dsa = DSA.Create(2048)
   ' Other code to execute using the dsa instance.
End Using
```

**Rfc2898DeriveBytes 构造函数接受哈希算法名称**

<xref:System.Security.Cryptography.Rfc2898DeriveBytes> 类具有三个带 <xref:System.Security.Cryptography.HashAlgorithmName> 参数的构造函数，该参数标识在派生密钥时使用的 HMAC 算法。 与 SHA-1 相比，开发人员应使用基于 SHA-2 的 HMAC，例如 SHA-256，如下面的示例所示：

```csharp
private static byte[] DeriveKey(string password, out int iterations, out byte[] salt,
                                out HashAlgorithmName algorithm)
{
   iterations = 100000;
   algorithm = HashAlgorithmName.SHA256;

   const int SaltSize = 32;
   const int DerivedValueSize = 32;

   using (Rfc2898DeriveBytes pbkdf2 = new Rfc2898DeriveBytes(password, SaltSize,
                                                             iterations, algorithm))
   {
      salt = pbkdf2.Salt;
      return pbkdf2.GetBytes(DerivedValueSize);
   }
}
```

```vb
Private Shared Function DeriveKey(password As String, ByRef iterations As Integer,
                                  ByRef salt AS Byte(), ByRef algorithm As HashAlgorithmName) As Byte()
   iterations = 100000
   algorithm = HashAlgorithmName.SHA256

   Const SaltSize As Integer = 32
   Const  DerivedValueSize As Integer = 32

   Using pbkdf2 = New Rfc2898DeriveBytes(password, SaltSize, iterations, algorithm)
      salt = pbkdf2.Salt
      Return pbkdf2.GetBytes(DerivedValueSize)
   End Using
End Function
```

**临时密钥支持**

PFX 导入可以选择绕过硬盘直接从内存加载私钥。  如果在 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 构造函数或 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.Import%2A?displayProperty=nameWithType> 方法的其中一个重载中指定了新的 <xref:System.Security.Cryptography.X509Certificates.X509KeyStorageFlags.EphemeralKeySet?displayProperty=nameWithType> 标记，则私钥将加载为临时密钥。 这能防止密钥在磁盘上可见。 但是：

- 由于密钥不会保留到磁盘，最好不要将通过此标记加载的证书添加到 X509Store。

- 以这种方式加载的密钥大多都是通过 Windows CNG 加载的。 因此，调用方必须通过调用扩展方法访问私钥，例如 [cert.GetRSAPrivateKey()](xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey%2A)。 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType> 属性不起作用。

- 由于旧的 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType> 属性对证书不起作用，开发人员应在切换至临时密钥之前执行严密的测试。

**PKCS#10 证书签名请求和 X.509 公钥证书的编程式创建**

从 .NET Framework 4.7.2 开始，工作负载可以生成证书签名请求 (CSR)，这允许将证书请求生成分阶到现有工具中。 这在测试方案中常常很有用。

有关详细信息和代码示例，请参阅 [.NET 博客](https://devblogs.microsoft.com/dotnet/net-framework-4-7-2-developer-pack-early-access-build-3056-is-available/)中的“PKCS#10 证书签名请求和 X.509 公钥证书的编程式创建”。

**新的 SignerInfo 成员**

从 .NET Framework 4.7.2 开始，<xref:System.Security.Cryptography.Pkcs.SignerInfo> 类将公开更多有关签名的信息。 你可以检索 <xref:System.Security.Cryptography.Pkcs.SignerInfo.SignatureAlgorithm?displayProperty=fullName> 属性的值，以确定签名者采用的签名算法。 可以调用 <xref:System.Security.Cryptography.Pkcs.SignerInfo.GetSignature%2A?displayProperty=nameWithType> 来获取此签名者的加密签名副本。

**在 CryptoStream 释放后保持包装流打开**

从 .NET Framework 4.7.2 开始，<xref:System.Security.Cryptography.CryptoStream> 类有了一个额外的构造函数可允许 <xref:System.Security.Cryptography.CryptoStream.Dispose%2A> 不关闭包装流。  若要在释放 <xref:System.Security.Cryptography.CryptoStream> 实例后保持包装流的打开状态，请调用新的 <xref:System.Security.Cryptography.CryptoStream> 构造函数，如下所示：

```csharp
var cStream = new CryptoStream(stream, transform, mode, leaveOpen: true);
```

```vb
Dim cStream = New CryptoStream(stream, transform, mode, leaveOpen:=true)
```

**DeflateStream 中的解压缩更改**

从 .NET Framework 4.7.2 开始，<xref:System.IO.Compression.DeflateStream> 类中的解压缩操作的实现变为默认使用本机 Windows API。 通常情况下，这能大大地提高性能。

对于面向 .NET Framework 4.7.2 的应用程序，默认启用通过使用 Windows API 进行解压缩的支持。 对于面向旧版 .NET Framework 但在 .NET Framework 4.7.2 下运行的应用程序，可以将以下 [AppContext 开关](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)添加到应用程序配置文件，从而选择启用此行为：

```xml
<AppContextSwitchOverrides value="Switch.System.IO.Compression.DoNotUseNativeZipLibraryForDecompression=false" />
```

**额外的集合 API**

.NET Framework 4.7.2 将一些新 API 添加到 <xref:System.Collections.Generic.SortedSet%601> 和 <xref:System.Collections.Generic.HashSet%601> 类型。 这些方法包括：

- `TryGetValue` 方法，将其他集合类型中使用的尝试模式扩展到了这两种类型中。 这两个方法是：

  - [public bool HashSet\<T>.TryGetValue(T equalValue, out T actualValue)](xref:System.Collections.Generic.SortedSet%601.TryGetValue%2A)
  - [public bool SortedSet\<T>.TryGetValue(T equalValue, out T actualValue)](xref:System.Collections.Generic.SortedSet%601.TryGetValue%2A)

- `Enumerable.To*` 扩展方法，将集合转换为 <xref:System.Collections.Generic.HashSet%601>：

  - [public static HashSet\<TSource> ToHashSet\<TSource>(this IEnumerable\<TSource> source)](xref:System.Linq.Enumerable.ToHashSet%2A)
  - [public static HashSet\<TSource> ToHashSet\<TSource>(this IEnumerable\<TSource> source, IEqualityComparer\<TSource> comparer)](xref:System.Linq.Enumerable.ToHashSet%2A)

- 新的 <xref:System.Collections.Generic.HashSet%601> 构造函数，让你设置集合容量，可以在提前知道 <xref:System.Collections.Generic.HashSet%601> 大小的情况下提升性能：

  - [public HashSet(int capacity)](xref:System.Collections.Generic.HashSet%601.%23ctor(System.Int32))
  - [public HashSet(int capacity, IEqualityComparer\<T> comparer)](xref:System.Collections.Generic.HashSet%601.%23ctor(System.Int32,System.Collections.Generic.IEqualityComparer%7B%600%7D))

<xref:System.Collections.Concurrent.ConcurrentDictionary%602> 类包含 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A> 和 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> 方法的新重载，以便在词典中检索值或添加找不到的值，以及将值添加到词典或者更新已存在的值。

```csharp
public TValue AddOrUpdate<TArg>(TKey key, Func<TKey, TArg, TValue> addValueFactory, Func<TKey, TValue, TArg, TValue> updateValueFactory, TArg factoryArgument)

public TValue GetOrAdd<TArg>(TKey key, Func<TKey, TArg, TValue> valueFactory, TArg factoryArgument)
```

```vb
Public AddOrUpdate(Of TArg)(key As TKey, addValueFactory As Func(Of TKey, TArg, TValue), updateValueFactory As Func(Of TKey, TValue, TArg, TValue), factoryArgument As TArg) As TValue

Public GetOrAdd(Of TArg)(key As TKey, valueFactory As Func(Of TKey, TArg, TValue), factoryArgument As TArg) As TValue
```

<a name="asp-net472"></a>

#### <a name="aspnet"></a>ASP.NET

**Web 窗体中的依赖项注入支持**

[依赖项注入 (DI)](/aspnet/core/fundamentals/dependency-injection#overview-of-dependency-injection) 分离对象和它们的依赖项，使得对象的代码不再仅因依赖项更改而需要进行更改。 在开发面向 .NET Framework 4.7.2 的 ASP.NET 应用程序时，可以：

- 在[处理程序和模块](/previous-versions/aspnet/bb398986(v=vs.100))、[页面实例](xref:System.Web.UI.Page)和 ASP.NET Web 应用程序项目的[用户控件](/previous-versions/aspnet/y6wb1a0e(v=vs.100))中使用基于资源库、基于接口和基于构造函数的注入。

- 在[处理程序和模块](/previous-versions/aspnet/bb398986(v=vs.100))、[页面实例](xref:System.Web.UI.Page)和 ASP.NET 网站项目的[用户控件](/previous-versions/aspnet/y6wb1a0e(v=vs.100))中使用基于资源库和基于接口的注入。

- 插入不同的依赖关系注入框架。

**同站点 cookie 支持**

[SameSite](https://tools.ietf.org/html/draft-west-first-party-cookies-07) 防止浏览器将 cookie 和跨站点请求一起发送。 .NET Framework 4.7.2 添加了一个值为 <xref:System.Web.SameSiteMode?displayProperty=nameWithType> 枚举成员的 <xref:System.Web.HttpCookie.SameSite?displayProperty=nameWithType> 属性。 如果它的值为 <xref:System.Web.SameSiteMode.Strict?displayProperty=nameWithType> 或 <xref:System.Web.SameSiteMode.Lax?displayProperty=nameWithType>，ASP.NET 将 `SameSite` 属性添加到 set-cookie 标头。 SameSite 支持适用于 <xref:System.Web.HttpCookie> 对象，以及 <xref:System.Web.Security.FormsAuthentication> 和 <xref:System.Web.SessionState> cookie。

可以为 <xref:System.Web.HttpCookie> 对象设置 SameSite，如下所示：

```csharp
var c = new HttpCookie("secureCookie", "same origin");
c.SameSite = SameSiteMode.Lax;
```

```vb
Dim c As New HttpCookie("secureCookie", "same origin")
c.SameSite = SameSiteMode.Lax
```

还可以通过修改 web.config 文件，在应用程序级别配置 SameSite cookie：

```xml
<system.web>
   <httpCookies sameSite="Strict" />
</system.web>
```

通过修改 Web 配置文件，可以为 <xref:System.Web.Security.FormsAuthentication> 和 <xref:System.Web.SessionState> cookie 添加 SameSite：

```xml
<system.web>
   <authentication mode="Forms">
      <forms cookieSameSite="Lax">
         <!-- ...   -->
      </forms>
   </authentication>
   <sessionState cookieSameSite="Lax"></sessionState>
</system.web>
```

<a name="net472"></a>

#### <a name="networking"></a>网络

**HttpClientHandler 属性的实现**

.NET Framework 4.7.1 将八个属性添加到了 <xref:System.Net.Http.HttpClientHandler?displayProperty=nameWithType> 类。 不过其中有两个会引发 <xref:System.PlatformNotSupportedException>。 .NET Framework 4.7.2 现在为这些属性提供实现。 这些属性为：

- <xref:System.Net.Http.HttpClientHandler.CheckCertificateRevocationList>
- <xref:System.Net.Http.HttpClientHandler.SslProtocols>

<a name="sql472"></a>

#### <a name="sqlclient"></a>SQLClient

**对 Azure Active Directory 通用身份验证和多重身份验证的支持**

不断增加的符合性和安全性需求让很多客户需要使用多重身份验证 (MFA)。 此外，当前的最佳做法不鼓励在连接字符串中直接包含用户密码。 为了支持这些更改，.NET Framework 4.7.2 通过添加新值“Active Directory Interactive”扩展了 [SQLClient 连接字符串](xref:System.Data.SqlClient.SqlConnection.ConnectionString)，让现有“身份验证”关键字支持 MFA 和 [Azure AD 身份验证](/azure/sql-database/sql-database-aad-authentication-configure)。 新的交互式方法支持本机和联合 Azure AD 用户以及 Azure AD 来宾用户。 使用此方法时，SQL 数据库将支持由 Azure AD 施加的 MFA 身份验证。 此外，为了遵循安全最佳做法，身份验证流程会请求用户密码。

在以前版本的 .NET Framework 中，SQL 连接只支持 <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryPassword?displayProperty=nameWithType> 和 <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryIntegrated?displayProperty=nameWithType> 选项。 两者都是非交互式 [ADAL 协议](/azure/active-directory/develop/active-directory-authentication-libraries)的一部分，而该协议不支持 MFA。 利用新的 <xref:System.Data.SqlClient.SqlAuthenticationMethod.ActiveDirectoryInteractive?displayProperty=nameWithType> 选项，SQL 连接可支持 MFA 以及现有身份验证方法（密码和集成身份验证），让用户可以交互式地输入用户密码，而无需将密码存留在连接字符串中。

有关详细信息和示例，请参阅 [.NET 博客](https://devblogs.microsoft.com/dotnet/net-framework-4-7-2-developer-pack-early-access-build-3056-is-available/)中的“SQL - Azure AD 通用和多重身份验证支持”。

**Always Encrypted 版本 2 的支持**

NET Framework 4.7.2 为基于 enclave 的 Always Encrypted 添加支持。 Always Encrypted 的原始版本是客户端加密技术，其中的加密密钥不会离开客户端。 在基于 enclave 的 Always Encrypted 中，客户端可以选择将加密密钥发送到安全的 enclave，即一个安全的计算实体，该实体可以看作是 SQL Server 的一部分，但 SQL Server 代码无法对其进行篡改。 为了支持基于 enclave 的 Always Encrypted，NET Framework 4.7.2 将以下类型和成员添加到 <xref:System.Data.SqlClient> 命名空间：

- <xref:System.Data.SqlClient.SqlConnectionStringBuilder.EnclaveAttestationUrl?displayProperty=nameWithType>，为基于 enclave 的 Always Encrypted 指定 URI。

- <xref:System.Data.SqlClient.SqlColumnEncryptionEnclaveProvider>，一个抽象类，所有 enclave 提供程序都自它派生。

- <xref:System.Data.SqlClient.SqlEnclaveSession>，封装给定 enclave 会话的状态。

- <xref:System.Data.SqlClient.SqlEnclaveAttestationParameters>，提供 SQL Server 所使用的认证参数，以获取执行特定认证协议所需的信息。

抽象 <xref:System.Data.SqlClient.SqlColumnEncryptionEnclaveProvider?displayProperty=nameWithType> 类提供了 enclave 提供程序的功能，应用程序配置文件随后会指定该类的具体实现。 例如：

```xml
<configuration>
  <configSections>
    <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection,System.Data,Version=4.0.0.0,Culture=neutral,PublicKeyToken=b77a5c561934e089"/>
  </configSections>
  <SqlColumnEncryptionEnclaveProviders>
    <providers>
      <add name="Azure" type="Microsoft.SqlServer.Management.AlwaysEncrypted.AzureEnclaveProvider,MyApp"/>
      <add name="HGS" type="Microsoft.SqlServer.Management.AlwaysEncrypted.HGSEnclaveProvider,MyApp" />
    </providers>
  </SqlColumnEncryptionEnclaveProviders >
</configuration>
```

基于 enclave 的 Always Encrypted 的基本流是：

1. 用户创建与 SQL Server（支持基于 enclave 的 Always Encrypted）的 AlwaysEncrypted 连接。 驱动程序联系认证服务以确保它连接到正确的 enclave。

1. enclave 验证成功后，驱动程序将建立与托管在 SQL Server 上的安全 enclave 之间的安全信道。

1. 在 SQL 连接期间，驱动程序与安全 enclave 共享由客户端授权的加密密钥。

<a name="wpf472"></a>

#### <a name="windows-presentation-foundation"></a>Windows Presentation Foundation

**按源查找 ResourceDictionaries**

从 .NET Framework 4.7.2 开始，诊断助手可以找到从给定源 URI 创建的 <xref:System.Windows.Xps.Packaging.IXpsFixedPageReader.ResourceDictionaries>。  （此功能通过诊断助手使用，而非生产应用程序。）通过诊断助手（例如 Visual Studio 的“编辑并继续”），用户可编辑 ResourceDictionary，将更改应用于正在运行的应用程序。 要实现这一点，其中一个步骤是从被编辑的字典中找到正在运行的应用程序创建的所有 ResourceDictionary。 例如，应用程序可以声明某个从给定源 URI 复制内容的 ResourceDictionary：

```xml
<ResourceDictionary Source="MyRD.xaml" />
```

编辑 MyRD.xaml 中的原始标记的诊断助手可以使用新功能来找到字典。  此功能通过新的静态方法 <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetResourceDictionariesForSource%2A?displayProperty=nameWithType> 实现。 诊断助手使用标识原始标记的绝对 URI 调用新方法，如以下代码所示：

```csharp
IEnumerable<ResourceDictionary> dictionaries = ResourceDictionaryDiagnostics.GetResourceDictionariesForSource(new Uri("pack://application:,,,/MyApp;component/MyRD.xaml"));
```

```vb
Dim dictionaries As IEnumerable(Of ResourceDictionary) = ResourceDictionaryDiagnostics.GetResourceDictionariesForSource(New Uri("pack://application:,,,/MyApp;component/MyRD.xaml"))
```

该方法返回空的枚举值，除非启用了 <xref:System.Windows.Diagnostics.VisualDiagnostics> 并且设置了 [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A) 环境变量。

**查找 ResourceDictionary 所有者**

从 .NET Framework 4.7.2 开始，诊断助手可以找到给定 <xref:Windows.UI.Xaml.ResourceDictionary> 的所有者。  （此功能供诊断助手，而非生产应用程序使用。）每当对 <xref:Windows.UI.Xaml.ResourceDictionary> 做出更改时，WPF 会自动查找所有可能会受此更改影响的 [DynamicResource](/dotnet/desktop/wpf/advanced/dynamicresource-markup-extension) 引用。

诊断助手（例如 Visual Studio 的“编辑并继续”）可能想对此进行扩展以处理 [StaticResource](/dotnet/desktop/wpf/advanced/staticresource-markup-extension) 引用。 此过程的第一步是找到字典的所有者，也就是找到其 `Resources` 属性引用该字典（不管是直接引用，还是通过 <xref:System.Windows.ResourceDictionary.MergedDictionaries?displayProperty=nameWithType> 属性间接引用）的所有对象。 <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics?displayProperty=nameWithType> 类上实现的三个新的静态方法（每个对应具有 `Resources` 属性的基类型）支持此步骤：

- [`public static IEnumerable<FrameworkElement> GetFrameworkElementOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetFrameworkElementOwners%2A)

- [`public static IEnumerable<FrameworkContentElement> GetFrameworkContentElementOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetFrameworkContentElementOwners%2A)

- [`public static IEnumerable<Application> GetApplicationOwners(ResourceDictionary dictionary);`](xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.GetApplicationOwners%2A)

这些方法返回空的枚举值，除非启用了 <xref:System.Windows.Diagnostics.VisualDiagnostics> 并且设置了 [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A) 环境变量。

**查找 StaticResource 引用**

现在，每当一个 [StaticResource](/dotnet/desktop/wpf/advanced/staticresource-markup-extension) 引用被解析时，诊断助手都能收到通知。  （此功能供诊断助手，而非生产应用程序使用。）诊断助手（例如 Visual Studio 的“编辑并继续”）可能想在 <xref:Windows.UI.Xaml.ResourceDictionary> 中某个资源的值发生更改时更新该资源的所有使用。 WPF 为 [DynamicResource](/dotnet/desktop/wpf/advanced/dynamicresource-markup-extension) 引用自动完成此操作，但不会为 [StaticResource](/dotnet/desktop/wpf/advanced/staticresource-markup-extension) 引用有意执行该操作。 从 .NET Framework 4.7.2 开始，诊断助手可以利用这些通知来查找静态资源的使用情况。

该通知由新的 <xref:System.Windows.Diagnostics.ResourceDictionaryDiagnostics.StaticResourceResolved?displayProperty=nameWithType> 事件实现：

```csharp
public static event EventHandler<StaticResourceResolvedEventArgs> StaticResourceResolved;
```

```vb
Public Shared Event StaticResourceResolved As EventHandler(Of StaticResourceResolvedEventArgs)
```

每当运行时解析 [StaticResource](/dotnet/desktop/wpf/advanced/staticresource-markup-extension) 引用时，都会引发此事件。 <xref:System.Windows.Diagnostics.StaticResourceResolvedEventArgs> 参数描述解析，并指示托管 [StaticResource](/dotnet/desktop/wpf/advanced/staticresource-markup-extension) 引用的对象和属性及用于解析的 <xref:Windows.UI.Xaml.ResourceDictionary> 和密钥：

```csharp
public class StaticResourceResolvedEventArgs : EventArgs
{
   public Object TargetObject { get; }

   public Object TargetProperty { get; }

   public ResourceDictionary ResourceDictionary { get; }

   public object ResourceKey { get; }
}
```

```vb
Public Class StaticResourceResolvedEventArgs : Inherits EventArgs
   Public ReadOnly Property TargetObject As Object
   Public ReadOnly Property TargetProperty As Object
   Public ReadOnly Property ResourceDictionary As ResourceDictionary
   Public ReadOnly Property ResourceKey As Object
End Class
```

除非启用 <xref:System.Windows.Diagnostics.VisualDiagnostics> 并设置了 [`ENABLE_XAML_DIAGNOSTICS_SOURCE_INFO`](xref:System.Windows.Diagnostics.VisualDiagnostics.GetXamlSourceInfo%2A) 环境变量，否则不会引发该事件（且忽略它的 `add` 访问器）。

#### <a name="clickonce"></a>ClickOnce

Windows 窗体的 HDPI 感知应用程序、Windows Presentation Foundation (WPF) 以及 Visual Studio Tools for Office (VSTO) 都可以通过使用 ClickOnce 进行部署。 如果在应用程序清单中找到了以下条目，则部署将在 .NET Framework 4.7.2 下成功执行：

```xml
<windowsSettings>
   <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
</windowsSettings>
```

对于 Windows 窗体应用程序，不需要像以前那样在应用程序配置文件（而非应用程序清单）中设置 DPI 感知就可以成功完成 ClickOnce 部署。

<a name="v471"></a>

## <a name="whats-new-in-net-framework-471"></a>.NET Framework 4.7.1 中的新增功能

.NET Framework 4.7.1 在以下几个领域新增了功能：

- [基类](#core471)
- [公共语言运行时 (CLR)](#clr)
- [网络连接](#net471)
- [ASP.NET](#asp-net471)

此外，.NET Framework 4.7.1 的重点是改进了辅助功能，使应用程序能为使用辅助技术的用户提供最佳体验。 有关 .NET Framework 4.7.1 中辅助功能改进的信息，请参阅 [.NET Framework 中辅助功能的新增功能](whats-new-in-accessibility.md)。

<a name="core471"></a>

#### <a name="base-classes"></a>基类

**支持 .NET Standard 2.0**

[.NET Standard](../../standard/net-standard.md) 定义一组 API，这些 API 必须可用于支持 Standard 版本的每个 .NET 实现。 .NET Framework 4.7.1 完全支持 .NET Standard 2.0 并增加了[ 200 个 API](https://github.com/dotnet/standard/blob/master/src/netstandard/src/ApiCompatBaseline.net461.txt)，这些 API 在 .NET Standard 2.0 中定义，.NET Framework 4.6.1、4.6.2 和 4.7 中也延续使用。 （请注意，这些版本的 .NET Framework 只有在其他 .NET Standard 支持文件也部署在目标系统时才支持 .NET Standard 2.0。）有关详细信息，请参阅 [.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)（.NET Framework 4.7.1 运行时和编译器功能）博客文章中的“BCL - .NET Standard 2.0 Support”（BCL - .NET Standard 2.0 支持）。

**支持配置生成器**

配置生成器允许开发者在运行时动态地插入和生成应用程序的配置设置。 自定义配置生成器可用于修改配置节中的现有数据，也可用于生成全新的配置节。 如果没有配置生成器，.config 文件将是静态的，并且其设置将在应用程序启动之前定义。

若要创建自定义配置生成器，请从抽象的 <xref:System.Configuration.ConfigurationBuilder> 类派生生成器并且替代其 <xref:System.Configuration.ConfigurationBuilder.ProcessConfigurationSection%2A?displayProperty=nameWithType> 和 <xref:System.Configuration.ConfigurationBuilder.ProcessRawXml%2A?displayProperty=nameWithType>。 也可在 .config 文件中定义生成器。 有关详细信息，请参阅 [.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/)（.NET Framework 4.7.1 ASP.NET 和配置功能）博客文章中的“Configuration Builders”（配置生成器）一节。

**运行时功能检测**

<xref:System.Runtime.CompilerServices.RuntimeFeature?displayProperty=nameWithType> 类提供一种机制，用于确定给定的 .NET 实现在编译时或运行时是否支持预定义的功能。 在编译时，编译器可以检查指定的字段是否存在，以确定是否支持某项功能，如果支持，它会发出利用这一功能的代码。 在运行时，应用程序可以在运行时发出代码之前调用 <xref:System.Runtime.CompilerServices.RuntimeFeature.IsSupported%2A?displayProperty=nameWithType> 方法。 有关详细信息，请参阅 [Add helper method to describe features supported by the runtime](https://github.com/dotnet/corefx/issues/17116)（添加 helper 方法以描述运行时支持的功能）。

**值元组类型是可序列化的**

从 .NET Framework 4.7.1 起，<xref:System.ValueTuple?displayProperty=nameWithType> 及其相关的泛型类型被标记为[可序列化](xref:System.SerializableAttribute)，允许进行二进制序列化。 这样，可以更轻松地将元组类型（如 <xref:System.Tuple%603> 和 <xref:System.Tuple%604>）迁移到值元组类型。 有关详细信息，请参阅 [.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)（.NET Framework 4.7.1 运行时和编译器功能）博客文章中的“Compiler -- ValueTuple is Serializable”（编译器 -- 值元组是可序列化的）。

**支持只读引用**

.NET Framework 4.7.1 增加了 <xref:System.Runtime.CompilerServices.IsReadOnlyAttribute?displayProperty=nameWithType>。 此特性由语言编译器用于标记具有只读 ref 返回类型或参数的成员。 有关详细信息，请参阅 [.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)（.NET Framework 4.7.1 运行时和编译器功能）博客文章中的“Compiler -- Support for ReadOnlyReferences”（编译器 -- 支持只读引用）。 有关 ref 返回值的信息，请参阅 [ref 返回值和 ref 局部变量（C# 指南）](../../csharp/programming-guide/classes-and-structs/ref-returns.md)和 [ref 返回值 (Visual Basic)](../../visual-basic/programming-guide/language-features/procedures/ref-return-values.md)。

<a name="clr"></a>

#### <a name="common-language-runtime-clr"></a>公共语言运行时 (CLR)

**垃圾回收性能改进**

.NET Framework 4.7.1 中的垃圾回收 (GC) 的更改提升了整体性能，尤其是大型对象堆 (LOH) 分配的性能。 在 .NET Framework 4.7.1 中，小型对象堆 (SOH) 分配和 LOH 分配使用不同的锁，当后台 GC 整理 SOH 时即发生 LOH 分配。 这样，进行大量 LOH 分配的应用程序发生分配锁争用的情况将减少，从而提高性能。 有关详细信息，请参阅 [.NET Framework 4.7.1 Runtime and Compiler Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-runtime-and-compiler-features/)（.NET Framework 4.7.1 运行时和编译器功能）博客文章中的“Runtime -- GC Performance Improvements”（运行时 -- GC 性能改进）一节。

<a name="net471"/>

#### <a name="networking"></a>网络

**Message.HashAlgorithm 的 SHA-2 支持**

在 .NET Framework 4.7 及早期版本中，<xref:System.Messaging.Message.HashAlgorithm%2A?displayProperty=nameWithType> 属性仅支持 <xref:System.Messaging.HashAlgorithm.Md5?displayProperty=nameWithType> 和 <xref:System.Messaging.HashAlgorithm.Sha?displayProperty=nameWithType> 的值。 从 .NET Framework 4.7.1 开始，还支持 <xref:System.Messaging.HashAlgorithm.Sha256?displayProperty=nameWithType>、<xref:System.Messaging.HashAlgorithm.Sha384?displayProperty=nameWithType> 和 <xref:System.Messaging.HashAlgorithm.Sha512?displayProperty=nameWithType>。 实际是否使用此值取决于消息队列，因为 <xref:System.Messaging.Message> 实例本身不进行哈希处理，而是简单地将值传递到消息队列。 有关详细信息，请参阅 [.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/)（.NET Framework 4.7.1 ASP.NET 和配置功能）博客文章中的“SHA-2 support for Message.HashAlgorithm”（Message.HashAlgorithm 的 SHA-2 支持）一节。

<a name="asp-net471"></a>

#### <a name="aspnet"></a>ASP.NET

**ASP.NET 应用程序中的执行步骤**

ASP.NET 处理包括 23 个事件的预定义管道中的请求。 ASP.NET 执行每个事件处理程序作为一个执行步骤。 对于 .NET Framework 4.7 之前的 ASP.NET 版本，由于本机和托管线程之间的切换，ASP.NET 无法传送执行上下文。 ASP.NET 有选择性地仅传送 <xref:System.Web.HttpContext>。 从 .NET Framework 4.7.1 开始，<xref:System.Web.HttpApplication.OnExecuteRequestStep(System.Action{System.Web.HttpContextBase,System.Action})?displayProperty=nameWithType> 方法还允许模块还原环境数据。 此功能针对与跟踪、分析、诊断或事务（例如应用程序的执行流）相关的库。 有关详细信息，请参阅 [.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/)（.NET Framework 4.7.1 ASP.NET 和配置功能）博客文章的“ASP.NET Execution Step Feature”（ASP.NET 执行步骤功能）一节。

**ASP.NET HttpCookie 分析**

.NET Framework 4.7.1 包括新的方法 <xref:System.Web.HttpCookie.TryParse%2A?displayProperty=nameWithType>，此方法提供标准化的方式来从字符串创建 <xref:System.Web.HttpCookie> 对象，并精确分配 cookie 值（如过期日期和路径）。 有关详细信息，请参阅 [.NET Framework 4.7.1 ASP.NET and Configuration Features](https://devblogs.microsoft.com/dotnet/net-framework-4-7-1-asp-net-and-configuration-features/)（.NET Framework 4.7.1 ASP.NET 和配置功能）博客文章中的“ASP.NET HttpCookie parsing”（ASP.NET HttpCookie 分析）一节。

**ASP.NET 窗体身份验证凭据的 SHA-2 哈希选项**

在 .NET Framework 4.7 及其早期版本中，ASP.NET 允许开发者使用 MD5 或 SHA1 在配置文件中存储用户凭据和哈希密码。 从 .NET Framework 4.7.1 开始，ASP.NET 还支持新的安全 SHA-2 哈希选项（如 SHA256、SHA384 和 SHA512）。 SHA1 保留默认值，非默认哈希算法可以在 Web 配置文件中定义。 例如：

```xml
<system.web>
    <authentication mode="Forms">
        <forms loginUrl="~/login.aspx">
          <credentials passwordFormat="SHA512">
            <user name="jdoe" password="6D003E98EA1C7F04ABF8FCB375388907B7F3EE06F278DB966BE960E7CBBD103DF30CA6D61F7E7FD981B2E4E3A64D43C836A4BEDCA165C33B163E6BCDC538A664" />
          </credentials>
        </forms>
    </authentication>
</system.web>
```

<a name="v47"></a>

## <a name="whats-new-in-net-framework-47"></a>.NET Framework 4.7 中的新增功能

.NET Framework 4.7 在以下几个领域新增了功能：

- [基类](#Core47)
- [网络连接](#net47)
- [ASP.NET](#ASP-NET47)
- [Windows Communication Foundation (WCF)](#wcf47)
- [Windows 窗体](#wf47)
- [Windows Presentation Foundation (WPF)](#WPF47)

有关 .NET Framework 4.7 中新增 API 的列表，请参阅 GitHub 上的 [.NET Framework 4.7 API 更改](https://github.com/Microsoft/dotnet/blob/master/releases/net47/dotnet47-api-changes.md)。 有关 .NET Framework 4.7 中功能改进和 bug 修复的列表，请参阅 GitHub 上的 [.NET Framework 4.7 更改列表](https://github.com/Microsoft/dotnet/blob/master/releases/net47/dotnet47-changes.md)。 有关详细信息，请参阅 .NET 博客中的 [Announcing the .NET Framework 4.7](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-7/)（发布 .NET Framework 4.7）。

<a name="Core47"></a>

#### <a name="base-classes"></a>基类

.NET Framework 4.7 通过 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 改进了序列化：

**借助椭圆曲线加密 (ECC) 增强了功能** _

在 .NET Framework 4.7 中，`ImportParameters(ECParameters)` 方法已添加到 <xref:System.Security.Cryptography.ECDsa> 和 <xref:System.Security.Cryptography.ECDiffieHellman> 类，以允许对象表示已经建立的密钥。 此外，还添加了 `ExportParameters(Boolean)` 方法，以便于使用显式曲线参数导出密钥。

.NET Framework 4.7 现已开始支持其他曲线（其中包括 Brainpool 曲线套件），并添加了预定义的定义，以便于通过新工厂方法 <xref:System.Security.Cryptography.ECDsa.Create%2A> 和 <xref:System.Security.Cryptography.ECDiffieHellman.Create%2A> 简化创建操作。

有关 [.NET Framework 4.7 加密改进示例](https://gist.github.com/richlander/5a182899895a87a296c21ada97f7a54e)，请访问 GitHub。

_ *DataContractJsonSerializer 提供更出色的控制字符支持**

在 .NET Framework 4.7 中，<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 类会将符合 ECMAScript 6 标准的控制字符串行化。 面向 .NET Framework 4.7 的应用程序默认启用此行为，而对于在 .NET Framework 4.7 下运行但面向 .NET Framework 先前版本的应用程序来说，可选择启用此功能。 有关详细信息，请参阅[应用程序兼容性](../migration-guide/application-compatibility.md)部分。

<a name="net47"></a>

#### <a name="networking"></a>网络

.NET Framework 4.7 新增了以下网络相关功能：

**提供对 TLS 协议的默认操作系统支持** _

借助 <xref:System.Net.Security.SslStream?displayProperty=nameWithType> 和上托堆栈组件（如 HTTP、FTP 和 SMTP）使用的 TLS 堆栈，开发者可以使用操作系统支持的默认 TLS 协议。 开发者再也不需要对 TLS 版本进行硬编码。

<a name="ASP-NET47"></a>

#### <a name="aspnet"></a>ASP.NET

在 .NET Framework 4.7 中，ASP.NET 新增了以下功能：

_ *对象缓存扩展性**

自 .NET Framework 4.7 起，ASP.NET 新增了一组 API，以便开发者可以替换内存中对象缓存和内存监视的默认 ASP.NET 实现代码。 现在，如果 ASP.NET 实现代码不充分，开发者可以替换以下三个组件中的任意一个：

- **对象缓存存储**： 在新的缓存提供程序配置部分中，开发者可以使用新接口 **ICacheStoreProvider** 为 ASP.NET 应用程序插入对象缓存的新实现代码。

- **内存监视**： 当运行的应用程序接近所配置的专用字节进程限制，或计算机的可用总物理内存不足时，ASP.NET 中的默认内存监视器就会通知应用程序。 接近这些限制时，就会触发通知。 对于某些应用程序，通知的触发点与限制过近，无法及时响应。 开发人员现在可以编写自己的内存监视器，以通过使用 <xref:System.Web.Hosting.ApplicationMonitors.MemoryMonitor%2A?displayProperty=nameWithType> 属性替换默认的内存监视器。

- **内存限制响应**： 默认情况下，当接近专用字节进程限制时，ASP.NET 会尝试释放对象缓存，并定期调用 <xref:System.GC.Collect%2A?displayProperty=nameWithType>。 对于某些应用程序，<xref:System.GC.Collect%2A?displayProperty=nameWithType> 调用频率或缓存释放量的效率低下。 开发者现在可以向应用程序的内存监视器添加 **IObserver** 实现代码，从而替换或补充默认行为。

<a name="wcf47"></a>

#### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

Windows Communication Foundation (WCF) 新增了以下功能和更改：

**能够配置 TLS 1.1 或 TLS 1.2 默认消息安全设置**

自 .NET Framework 4.7 起，除了 SSL 3.0 和 TLS 1.0 外，WCF 还允许配置 TLS 1.1 或 TLS 1.2 作为默认消息安全协议。 这是一项选择启用设置；必须向应用程序配置文件添加以下条目，才能启用此设置：

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols=false;Switch.System.Net.DontEnableSchUseStrongCrypto=false" />
</runtime>
```

**提升了 WCF 应用程序和 WCF 序列化的可靠性**

WCF 包含大量代码更改，消除了争用条件，从而提升了序列化选项的性能和可靠性。 其中包括:

- 更好地支持在调用 **SocketConnection.BeginRead** 和 **SocketConnection.Read** 时混合异步和同步代码。
- 提升了在中止与 **SharedConnectionListener** 和 **DuplexChannelBinder** 的连接时的可靠性。
- 提高了调用 <xref:System.Runtime.Serialization.FormatterServices.GetSerializableMembers%28System.Type%29?displayProperty=nameWithType> 方法时序列化操作的可靠性。
- 提升了在调用 **ChannelSynchronizer.RemoveWaiter** 方法以删除等待程序时的可靠性。

<a name="wf47"></a>

#### <a name="windows-forms"></a>Windows 窗体

在 .NET Framework 4.7 中，Windows 窗体改进了对高 DPI 监视器的支持。

**高 DPI 支持**

自定位 .NET Framework 4.7 的应用程序起，.NET Framework 为 Windows 窗体应用程序提供高 DPI 和动态 DPI 支持。 高 DPI 支持改进了高 DPI 监视器上窗体和控件的布局和外观。 当用户更改正在运行的应用程序的 DPI 或显示比例系数时，动态 DPI 会更改窗体和控件的布局和外观。

高 DPI 支持是一项选择启用功能，可通过在应用程序配置文件中定义 [\<System.Windows.Forms.ConfigurationSection>](../configure-apps/file-schema/winforms/index.md) 部分进行配置。 若要详细了解如何向 Windows 窗体应用程序添加高 DPI 支持和动态 DPI 支持，请参阅 [Windows 窗体中的高 DPI 支持](/dotnet/desktop/winforms/high-dpi-support-in-windows-forms)。

<a name="WPF47"></a>

#### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

在 .NET Framework 4.7 中，WPF 新增了以下增强功能：

**支持基于 Windows WM_POINTER 消息的触控/触笔堆栈**

现在可以视情况使用基于 [WM_POINTER 消息](/previous-versions/windows/desktop/InputMsg/messages)的触控/触笔堆栈，而不使用 Windows Ink 服务平台 (WISP)。 这是 .NET Framework 中的一项可选择启用的功能。 有关详细信息，请参阅[应用程序兼容性](../migration-guide/application-compatibility.md)部分。

**WPF 打印 API 的新实现代码**

<xref:System.Printing.PrintQueue?displayProperty=nameWithType> 类中的 WPF 打印 API 调用 Windows [打印文档包 API](/windows/desktop/printdocs/tailored-app-printing-api)，而不调用弃用的 [XPS 打印 API](/windows/desktop/printdocs/xps-printing)。 要了解此更改对应用程序兼容性的影响，请参阅[应用程序兼容性](../migration-guide/application-compatibility.md)部分。

<a name="v462"></a>

## <a name="whats-new-in-net-framework-462"></a>.NET Framework 4.6.2 中的新增功能

.NET Framework 4.6.2 在以下几个领域新增了功能：

- [ASP.NET](#ASPNET462)

- [字符类别](#Strings)

- [加密](#Crypto462)

- [SqlClient](#SQLClient)

- [Windows Communication Foundation](#WCF)

- [Windows Presentation Foundation (WPF)](#WPF462)

- [Windows Workflow Foundation (WF)](#WF462)

- [ClickOnce](#clickonce-1)

- [将 Windows 窗体和 WPF 应用转换为 UWP 应用](#UWPConvert)

- [调试改进](#Debug462)

有关 .NET Framework 4.6.2 中新增 API 的列表，请参阅 GitHub 上的 [.NET Framework 4.6.2 API 更改](https://github.com/Microsoft/dotnet/blob/master/releases/net462/dotnet462-api-changes.md)。 有关 .NET Framework 4.6.2 中功能改进和 bug 修复的列表，请参阅 GitHub 上的 [.NET Framework 4.6.2 更改列表](https://github.com/Microsoft/dotnet/blob/master/releases/net462/dotnet462-changes.md)。 有关详细信息，请参阅 .NET 博客中的 [Announcing the .NET Framework 4.6.2](https://devblogs.microsoft.com/dotnet/announcing-net-framework-4-6-2/)（发布 .NET Framework 4.6.2）。

<a name="ASPNET462"></a>

### <a name="aspnet"></a>ASP.NET

在 .NET Framework 4.6.2 中，ASP.NET 包括以下增强功能：

**改进了对数据注释验证程序中本地化错误消息的支持**

数据批注验证程序使你能够通过将一个或多个属性添加到类属性来执行验证。 如果验证失败，该属性的 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> 元素定义错误消息的文本。 自 .NET Framework 4.6.2 起，ASP.NET 简化了错误消息的本地化。 如果有以下情况，将本地化错误消息：

1. 验证属性中提供 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType>。

2. 资源文件存储在 App_LocalResources 文件夹中。

3. 本地化资源文件名称的格式为 `DataAnnotation.Localization.{`*name*`}.resx`，其中 *name* 是采用 *languageCode*`-`*country/regionCode* 或 *languageCode* 格式的区域性名称。

4. 该资源的项名称是分配给 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.ErrorMessage%2A?displayProperty=nameWithType> 属性的字符串，其值是本地化的错误消息。

例如，以下数据注释属性定义无效分级的默认区域性错误消息。

```csharp
public class RatingInfo
{
   [Required(ErrorMessage = "The rating must be between 1 and 10.")]
   [Display(Name = "Your Rating")]
   public int Rating { get; set; }
}
```

```vb
Public Class RatingInfo
   <Required(ErrorMessage = "The rating must be between 1 and 10.")>
   <Display(Name = "Your Rating")>
   Public Property Rating As Integer = 1
End Class
```

然后可以创建一个资源文件 DataAnnotation.Localization.fr.resx，它的键为错误消息字符串，值为本地化的错误消息。 该文件必须位于 `App.LocalResources` 文件夹中。 例如，下面列出了键以及它在本地化法语 (fr) 错误消息中的值：

| “属性”                                 | “值”                                     |
| ------------------------------------ | ----------------------------------------- |
| 分级必须介于 1 和 10 之间。 | La note doit être comprise entre 1 et 10. |

 此外，数据批注本地化可扩展。 开发人员可以通过实现 <xref:System.Web.Globalization.IStringLocalizerProvider> 接口插入自己的字符串本地化工具提供程序，以将本地化字符串存储在资源文件以外的某个位置。

 **会话状态存储提供程序的异步支持**

 ASP.NET 现允许将返回任务的方法与会话状态存储提供程序一起使用，从而允许 ASP.NET 应用获取异步的可伸缩性优势。 要使用会话状态存储提供程序支持异步操作，ASP.NET 包括一个新的接口 <xref:System.Web.SessionState.ISessionStateModule?displayProperty=nameWithType>，它继承自 <xref:System.Web.IHttpModule> 并允许开发人员实现其自己的会话状态模块和异步会话存储提供程序。 接口定义如下：

```csharp
public interface ISessionStateModule : IHttpModule {
    void ReleaseSessionState(HttpContext context);
    Task ReleaseSessionStateAsync(HttpContext context);
}
```

```vb
Public Interface ISessionStateModule : Inherits IHttpModule
   Sub ReleaseSessionState(context As HttpContext)
   Function ReleaseSessionStateAsync(context As HttpContext) As Task
End Interface
```

 此外，<xref:System.Web.SessionState.SessionStateUtility> 类包括两种新方法：<xref:System.Web.SessionState.SessionStateUtility.IsSessionStateReadOnly%2A> 和 <xref:System.Web.SessionState.SessionStateUtility.IsSessionStateRequired%2A>，可用来支持异步操作。

 **对输出缓存提供程序的异步支持**

 自 .NET Framework 4.6.2 起，返回任务的方法可与输出缓存提供程序结合使用，从而实现异步的可伸缩性优势。  实现这些方法的提供程序减少了 Web 服务器上的线程阻止，并提高 ASP.NET 服务的可伸缩性。

 添加了以下 API 以支持异步输出缓存提供程序：

- <xref:System.Web.Caching.OutputCacheProviderAsync?displayProperty=nameWithType> 类，它继承自 <xref:System.Web.Caching.OutputCacheProvider?displayProperty=nameWithType>，并允许开发人员实现异步输出缓存提供程序。

- <xref:System.Web.Caching.OutputCacheUtility> 类，该类提供用于配置输出缓存的 Helper 方法。

- <xref:System.Web.HttpCachePolicy?displayProperty=nameWithType> 类中的 18 种新方法。 它们包括 <xref:System.Web.HttpCachePolicy.GetCacheability%2A>、<xref:System.Web.HttpCachePolicy.GetCacheExtensions%2A>、<xref:System.Web.HttpCachePolicy.GetETag%2A>、<xref:System.Web.HttpCachePolicy.GetETagFromFileDependencies%2A>、<xref:System.Web.HttpCachePolicy.GetMaxAge%2A>、<xref:System.Web.HttpCachePolicy.GetMaxAge%2A>、<xref:System.Web.HttpCachePolicy.GetNoStore%2A>、<xref:System.Web.HttpCachePolicy.GetNoTransforms%2A>、<xref:System.Web.HttpCachePolicy.GetOmitVaryStar%2A>、<xref:System.Web.HttpCachePolicy.GetProxyMaxAge%2A>、<xref:System.Web.HttpCachePolicy.GetRevalidation%2A>、<xref:System.Web.HttpCachePolicy.GetUtcLastModified%2A>、<xref:System.Web.HttpCachePolicy.GetVaryByCustom%2A>、<xref:System.Web.HttpCachePolicy.HasSlidingExpiration%2A> 和 <xref:System.Web.HttpCachePolicy.IsValidUntilExpires%2A>。

- <xref:System.Web.HttpCacheVaryByContentEncodings?displayProperty=nameWithType> 类中的两种新方法：<xref:System.Web.HttpCacheVaryByContentEncodings.GetContentEncodings%2A> 和 <xref:System.Web.HttpCacheVaryByContentEncodings.SetContentEncodings%2A>。

- <xref:System.Web.HttpCacheVaryByHeaders?displayProperty=nameWithType> 类中的两种新方法：<xref:System.Web.HttpCacheVaryByHeaders.GetHeaders%2A> 和 <xref:System.Web.HttpCacheVaryByHeaders.SetHeaders%2A>。

- <xref:System.Web.HttpCacheVaryByParams?displayProperty=nameWithType> 类中的两种新方法：<xref:System.Web.HttpCacheVaryByParams.GetParams%2A> 和 <xref:System.Web.HttpCacheVaryByParams.SetParams%2A>。

- <xref:System.Web.Caching.AggregateCacheDependency?displayProperty=nameWithType> 类中的 <xref:System.Web.Caching.AggregateCacheDependency.GetFileDependencies%2A> 方法。

- <xref:System.Web.Caching.CacheDependency> 中的 <xref:System.Web.Caching.CacheDependency.GetFileDependencies%2A> 方法。

<a name="Strings"></a>

### <a name="character-categories"></a>字符类别

.NET Framework 4.6.2 中的字符是根据 [Unicode 标准 8.0.0 版](https://www.unicode.org/versions/Unicode8.0.0/)进行分类的。 在 .NET Framework 4.6 和 .NET Framework 4.6.1 中，字符是根据 Unicode 6.3 字符类别进行分类。

对 Unicode 8.0 的支持限于 <xref:System.Globalization.CharUnicodeInfo> 类的字符分类以及依赖它的类型和方法。 其中包括 <xref:System.Globalization.StringInfo> 类、重载的 <xref:System.Char.GetUnicodeCategory%2A?displayProperty=nameWithType> 方法和 .NET Framework 正则表达式引擎识别的[字符类](../../standard/base-types/character-classes-in-regular-expressions.md)。  字符及字符串的比较和排序不受此更改影响，仍依赖于基础操作系统，或 Windows 7 系统、.NET Framework 提供的字符数据。

有关从 Unicode 6.0 到 Unicode 7.0 的字符类别的更改，请参阅 Unicode Consortium 网站上的 [Unicode 标准 7.0.0 版](https://www.unicode.org/versions/Unicode7.0.0/)。 有关从 Unicode 7.0 到 Unicode 8.0 的更改，请参阅 Unicode Consortium 网站上的 [Unicode 标准 8.0.0 版](https://www.unicode.org/versions/Unicode8.0.0/)。

<a name="Crypto462"></a>

### <a name="cryptography"></a>密码

**对包含 FIPS 186-3 DSA 的 X509 证书的支持**

.NET Framework 4.6.2 现已开始支持密钥超过 FIPS 186-2 1024 位限制的 DSA（数字签名算法）X509 证书。

除了支持更大的 FIPS 186-3 密钥大小之外，.NET Framework 4.6.2 还支持使用 SHA-2 系列的哈希算法（SHA256、SHA384 和 SHA512）计算签名。 FIPS 186-3 支持由新的 <xref:System.Security.Cryptography.DSACng?displayProperty=nameWithType> 类提供。

为了跟进 .NET Framework 4.6 中的 <xref:System.Security.Cryptography.RSA> 类和 .NET Framework 4.6.1 中的 <xref:System.Security.Cryptography.ECDsa> 类的最新更改，.NET Framework 4.6.2 中的 <xref:System.Security.Cryptography.DSA> 抽象基类有附加方法允许调用方在无需强制转换即可使用此功能。 可以调用 <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPrivateKey%2A?displayProperty=nameWithType> 扩展方法对数据进行签名，如以下示例所示。

```csharp
public static byte[] SignDataDsaSha384(byte[] data, X509Certificate2 cert)
{
    using (DSA dsa = cert.GetDSAPrivateKey())
    {
        return dsa.SignData(data, HashAlgorithmName.SHA384);
    }
}
```

```vb
Public Shared Function SignDataDsaSha384(data As Byte(), cert As X509Certificate2) As Byte()
    Using DSA As DSA = cert.GetDSAPrivateKey()
        Return DSA.SignData(data, HashAlgorithmName.SHA384)
    End Using
End Function
```

并且可以调用 <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPublicKey%2A?displayProperty=nameWithType> 扩展方法验证已签名的数据，如以下示例所示。

```csharp
public static bool VerifyDataDsaSha384(byte[] data, byte[] signature, X509Certificate2 cert)
{
    using (DSA dsa = cert.GetDSAPublicKey())
    {
        return dsa.VerifyData(data, signature, HashAlgorithmName.SHA384);
    }
}
```

```vb
 Public Shared Function VerifyDataDsaSha384(data As Byte(), signature As Byte(), cert As X509Certificate2) As Boolean
    Using dsa As DSA = cert.GetDSAPublicKey()
        Return dsa.VerifyData(data, signature, HashAlgorithmName.SHA384)
    End Using
End Function
```

**提高了 ECDiffieHellman 密钥派生例程的输入的清晰度**

.NET Framework 3.5 通过三个不同的密钥派生功能 (KDF) 例程增加了对椭圆曲线 Diffie-Hellman 密钥协议的支持。 例程的输入以及这些例程本身通过 <xref:System.Security.Cryptography.ECDiffieHellmanCng> 对象上的属性进行配置。 但由于不是每个例程都会读取每个输入属性，因此过去很有可能对开发人员造成了困扰。

为了在 .NET Framework 4.6.2 中解决这一问题，已向 <xref:System.Security.Cryptography.ECDiffieHellman> 基类添加以下三种方法，以便更明确地表示这些 KDF 例程及其输入：

|ECDiffieHellman 方法|描述|
|----------------------------|-----------------|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyFromHash%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Security.Cryptography.HashAlgorithmName%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|使用下面的公式派生密钥材料<br /><br /> HASH(secretPrepend &#124;&#124; *x* &#124;&#124; secretAppend)<br /><br /> HASH(secretPrepend OrElse *x* OrElse secretAppend)<br /><br /> 其中 *x* 是 EC Diffie-Hellman 算法的计算结果。|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyFromHmac%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Security.Cryptography.HashAlgorithmName%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|使用下面的公式派生密钥材料<br /><br /> HMAC(hmacKey, secretPrepend &#124;&#124; *x* &#124;&#124; secretAppend)<br /><br /> HMAC(hmacKey, secretPrepend OrElse *x* OrElse secretAppend)<br /><br /> 其中 *x* 是 EC Diffie-Hellman 算法的计算结果。|
|<xref:System.Security.Cryptography.ECDiffieHellman.DeriveKeyTls%28System.Security.Cryptography.ECDiffieHellmanPublicKey%2CSystem.Byte%5B%5D%2CSystem.Byte%5B%5D%29>|使用 TLS 伪随机函数 (PRF) 派生算法派生密钥材料。|

**对持久化密钥对称加密的支持**

Windows 加密库 (CNG) 现已开始支持存储持久化对称密钥和使用硬件存储的对称密钥。开发者可通过 .NET Framework 4.6.2 使用此功能。  因为密钥名和密钥提供程序的概念是特定于实现的，所以使用此功能要求使用具体实现类型（而不是首选出厂方法）的构造函数（例如，调用 `Aes.Create`）。

持久化密钥对称加密支持因 AES (<xref:System.Security.Cryptography.AesCng>) 和 3DES (<xref:System.Security.Cryptography.TripleDESCng>) 算法存在。 例如：

```csharp
public static byte[] EncryptDataWithPersistedKey(byte[] data, byte[] iv)
{
    using (Aes aes = new AesCng("AesDemoKey", CngProvider.MicrosoftSoftwareKeyStorageProvider))
    {
        aes.IV = iv;

        // Using the zero-argument overload is required to make use of the persisted key
        using (ICryptoTransform encryptor = aes.CreateEncryptor())
        {
            if (!encryptor.CanTransformMultipleBlocks)
            {
                throw new InvalidOperationException("This is a sample, this case wasn't handled...");
            }

            return encryptor.TransformFinalBlock(data, 0, data.Length);
        }
    }
}
```

```vb
Public Shared Function EncryptDataWithPersistedKey(data As Byte(), iv As Byte()) As Byte()
    Using Aes As Aes = New AesCng("AesDemoKey", CngProvider.MicrosoftSoftwareKeyStorageProvider)
        Aes.IV = iv

        ' Using the zero-argument overload Is required to make use of the persisted key
        Using encryptor As ICryptoTransform = Aes.CreateEncryptor()
            If Not encryptor.CanTransformMultipleBlocks Then
                Throw New InvalidOperationException("This is a sample, this case wasn't handled...")
            End If
            Return encryptor.TransformFinalBlock(data, 0, data.Length)
        End Using
    End Using
End Function
```

**对 SHA-2 哈希的 SignedXml 支持**

.NET Framework 4.6.2 中的 <xref:System.Security.Cryptography.Xml.SignedXml> 类现已开始支持 RSA-SHA256、RSA-SHA384 和 RSA-SHA512 PKCS#1 签名方法，还支持 SHA256、SHA384 和 SHA512 引用摘要算法。

URI 常量都在 <xref:System.Security.Cryptography.Xml.SignedXml> 上公开：

|SignedXml 字段|返回的常量|
|---------------------|--------------|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA256Url>|"http://www.w3.org/2001/04/xmlenc#sha256"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA256Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA384Url>|"http://www.w3.org/2001/04/xmldsig-more#sha384"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA384Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha384"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigSHA512Url>|"http://www.w3.org/2001/04/xmlenc#sha512"|
|<xref:System.Security.Cryptography.Xml.SignedXml.XmlDsigRSASHA512Url>|"http://www.w3.org/2001/04/xmldsig-more#rsa-sha512"|

 已将自定义 <xref:System.Security.Cryptography.SignatureDescription> 处理程序注册到 <xref:System.Security.Cryptography.CryptoConfig> 以添加对这些算法的支持的任何程序将会继续像过去一样工作，但由于现在有平台默认值，所以不再需要 <xref:System.Security.Cryptography.CryptoConfig> 注册。

<a name="SQLClient"></a>

### <a name="sqlclient"></a>SqlClient

SQL Server 的 .NET framework 数据提供程序 (<xref:System.Data.SqlClient?displayProperty=nameWithType>) 包括 .NET Framework 4.6.2 中的以下新功能：

**Azure SQL 数据库的连接池和超时**

启用连接池并出现超时或其他登录错误后，会缓存一个异常，并会在接下来的 5 秒到 1 分钟内尝试任何后续连接时引发缓存的异常。 有关详细信息，请参阅 [SQL Server 连接池 (ADO.NET)](../data/adonet/sql-server-connection-pooling.md)。

连接到 Azure SQL 数据库时此行为是不可取的，因为连接尝试可能会失败，出现通常会快速恢复的暂时性错误。 为更好地优化连接重试体验，会在与 Azure SQL 数据库连接失败时删除连接池阻塞期行为。

新 `PoolBlockingPeriod` 关键字的添加使你能够选择最适合你的应用的阻塞期。 值包括：

<xref:System.Data.SqlClient.PoolBlockingPeriod.Auto>

已禁用连接到 Azure SQL 数据库的应用程序的连接池阻塞期，已启用连接到任何其他 SQL Server 实例的应用程序的连接池阻塞期。 这是默认值。 如果 Server 终结点名称采用以下任一结尾，则将它们视为 Azure SQL 数据库：

- .database.windows.net

- .database.chinacloudapi.cn

- .database.usgovcloudapi.net

- .database.cloudapi.de

<xref:System.Data.SqlClient.PoolBlockingPeriod.AlwaysBlock>

连接池阻塞期始终处于启用状态。

<xref:System.Data.SqlClient.PoolBlockingPeriod.NeverBlock>

连接池阻塞期始终处于禁用状态。

**Always Encrypted 的增强功能**

SQLClient 引入了针对 Always Encrypted 的两个增强功能：

- 为改善针对加密数据库列的参数化查询的性能，现会缓存查询参数的加密元数据。 将 <xref:System.Data.SqlClient.SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled%2A?displayProperty=nameWithType> 属性设置为 `true`（这是默认值）时，如果多次调用相同的查询，则客户端只从服务器检索一次参数元数据。

- 密钥缓存中的列加密密钥条目现会在可配置时间间隔后被逐出，使用 <xref:System.Data.SqlClient.SqlConnection.ColumnEncryptionKeyCacheTtl%2A?displayProperty=nameWithType> 属性设置。

<a name="WCF"></a>

### <a name="windows-communication-foundation"></a>Windows Communication Foundation

在 .NET Framework 4.6.2 中，Windows Communication Foundation 在以下几个方面进行了增强：

**使用 CNG 对存储的证书的 WCF 传输安全支持**

WCF 传输安全使用 Windows 加密库 (CNG) 支持存储的证书。 在 .NET Framework 4.6.2 中，此支持仅限于将证书与指数长度不超过 32 位的公钥结合使用。 对于定位 .NET Framework 4.6.2 的应用程序，此功能默认启用。

对面向 .NET Framework 4.6.1 及更低版本，但在 .NET Framework 4.6.2 上运行的应用程序，可在 app.config 或 web.config 文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分中添加以下代码行来启用此功能。

```xml
<AppContextSwitchOverrides
    value="Switch.System.ServiceModel.DisableCngCertificates=false"
/>
```

这还可以使用代码以编程方式完成，如下所示：

```csharp
private const string DisableCngCertificates = @"Switch.System.ServiceModel.DisableCngCertificates";
AppContext.SetSwitch(disableCngCertificates, false);
```

```vb
Const DisableCngCertificates As String = "Switch.System.ServiceModel.DisableCngCertificates"
AppContext.SetSwitch(disableCngCertificates, False)
```

**通过 DataContractJsonSerializer 类更好地支持多个夏令时调整规则**

客户可以使用应用程序配置设置来确定 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 类是否支持一个时区的多个调整规则。 这是一项可以选择使用的功能。 若要启用它，将以下设置添加到 app.config 文件中：

```xml
<runtime>
     <AppContextSwitchOverrides value="Switch.System.Runtime.Serialization.DoNotUseTimeZoneInfo=false" />
</runtime>
```

启用此功能后，<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> 对象使用 <xref:System.TimeZoneInfo> 类型（而不是 <xref:System.TimeZone> 类型）来反序列化日期和时间数据。 <xref:System.TimeZoneInfo> 支持多个调整规则，这样就可以使用历史时区数据；<xref:System.TimeZone> 却不支持。

若要详细了解 <xref:System.TimeZoneInfo> 结构和时区调整，请参阅[时区概述](../../standard/datetime/time-zone-overview.md)。

**NetNamedPipeBinding 最佳匹配**

WCF 包含可以在客户端应用程序上设置以确保它们始终连接到服务的新应用设置，该服务在与它们请求的最匹配的 URI 上进行侦听。 将此应用设置设置为 `false`（默认值）时，客户端可以使用 <xref:System.ServiceModel.NetNamedPipeBinding> 尝试连接到服务，该服务在是所请求 URI 的子字符串的 URI 上进行侦听。

例如，一个客户端尝试连接到在 `net.pipe://localhost/Service1` 处进行侦听的服务，但该计算机上使用管理员特权运行的另一个服务在 `net.pipe://localhost` 处进行侦听。 将此应用设置设置为 `false` 时，客户端将尝试连接到错误的服务。 将应用设置设置为 `true` 后，客户端将始终连接到最匹配的服务。

> [!NOTE]
> 使用 <xref:System.ServiceModel.NetNamedPipeBinding> 的客户端基于服务的基址（如果存在）而不是完整终结点地址查找服务。 若要确保此设置始终有效，则服务应使用唯一基址。

若要启用此更改，将以下应用设置添加到客户端应用程序的 App.config 或 Web.config 文件中：

```xml
<configuration>
    <appSettings>
        <add key="wcf:useBestMatchNamedPipeUri" value="true" />
    </appSettings>
</configuration>
```

**SSL 3.0 不是默认协议**

结合使用 NetTcp 与传输安全和证书的凭据类型时，SSL 3.0 不再是用于协商安全连接的默认协议。 在大多数情况下，应该不会影响现有应用，因为 TLS 1.0 包含在 NetTcp 的协议列表中。 所有现有客户端应该能够至少使用 TLS 1.0 协商连接。 如果 Ssl3 必需，则使用以下配置机制之一将其添加到协商协议的列表。

- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols%2A?displayProperty=nameWithType> 属性

- <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A?displayProperty=nameWithType> 属性

- [\<netTcpBinding>](../configure-apps/file-schema/wcf/nettcpbinding.md) 部分的 [\<transport>](../configure-apps/file-schema/wcf/transport-of-nettcpbinding.md) 部分

- [\<customBinding>](../configure-apps/file-schema/wcf/custombinding.md) 部分的 [\<sslStreamSecurity>](../configure-apps/file-schema/wcf/sslstreamsecurity.md) 部分

<a name="WPF462"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

在 .NET Framework 4.6.2 中，Windows Presentation Foundation 在以下几个方面进行了增强：

**组排序**

使用 <xref:System.Windows.Data.CollectionView> 对象对数据进行分组的应用程序现在可以显式声明如何对组进行排序。 显式排序可解决在应用动态添加或删除组，或在它更改分组中包含的项属性的值时出现的非直观排序问题。 它还可通过将分组属性比较从完整集合排序移动到组排序来改善组创建过程的性能。

为支持组排序，新的 <xref:System.ComponentModel.GroupDescription.SortDescriptions%2A?displayProperty=nameWithType> 和 <xref:System.ComponentModel.GroupDescription.CustomSort%2A?displayProperty=nameWithType> 属性描述如何对 <xref:System.ComponentModel.GroupDescription> 对象生成的组的集合进行排序。 这类似于同名 <xref:System.Windows.Data.ListCollectionView> 属性描述如何对数据项进行排序的方式。

<xref:System.Windows.Data.PropertyGroupDescription> 类的两个新静态属性：<xref:System.Windows.Data.PropertyGroupDescription.CompareNameAscending%2A> 和 <xref:System.Windows.Data.PropertyGroupDescription.CompareNameDescending%2A>，可用于最常见的情况。

例如，下面的 XAML 按年龄分组数据，按升序对年龄组排序，并按姓氏分组每个年龄组内的项。

```xaml
<GroupDescriptions>
     <PropertyGroupDescription
         PropertyName="Age"
         CustomSort=
              "{x:Static PropertyGroupDescription.CompareNamesAscending}"/>
     </PropertyGroupDescription>
</GroupDescriptions>

<SortDescriptions>
     <SortDescription PropertyName="LastName"/>
</SortDescriptions>
```

**触控键盘支持**

在可采用文本输入的控件接收触摸输入时，通过自动调用和解除 Windows 10 中的触控键盘，可在 WPF 应用程序中启用焦点跟踪。

在 .NET framework 的早期版本中，WPF 应用程序不能在不禁用 WPF 笔/触摸手势支持的情况下选择加入焦点跟踪。 因此，WPF 应用程序必须选择完整的 WPF 触摸支持或依赖于 Windows 鼠标提升。

**按监视器 DPI**

为了支持最近激增的 WPF 应用程序高 DPI 和混合 DPI 环境，.NET Framework 4.6.2 中的 WPF 启用了按监视器 DPI 感知。 有关如何使 WPF 应用成为按监视器 DPI 感知的详细信息，请参阅 GitHub 上的[示例和开发人员指南](https://github.com/Microsoft/WPF-Samples/tree/master/PerMonitorDPI)。

在 .NET framework 的早期版本中，WPF 应用为系统 DPI 感知。 换而言之，应用程序的 UI 由操作系统相应地进行缩放，具体取决于在其上呈现应用的监视器的 DPI。

对于在 .NET Framework 4.6.2 下运行的应用程序，可在应用程序配置文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分中添加配置语句，在 WPF 应用程序中禁用按监视器 DPI 更改，如下所示：

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.Windows.DoNotScaleForDpiChanges=false"/>
</runtime>
```

<a name="WF462"></a>

### <a name="windows-workflow-foundation-wf"></a>Windows Workflow Foundation (WF)

在 .NET Framework 4.6.2 中，Windows Workflow Foundation 在以下几个方面进行了增强：

**在重新托管的 WF 设计器中支持 C# 表达式和 IntelliSense**

自 .NET Framework 4.5 起，WF 支持在 Visual Studio 设计器和代码工作流中使用 C# 表达式。 重新托管的工作流设计器是 WF 的一项重要功能，允许工作流设计器位于 Visual Studio 外部的应用程序中（如 WPF 中）。  Windows Workflow Foundation 提供在重新托管的工作流设计器中支持 C# 表达式和 IntelliSense 的功能。 有关详细信息，请参阅 [Windows Workflow Foundation 博客](/archive/blogs/workflowteam/building-c-expressions-support-and-intellisense-in-the-rehosted-workflow-designer)。

`Availability of IntelliSense when a customer rebuilds a workflow project from Visual Studio` 在低于 4.6.2 的 .NET Framework 版本中，客户从 Visual Studio 重新生成工作流项目时，WF 设计器 IntelliSense 会中断。 虽然项目生成成功，但在设计器中找不到该工作流类型，并且来自 IntelliSense 的缺少工作流类型的警告会出现在 **错误列表** 窗口中。 .NET Framework 4.6.2 解决了这个问题，并让 IntelliSense 可供使用。

**启用了工作流跟踪的工作流 V1 应用程序现以 FIPS 模式运行**

已启用 FIPS 兼容模式的计算机现在可以成功运行启用了工作流跟踪的工作流版本 1 样式的应用程序。 若要启用此方案，必须对 app.config 文件进行以下更改：

```xml
<add key="microsoft:WorkflowRuntime:FIPSRequired" value="true" />
```

如果未启用此方案，运行应用程序将继续生成异常，并显示消息“此实现不是 Windows 平台 FIPS 验证的加密算法的一部分”。

**结合使用动态更新和 Visual Studio 工作流设计器时的工作流改进**

工作流设计器、流程图活动设计器和其他工作流活动设计器现在已成功加载并显示调用 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType> 方法后已保存的工作流。 在 .NET Framework 4.6.2 之前的 .NET Framework 版本中，在 Visual Studio 中为调用 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType> 后保存的工作流加载 XAML 文件可能会导致以下问题：

- 工作流设计器无法正确加载 XAML 文件（当 <xref:System.Activities.Presentation.ViewState.ViewStateData.Id%2A?displayProperty=nameWithType> 位于行末尾处时）。

- 流程图活动设计器或其他工作流活动设计器可能在其默认位置显示所有对象，与附加的属性值相反。

<a name="clickonce-1"></a>

### <a name="clickonce"></a>ClickOnce

除现已支持的 1.0 协议以外，ClickOnce 已更新为还支持 TLS 1.1 和 TLS 1.2。 ClickOnce 会自动检测哪种协议必需；启用 TLS 1.1 和 1.2 支持无需 ClickOnce 应用程序中的任何额外步骤。

<a name="UWPConvert"></a>

### <a name="converting-windows-forms-and-wpf-apps-to--uwp-apps"></a>将 Windows 窗体和 WPF 应用转换为 UWP 应用

Windows 现在提供将现有 Windows 桌面应用（包括 WPF 和 Windows 窗体应用）引入通用 Windows 平台 (UWP) 的功能。 这项技术充当的是一座桥梁，使你能够逐渐将现有代码库迁移到 UWP，从而将你的应用引入所有 Windows 10 设备。

转换后的桌面应用会获得应用标识，类似于 UWP 应用的应用标识，该标识使 UWP API 可访问以启用如动态磁贴和通知等功能。 应用的行为将继续像以前一样，并作为完全信任应用运行。 应用转换之后，可将应用容器进程添加到现有的完全信任进程，以添加自适应用户界面。 将所有功能移动到应用容器进程后，可以删除完全信任进程，并且可对所有 Windows 10 设备提供新的 UWP 应用。

<a name="Debug462"></a>

### <a name="debugging-improvements"></a>调试改进

非托管调试 API 在 .NET Framework 4.6.2 中得到了增强，可在引发 <xref:System.NullReferenceException> 时执行附加分析，让你能够确定单行源代码中哪个变量是 `null`。   为支持此方案，已将以下 API 添加到非托管调试 API。

- [ICorDebugCode4](../unmanaged-api/debugging/icordebugcode4-interface.md)、[ICorDebugVariableHome](../unmanaged-api/debugging/icordebugvariablehome-interface.md) 和 [ICorDebugVariableHomeEnum](../unmanaged-api/debugging/icordebugvariablehomeenum-interface.md) 接口，它们公开托管变量的本机位置。 这使调试器能够在 <xref:System.NullReferenceException> 发生时执行某些代码流分析并逆向工作，以确定对应于为 `null` 的本机位置的托管变量。

- [ICorDebugType2::GetTypeID](../unmanaged-api/debugging/icordebugtype2-gettypeid-method.md) 方法提供 ICorDebugType 到 [COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md) 的映射，这使调试器能够获取 [COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md)，而无需 ICorDebugType 的实例。 然后，[COR_TYPEID](../unmanaged-api/debugging/cor-typeid-structure.md) 上现有的 API 可用于确定该类型的类布局。

<a name="v461"></a>

## <a name="whats-new-in-net-framework-461"></a>.NET Framework 4.6.1 中的新增功能

.NET Framework 4.6.1 在以下几个领域新增了功能：

- [加密](#Crypto)

- [ADO.NET](#ADO.NET461)

- [Windows Presentation Foundation (WPF)](#WPF461)

- [Windows Workflow Foundation](#WWF461)

- [分析](#Profile461)

- [NGen](#NGEN461)

若要详细了解 .NET Framework 4.6.1，请参阅以下主题：

- [.NET Framework 4.6.1 更改列表](https://github.com/Microsoft/dotnet/blob/master/releases/net461/dotnet461-changes.md)

- [4.6.1 中的应用程序兼容性](../migration-guide/application-compatibility.md)

- [.NET Framework API 差异](https://github.com/Microsoft/dotnet/blob/master/releases/net461/dotnet461-api-changes.md)（在 GitHub 上）

<a name="Crypto"></a>

### <a name="cryptography-support-for-x509-certificates-containing-ecdsa"></a>加密：支持包含 ECDSA 在内的 X509 证书

.NET Framework 4.6 添加了针对 X509 证书的 RSACng 支持。 .NET Framework 4.6.1 现已开始支持 ECDSA（椭圆曲线数字签名算法）X509 证书。

ECDSA 可提供更好的性能，是一种比 RSA 更安全的加密算法，从而可在传输层安全性 (TLS) 性能和可伸缩性十分重要的情况下提供极佳选择。 .NET Framework 实现可将调用包装到现有 Windows 功能中。

下面的示例代码展示了如何利用 .NET Framework 4.6.1 中新增的 ECDSA X509 证书支持，轻松生成字节流的签名。

[!code-csharp[whatsnew.461.crypto#1](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.461.crypto/cs/Code46.cs#1)]
[!code-vb[whatsnew.461.crypto#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.461.crypto/vb/Code461.vb#1)]

这与在 .NET Framework 4.6 中生成签名所需的代码形成了鲜明对比。

[!code-csharp[whatsnew.461.crypto#2](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.461.crypto/cs/Code46.cs#2)]
[!code-vb[whatsnew.461.crypto#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.461.crypto/vb/Code46.vb#2)]

<a name="ADO.NET461"></a>

### <a name="adonet"></a>ADO.NET

以下内容已添加到 ADO.NET 中：

**针对硬件保护密钥的 Always Encrypted 支持**

ADO.NET 现在支持以本机方式在硬件安全模块 (HSM) 中存储始终加密列主密钥。 借助此支持，客户可以利用存储在 HSM 中的非对称密钥，而不必编写自定义列主密钥存储提供程序并在应用程序中注册它们。

客户需要在应用服务器或客户端计算机上安装 HSM 供应商提供的 CSP 提供程序或 CNG 密钥存储提供程序，才能访问使用存储在 HSM 中的列主密钥保护的始终加密数据。

**改进了 AlwaysOn 的 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A> 连接行为**

SqlClient 现在可自动提供与 AlwaysOn 可用性组 (AG) 之间的更快连接。 它以透明方式检测应用程序是否连接到不同子网上的 AlwaysOn 可用性组 (AG)，快速发现当前的活动服务器并提供与服务器之间的连接。 在此版本之前，应用程序必须将连接字符串设置为包括 `"MultisubnetFailover=true"`，以指示它已连接到 AlwaysOn 可用性组。 如果未将连接关键字设置为 `true`，则应用程序可能会在连接到 AlwaysOn 可用性组时遇到超时。 使用此版本时，应用程序 *无需* 再将 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A> 设置为 `true`。 有关对 Always On 可用性组的 SqlClient 支持的详细信息，请参阅[对高可用性、灾难恢复的 SqlClient 支持](../data/adonet/sql/sqlclient-support-for-high-availability-disaster-recovery.md)。

<a name="WPF461"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

Windows Presentation Foundation 包括一些改进和更改。

**提升了性能**

在 .NET Framework 4.6.1 中，触控事件的触发延迟问题得到了解决。 此外在快速输入过程中，在 <xref:System.Windows.Controls.RichTextBox> 控件中输入不再占用呈现线程。

**拼写检查改进**

WPF 中的拼写检查器在 Windows 8.1 和更高版本上进行了更新，可利用操作系统支持对其他语言进行拼写检查。  在 Windows 8.1 之前的 Windows 版本上，功能方面没有更改。

与以前版本的 .NET Framework 一样，可通过按以下顺序查找信息来检测 <xref:System.Windows.Controls.TextBox> 控件或 <xref:System.Windows.Controls.RichTextBox> 块的语言：

- `xml:lang`（如果存在）。

- 当前输入语言。

- 当前线程区域性。

有关 WPF 中语言支持的详细信息，请参阅[有关 .NET Framework 4.6.1 功能的 WPF 博客文章](https://devblogs.microsoft.com/wpf/wpf-in-net-4-6-1/)。

**针对每用户自定义词典的附加支持**

在 .NET Framework 4.6.1 中，WPF 可识别全局注册的自定义字典。 除了能够针对每个控件注册它们，还提供了此功能。

在以前版本的 WPF 中，自定义词典无法识别已排除的单词和自动更正列表。 在 Windows 8.1 和 Windows 10 上，通过使用可以置于 `%AppData%\Microsoft\Spelling\<language tag>` 目录下的文件来支持它们。  以下规则适用于这些文件：

- 这些文件应具有扩展名 .dic（用于已添加的单词）、.exc（用于已排除的单词）或 .acl（用于自动更正）。

- 这些文件应是以字节顺序标记 (BOM) 开头的 UTF-16 LE 纯文本。

- 每行应包含一个单词（位于已添加和已排除的单词列表中），或是其中用竖线 ("&#124;") 分隔单词的自动更正对（位于自动更正单词列表中）。

- 这些文件被视为只读，不会由系统进行修改。

> [!NOTE]
> WPF 拼写检查 API 不直接支持这些新文件格式，在应用程序中向 WPF 提供自定义词典应继续使用 .lex 文件。

**示例**

[Microsoft/WPF 示例](https://github.com/Microsoft/WPF-Samples) GitHub 存储库中具有大量的 WPF 示例。 可通过向我们发送拉取请求或建立 [GitHub 问题](https://github.com/Microsoft/WPF-Samples/issues)来帮助我们改进示例。

**DirectX 扩展**

WPF 包括一个 [NuGet 包](https://www.nuget.org/packages/Microsoft.Wpf.Interop.DirectX-x86/)，它提供 <xref:System.Windows.Interop.D3DImage> 的新实现，从而使你可以轻松地与 DX10 和 Dx11 内容进行互操作。 此包的代码已开放源代码，在 [GitHub 上](https://github.com/Microsoft/WPFDXInterop)提供。

<a name="WWF461"></a>

### <a name="windows-workflow-foundation-transactions"></a>Windows Workflow Foundation：事务

<xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A?displayProperty=nameWithType> 方法现在可以使用 MSDTC 以外的分布式事务管理器来提升事务。 可通过将 GUID 事务提升程序标识符指定为新的 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%28System.Transactions.IPromotableSinglePhaseNotification%2CSystem.Guid%29?displayProperty=nameWithType> 重载来实现此目的。 如果此操作成功，则会对事务的功能施加一些限制。 非 MSDTC 事务提升程序登记之后，以下方法会引发 <xref:System.Transactions.TransactionPromotionException>，因为这些方法需要提升到 MSDTC：

- <xref:System.Transactions.Transaction.EnlistDurable%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetDtcTransaction%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetExportCookie%2A?displayProperty=nameWithType>

- <xref:System.Transactions.TransactionInterop.GetTransmitterPropagationToken%2A?displayProperty=nameWithType>

非 MSDTC 事务提升程序登记之后，它必须使用它定义的协议来用于将来的持久登记。 事务提升程序的 <xref:System.Guid> 可以使用 <xref:System.Transactions.Transaction.PromoterType%2A> 属性来获取。 当事务提升时，事务提升程序会提供表示提升令牌的 <xref:System.Byte> 数组。 应用程序可以使用 <xref:System.Transactions.Transaction.GetPromotedToken%2A> 方法获取非 MSDTC 提升事务的提升令牌。

新 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%28System.Transactions.IPromotableSinglePhaseNotification%2CSystem.Guid%29?displayProperty=nameWithType> 重载的用户必须遵循特定调用序列，才能使提升操作成功完成。 这些规则记录在该方法的文档中。

<a name="Profile461"></a>

### <a name="profiling"></a>分析

非托管分析 API 在以下方面得到了增强：

- 更好地支持在 [ICorProfilerInfo7](../unmanaged-api/profiling/icorprofilerinfo7-interface.md) 接口中访问 PDB。

  在 ASP.NET Core 中，由 Roslyn 在内存中编译程序集正变得更加常见。 对于创建分析工具的开发人员而言，这意味着过去在磁盘上进行序列化的 PDB 可能会不再存在。 对于代码覆盖率或逐行性能分析这类任务，分析器工具通常使用 PDB 将代码映射回源行。 [ICorProfilerInfo7](../unmanaged-api/profiling/icorprofilerinfo7-interface.md) 接口现在包含两种新方法（[ICorProfilerInfo7::GetInMemorySymbolsLength](../unmanaged-api/profiling/icorprofilerinfo7-getinmemorysymbolslength-method.md) 和 [ICorProfilerInfo7::ReadInMemorySymbols](../unmanaged-api/profiling/icorprofilerinfo7-readinmemorysymbols.md)），可为这些探查器工具提供对内存中 PDB 数据的访问。通过使用新 API，探查器可以采用字节数组形式获取内存中 PDB 的内容，然后对它进行处理或将它序列化到磁盘。

- 使用 ICorProfiler 接口可更好地检测。

  使用 `ICorProfiler` API 的 ReJit 功能进行动态检测的分析器现在可以修改某些元数据。 以前这类工具可以随时检测 IL，但只能在模块加载时修改元数据。 因为 IL 引用元数据，所以这会限制可以进行的检测的种类。 我们通过添加 [ICorProfilerInfo7::ApplyMetaData](../unmanaged-api/profiling/icorprofilerinfo7-applymetadata-method.md) 方法来支持在模块加载之后编辑元数据的子集（特别是通过添加新的 `AssemblyRef`、`TypeRef`、`TypeSpec`、`MemberRef`、`MemberSpec` 和 `UserString` 记录），解除了其中一些限制。 通过此更改可以进行范围广得多的动态检测。

<a name="NGEN461"></a>

### <a name="native-image-generator-ngen-pdbs"></a>本机映像生成器 (NGEN) PDB

跨计算机事件跟踪允许客户在计算机 A 上分析一个程序，并在计算机 B 上查看具有源行映射的分析数据。通过使用以前版本的 .NET Framework，用户会将所有模块和本机映像从分析计算机复制到包含 IL PDB 的分析计算机来创建源到本机映射。 虽然此过程在文件相对较小（如用于手机应用程序）时可能工作良好，但是文件在桌面系统上可能非常大，需要很长时间来进行复制。

借助 Ngen PDB，NGen 可以创建包含 IL 到本机映射的 PDB，而无需依赖于 IL PDB。 在我们的跨计算机事件跟踪方案中，只需将计算机 A 生成的本机映像 PDB 复制到计算机 B，并使用[调试接口访问 API](/visualstudio/debugger/debug-interface-access/debug-interface-access-sdk-reference) 读取 IL PDB 的源到 IL 映射和本机映像 PDB 的 IL 到本机映射。 组合这两个映射可提供源到本机映射。 由于本机映像 PDB 远小于所有模块和本机映像，因此从计算机 A 复制到计算机 B 的过程要快得多。

<a name="v46"></a>

## <a name="whats-new-in-net-2015"></a>.NET 2015 的新增功能

.NET 2015 引入了 .NET Framework 4.6 和 .NET Core。 一些新功能两者都适用，而另一些功能则是 .NET Framework 4.6 或 .NET Core 的专属功能。

- **ASP.NET Core**

  .NET 2015 包括 ASP.NET Core，是一个用于生成基于云的新式应用的精益 .NET 实现。 ASP.NET Core 是模块化的，因此你可以仅包括应用程序所需的那些功能。 其可承载于 IIS 上或自承载于自定义过程中，并且你可以在同一服务器上运行具有不同版本 .NET Framework 的应用。 它包括为云部署而设计的新环境配置系统。

  MVC、Web API 和 Web Pages 统一至称为 MVC 6 的单个 Framework。 通过 Visual Studio 2015 或更高版本中的新工具生成 ASP.NET Core 应用。 现有应用程序将在新 .NET Framework 上工作；但是，若要生成使用 MVC 6 或 SignalR 3 的应用，则必须使用 Visual Studio 2015 或更高版本中的项目系统。

  有关信息，请参阅 [ASP.NET Core](/aspnet/core/)。

- **ASP.NET 更新**

  - **基于任务的 API，用于异步响应刷新**

    ASP.NET 现在提供一个基于任务的简单 API <xref:System.Web.HttpResponse.FlushAsync%2A?displayProperty=nameWithType> 用于异步响应刷新，它允许通过使用你的语言的 `async/await` 支持来异步刷新响应。

  - **模型绑定支持 Task 返回方法**

    在 .NET Framework 4.5 中，ASP.NET 增加了模型绑定功能，启用了一种以代码为中心的可扩展方法，用于在 Web 窗体页面和用户控件中执行基于 CRUD 的数据操作。 模型绑定系统现在支持 <xref:System.Threading.Tasks.Task>-returning 模型绑定方法。 此功能使得 Web 窗体开发人员在使用较新版本的 ORM（包括实体框架）时，能获得异步的可伸缩性优点以及数据绑定系统的易用性。

    异步模型绑定由 `aspnet:EnableAsyncModelBinding` 配置设置控制。

    ```xml
    <appSettings>
        <add key=" aspnet:EnableAsyncModelBinding" value="true|false" />
    </appSettings>
    ```

    在定位 .NET Framework 4.6 的应用程序中，默认值为 `true`。 在定位旧版 .NET Framework 但在 .NET Framework 4.6 上运行的应用程序中，默认值为 `false`。 可以通过将配置设置设置为 `true` 来启用它。

  - **HTTP/2 支持 (Windows 10)**

    [HTTP/2](https://www.wikipedia.org/wiki/HTTP/2) 是新版的 HTTP 协议，提供更好的连接利用率（客户端和服务器之间的往返更少），从而减少为用户加载网页的延迟。  网页（而不是服务）从 HTTP/2 中获益最多，因为该协议优化多个作为单个体验的一部分进行请求的项目。 已向 .NET Framework 4.6 中的 ASP.NET 添加了 HTTP/2 支持。 因为网络功能存在于多个层，所以 Windows、IIS 和 ASP.NET 中均需要新功能以启用 HTTP/2。 必须在 Windows 10 上运行，以便将 HTTP/2 与 ASP.NET 搭配使用。

    HTTP/2 也受到支持，默认情况下在使用 <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> API 的 Windows 10 通用 Windows 平台 (UWP) 上使用。

    为了提供一种方法来使用 ASP.NET 应用程序中的 [PUSH_PROMISE](https://httpwg.github.io/http2-spec/#PUSH_PROMISE) 功能，已向 <xref:System.Web.HttpResponse> 类添加了一种具有两个重载（<xref:System.Web.HttpResponse.PushPromise%28System.String%29> 和 <xref:System.Web.HttpResponse.PushPromise%28System.String%2CSystem.String%2CSystem.Collections.Specialized.NameValueCollection%29>）的新方法。

    > [!NOTE]
    > 尽管 ASP.NET Core 支持 HTTP/2，不过尚未添加针对 PUSH PROMISE 功能的支持。

    浏览器和 Web 服务器（Windows 上的 IIS）执行所有工作。 无需为用户执行任何繁重任务。

    大多数[主要浏览器都支持 HTTP/2](https://www.wikipedia.org/wiki/HTTP/2)，因此很可能你的用户将从 HTTP/2 支持中受益（如果你的服务器支持它）。

  - **对令牌绑定协议的支持**

    Microsoft 和 Google 一直在针对身份验证的新方法进行合作，这种方法称为[令牌绑定协议](https://github.com/TokenBinding/Internet-Drafts)。 前提是罪犯可以盗取并使用身份验证令牌（在你的浏览器缓存中）以访问本应安全的资源（例如你的银行帐户），而无需知道密码或任何其他特权。 新协议旨在缓解此问题。

    令牌绑定协议将在 Windows 10 中作为浏览器功能实现。 ASP.NET 应用将参与该协议，以使身份验证令牌验证为合法。 客户端和服务器实现建立由协议指定的端到端的保护。

  - **随机字符串哈希算法**

    .NET Framework 4.5 引入了[随机字符串哈希算法](../configure-apps/file-schema/runtime/userandomizedstringhashalgorithm-element.md)。 但是，由于某些 ASP.NET 功能依赖于稳定的哈希代码，因此 ASP.NET 不支持该算法。 在 .NET Framework 4.6 中，现在支持随机字符串哈希算法。 若要启用此功能，请使用 `aspnet:UseRandomizedStringHashAlgorithm` 配置设置。

    ```xml
    <appSettings>
        <add key="aspnet:UseRandomizedStringHashAlgorithm" value="true|false" />
    </appSettings>
    ```

- **ADO.NET**

  ADO.NET 现在支持 SQL Server 2016 社区技术预览版 2 (CTP2) 中提供的 Always Encrypted 功能。 借助 Always Encrypted，SQL Server 可对加密数据执行操作，并且最重要的是，加密密钥与应用程序一起驻留在客户的受信任环境内，而不是驻留在服务器上。 Always Encrypted 可确保客户数据的安全，因此 DBA 没有纯文本数据的访问权限。 数据的加密和解密都在驱动程序级别以透明方式执行，从而将现有应用程序必须做出的更改减至最少。 有关详细信息，请参阅 [Always Encrypted（数据库引擎）](/sql/relational-databases/security/encryption/always-encrypted-database-engine)和 [Always Encrypted（客户端开发）](/sql/relational-databases/security/encryption/always-encrypted-client-development)。

- **托管代码的 64 位 JIT 编译器**

  .NET Framework 4.6 采用新版 64 位 JIT 编译器（最初代码名为 RyuJIT）。 新的 64 位编译器相较旧的 64 位 JIT 编译器具有显著的性能提升。 新的 64 位编译器针对 .NET Framework 4.6 上运行的 64 位进程而启用。 如果你的应用被编译为 64 位或 AnyCPU 并在 64 位操作系统上运行，则它将在 64 位进程中运行。 虽然已采取谨慎的措施来使到新编译器的转换尽可能透明，但行为也可能发生变化。

  新的 64 位 JIT 编译器还包括硬件 SIMD 加速功能，结合 <xref:System.Numerics> 命名空间中支持 SIMD 的类型使用时，可以获得良好的性能提升。

- **程序集加载程序改进**

  通过在加载相应的 NGEN 映像后卸载 IL 程序集，程序集加载程序现在能更有效地利用内存。 此更改会降低虚拟内存（这对诸如 Visual Studio 等的大型 32 位应用特别有益），还会节省物理内存。

- **基类库更改**

  为了启用关键方案，已向 .NET Framework 4.6 添加了许多新 API。 这些包括以下更改和添加：

  - **IReadOnlyCollection\<T> 实现**

    其他集合实现 <xref:System.Collections.Generic.IReadOnlyCollection%601>，例如 <xref:System.Collections.Generic.Queue%601> 和 <xref:System.Collections.Generic.Stack%601>。

  - **CultureInfo.CurrentCulture 和 CultureInfo.CurrentUICulture**

    <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> 和 <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 属性现在是读写而不是只读。 如果你向这些属性分配一个新 <xref:System.Globalization.CultureInfo> 对象，则由 `Thread.CurrentThread.CurrentCulture` 属性定义的当前线程区域性和由 `Thread.CurrentThread.CurrentUICulture` 属性定义在当前 UI 线程区域性也会更改。

  - **垃圾回收 (GC) 增强功能**

    <xref:System.GC> 类现在包括允许你在执行关键路径期间禁止垃圾回收的 <xref:System.GC.TryStartNoGCRegion%2A> 和 <xref:System.GC.EndNoGCRegion%2A> 方法。

    <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%2CSystem.Boolean%29?displayProperty=nameWithType> 方法的新重载允许你控制小型对象堆和大型对象堆是否均扫频和压缩或仅扫频。

  - **启用了 SIMD 的类型**

    <xref:System.Numerics> 命名空间现在包括许多支持 SIMD 的类型，如 <xref:System.Numerics.Matrix3x2>、<xref:System.Numerics.Matrix4x4>、<xref:System.Numerics.Plane>、<xref:System.Numerics.Quaternion>、<xref:System.Numerics.Vector2>、<xref:System.Numerics.Vector3> 和 <xref:System.Numerics.Vector4>。

    由于新的 64 位 JIT 编译器还包括硬件 SIMD 加速功能，将支持 SIMD 的类型与新的 64 位 JIT 编译器一起使用时，会带来特别显著的性能提升。

  - **加密更新**

    <xref:System.Security.Cryptography?displayProperty=nameWithType> API 更新为支持 [Windows CNG 加密 API](/windows/desktop/SecCNG/cng-reference)。 以前版本的 .NET Framework 完全依赖于[早期版本的 Windows 加密 API](/windows/desktop/SecCrypto/cryptography-portal)，以用作 <xref:System.Security.Cryptography?displayProperty=nameWithType> 实现的基础。 我们已经请求支持 CNG API，因为它支持[现代加密算法](/windows/desktop/SecCNG/cng-features#suite-b-support)，这对某些类别的应用十分重要。

    .NET Framework 4.6 包括以下新的增强功能以支持 Windows CNG 加密 API：

    - X509 证书（`System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)` 和 `System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)`）的一组扩展方法，如果可能，它们将返回基于 CNG 的实现，而不返回基于 CAPI 的实现。 （一些智能卡等仍需要 CAPI，并由 API 处理回退）。

    - <xref:System.Security.Cryptography.RSACng?displayProperty=nameWithType> 类，该类提供 RSA 算法的 CNG 实现。

    - RSA API 的增强功能，常见操作不再需要转换。 例如，使用 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 对象加密数据需要类似以前版本的 .NET Framework 中的以下代码。

      [!code-csharp[WhatsNew.Casting#1](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.casting/cs/program.cs#1)]
      [!code-vb[WhatsNew.Casting#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.casting/vb/module1.vb#1)]

      可采用如下方式重写在 .NET Framework 4.6 中使用新加密 API 的代码以避免转换。

      [!code-csharp[WhatsNew.Casting#2](~/samples/snippets/csharp/VS_Snippets_CLR/whatsnew.casting/cs/program.cs#2)]
      [!code-vb[WhatsNew.Casting#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/whatsnew.casting/vb/module1.vb#2)]

  - **支持将日期和时间与 UNIX 时间相互转换**

    以下新方法已添加到 <xref:System.DateTimeOffset> 结构，以支持将日期和时间转换为 UNIX 时间或将 UNIX 时间转换为日期和时间：

    - <xref:System.DateTimeOffset.FromUnixTimeSeconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.FromUnixTimeMilliseconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.ToUnixTimeSeconds%2A?displayProperty=nameWithType>

    - <xref:System.DateTimeOffset.ToUnixTimeMilliseconds%2A?displayProperty=nameWithType>

  - **兼容性开关**

    <xref:System.AppContext> 类添加一个新的兼容性功能，使库编写器可为其用户提供统一的新功能选择退出机制。 它在组件之间建立松耦合的协定，以便与选择退出请求进行通信。 对现有功能进行更改时，此功能通常很重要。 相反，已有新功能隐式选择加入。

    使用 <xref:System.AppContext>，库定义并公开兼容性开关，而依赖于这些开关的代码可以设置这些开关以影响库行为。 默认情况下，库提供新功能；如果设置了开关，则只更改新功能（即，它们提供以前的功能）。

    应用程序（或库）可以声明相关库定义的开关的值（始终是 <xref:System.Boolean> 值）。 该开关始终隐式 `false`。 将此开关设置为 `true` 将启用它。 将此开关显式设置为 `false` 将提供新行为。

    ```csharp
    AppContext.SetSwitch("Switch.AmazingLib.ThrowOnException", true);
    ```

    ```vb
    AppContext.SetSwitch("Switch.AmazingLib.ThrowOnException", True)
    ```

    库必须检查使用者是否已声明该开关的值，并且相应地作用于它。

    ```csharp
    if (!AppContext.TryGetSwitch("Switch.AmazingLib.ThrowOnException", out shouldThrow))
    {
        // This is the case where the switch value was not set by the application.
        // The library can choose to get the value of shouldThrow by other means.
        // If no overrides nor default values are specified, the value should be 'false'.
        // A false value implies the latest behavior.
    }

    // The library can use the value of shouldThrow to throw exceptions or not.
    if (shouldThrow)
    {
        // old code
    }
    else
    {
        // new code
    }
    ```

    ```vb
    If Not AppContext.TryGetSwitch("Switch.AmazingLib.ThrowOnException", shouldThrow) Then
        ' This is the case where the switch value was not set by the application.
        ' The library can choose to get the value of shouldThrow by other means.
        ' If no overrides nor default values are specified, the value should be 'false'.
        ' A false value implies the latest behavior.
    End If

    ' The library can use the value of shouldThrow to throw exceptions or not.
    If shouldThrow Then
        ' old code
    Else
        ' new code
    End If
    ```

    使用一致的开关格式是有益的，因为它们是由库公开的正式协定。 以下是两种明显的格式。

    - *Switch*.*namespace*.*switchname*

    - *Switch*.*library*.*switchname*

  - **更改为基于任务的异步模式 (TAP)**

    对于面向 .NET Framework 4.6、<xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 对象的应用，请继承调用线程的区域性和 UI 区域性。 面向早期 .NET Framework 版本或不面向特定版本的 .NET Framework 的应用的行为不受影响。 有关详细信息，请参阅 <xref:System.Globalization.CultureInfo> 类主题中的“区域性和基于任务的异步操作”一节。

    <xref:System.Threading.AsyncLocal%601?displayProperty=nameWithType> 类允许你表示对于给定异步控制流（如 `async` 方法）来说是本地数据的环境数据。 它可用于跨线程保存数据。 你还可以定义一个回调方法，该回调方法在环境数据发生变化时就会发出通知，而不论环境数据发生变化的原因是 <xref:System.Threading.AsyncLocal%601.Value%2A?displayProperty=nameWithType> 属性显式更改还是线程遇到上下文转换。

    三种便利方法（<xref:System.Threading.Tasks.Task.CompletedTask%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Task.FromCanceled%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Task.FromException%2A?displayProperty=nameWithType>）已添加到基于任务的异步模式 (TAP)，以返回处于特定状态的已完成任务。

    <xref:System.IO.Pipes.NamedPipeClientStream> 类现在支持与其新的 <xref:System.IO.Pipes.NamedPipeClientStream.ConnectAsync%2A> 进行异步通信。 方法。

  - **EventSource 现在支持写入事件日志**

    除了在计算机上创建的任何现有 ETW 会话外，现在你还可以使用 <xref:System.Diagnostics.Tracing.EventSource> 类将管理或操作消息记录到事件日志中。 在过去，你必须使用 Microsoft.Diagnostics.Tracing.EventSource NuGet 包才能实现此功能。 此功能现在内置于 .NET Framework 4.6 中。

    NuGet 包和 .NET Framework 4.6 都更新了以下功能：

    - **动态事件**

      允许在不创建事件方法的情况下“在运行过程中”定义事件。

    - **丰富的负载**

      允许将专门特性化的类和数组以及基元类型作为负载传递

    - **活动跟踪**

      使“开始”和“停止”事件用 ID 标记在它们之间发生的事件，以表示当前处于活动状态的所有活动。

    为了支持这些功能，已将重载的 <xref:System.Diagnostics.Tracing.EventSource.Write%2A> 方法添加到了 <xref:System.Diagnostics.Tracing.EventSource> 类。

- **Windows Presentation Foundation (WPF)**

  - **HDPI 改进**

    在 .NET Framework 4.6 中，WPF 现提供更出色的 HDPI 支持。 已对布局舍入进行了更改，以减少带边框的控件中的剪切实例。 默认情况下，仅当 <xref:System.Runtime.Versioning.TargetFrameworkAttribute> 设置为 .NET Framework 4.6 时，才启用此功能。  对于面向旧版 Framework，但在 .NET Framework 4.6 上运行的应用程序，可通过在 app.config 文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分中添加下面的代码行，选择启用新行为：

    ```xml
    <AppContextSwitchOverrides
    value="Switch.MS.Internal.DoNotApplyLayoutRoundingToMarginsAndBorderThickness=false"
    />
    ```

    跨越具有不同 DPI 设置（多 DPI 设置）的多个监视器的 WPF 窗口现在完全呈现，且没有涂黑区域。 可以通过将下面的行添加到 app.config 文件的 `<appSettings>` 部分来选择退出此行为，以禁用此新行为：

    ```xml
    <add key="EnableMultiMonitorDisplayClipping" value="true"/>
    ```

    已向 <xref:System.Windows.Input.Cursor?displayProperty=nameWithType> 添加了对基于 DPI 设置自动加载右侧光标的支持。

  - **触摸更好**

    在 .NET Framework 4.6 中，客户在 [Connect](https://connect.microsoft.com/VisualStudio/feedback/details/903760/) 中报告的触控服务导致不可预测行为发生的问题得到了解决。 Windows 应用商店应用程序和 WPF 应用程序的双击阈值现在与 Windows 8.1 及更高版本中的相同。

  - **透明子窗口支持**

    .NET Framework 4.6 中的 WPF 支持在 Windows 8.1 及更高版本中使用透明子窗口。 这使得你可以在顶层窗口中创建非矩形的透明子窗口。 你可以通过将 <xref:System.Windows.Interop.HwndSourceParameters.UsesPerPixelTransparency%2A?displayProperty=nameWithType> 属性设置为 `true` 启用此功能。

- **Windows Communication Foundation (WCF)**

  - **SSL 支持**

    将 NetTcp 用于传输安全和客户端身份验证时，除了 SSL 3.0 和 TLS 1.0，WCF 现在还支持 SSL 版本 TLS 1.1 和 TLS 1.2。 现在可选择要使用的协议，或禁用旧的次要安全协议。 可以通过设置 <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols%2A> 属性或通过将以下内容添加到配置文件来完成此操作。

    ```xml
    <netTcpBinding>
        <binding>
          <security mode= "None|Transport|Message|TransportWithMessageCredential" >
              <transport clientCredentialType="None|Windows|Certificate"
                        protectionLevel="None|Sign|EncryptAndSign"
                        sslProtocols="Ssl3|Tls1|Tls11|Tls12">
                </transport>
          </security>
        </binding>
    </netTcpBinding>
    ```

  - **使用不同的 HTTP 连接发送消息**

    WCF 现在允许用户确保使用不同的基础 HTTP 连接发送特定消息。 有两种方法可以实现此目的：

    - **使用连接组名称前缀**

      用户可以指定 WCF 将用作连接组名称前缀的字符串。 使用不同的基础 HTTP 连接发送具有不同前缀的两个消息。 通过将键/值对添加到消息的 <xref:System.ServiceModel.Channels.Message.Properties%2A?displayProperty=nameWithType> 属性来设置前缀。 键是“HttpTransportConnectionGroupNamePrefix”，值是所需的前缀。

    - **使用不同的通道工厂**

      用户还可以启用一种功能，以确保使用由不同通道工厂所创建通道发送的消息将使用不同的基础 HTTP 连接。 若要启用此功能，用户必须将以下 `appSetting` 设置为 `true`：

      ```xml
      <appSettings>
          <add key="wcf:httpTransportBinding:useUniqueConnectionPoolPerFactory" value="true" />
      </appSettings>
      ```

- **Windows Workflow Foundation (WWF)**

  现在可以指定当请求超时之前存在某个未完成的“非协议”书签时，工作流服务针对无序操作请求将保持的秒数。 “非协议”书签是不与未完成的“接收”活动相关的书签。 某些活动会在其实现内创建非协议书签，因此非协议书签的存在可能不太明显。 此类书签包括“状态”和“选取”。 因此，如果你拥有使用状态机实现的工作流服务，或包含“选取”活动的工作流服务，你将很可能具有非协议书签。 通过将如下所示的行添加到 app.config 文件的 `appSettings` 部分来指定时间间隔：

  ```xml
  <add key="microsoft:WorkflowServices:FilterResumeTimeoutInSeconds" value="60"/>
  ```

  默认值为 60 秒。 如果 `value` 设置为 0，则会立即拒绝无序请求并出现错误，错误文本如下所示：

  ```console
  Operation 'Request3|{http://tempuri.org/}IService' on service instance with identifier '2b0667b6-09c8-4093-9d02-f6c67d534292' cannot be performed at this time. Please ensure that the operations are performed in the correct order and that the binding in use provides ordered delivery guarantees.
  ```

  当收到无序操作消息且没有非协议书签时，你将收到同一条消息。

  如果 `FilterResumeTimeoutInSeconds` 元素的值为非零，且没有非协议书签并且超时间隔过期，则操作失败并出现超时消息。

- **事务**

  你现在可以包含事务的分布式事务标识符，该事务导致了引发派生自 <xref:System.Transactions.TransactionException> 的异常。 通过将以下键添加到 app.config 文件的 `appSettings` 部分来完成此操作：

  ```xml
  <add key="Transactions:IncludeDistributedTransactionIdInExceptionMessage" value="true"/>
  ```

  默认值为 `false`。

- **网络连接**

  - **套接字重用**

    Windows 10 包括一个新的高可伸缩性网络算法，它能通过重用出站 TCP 连接的本地端口来更好地利用计算机资源。 .NET Framework 4.6 支持新算法，这使得 .NET 应用可以充分利用新的行为。 在以前版本的 Windows 中，有人工并发连接限制（通常为 16,384，即动态端口范围的默认大小），这可能导致负载下的端口耗尽，从而限制了服务的可伸缩性。

    在 .NET Framework 4.6 中，添加了两个 API，用于启用端口重用，有效撤消了并发连接方面的 64 KB 限制：

    - <xref:System.Net.Sockets.SocketOptionName?displayProperty=nameWithType> 枚举值。

    - <xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> 属性。

    默认情况下，<xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> 属性为 `false`，除非 `HKLM\SOFTWARE\Microsoft\.NETFramework\v4.0.30319` 注册表项的 `HWRPortReuseOnSocketBind` 值设置为 0x1。 若要对 HTTP 连接启用本地端口重用，请将 <xref:System.Net.ServicePointManager.ReusePort%2A?displayProperty=nameWithType> 属性设置为 `true`。 这将导致来自 <xref:System.Net.Http.HttpClient> 和 <xref:System.Net.HttpWebRequest> 的所有传出 TCP 套接字连接使用新的 Windows 10 套接字选项 [SO_REUSE_UNICASTPORT](/windows/desktop/WinSock/sol-socket-socket-options)，该选项将启用本地端口重用。

    调用 <xref:System.Net.Sockets.Socket.SetSocketOption%2A?displayProperty=nameWithType> 等方法时，编写仅限套接字的应用程序的开发人员可以指定 <xref:System.Net.Sockets.SocketOptionName?displayProperty=nameWithType> 选项，以便出站套接字在绑定期间重用本地端口。

  - **对国际域名和 PunyCode 的支持**

    一个新属性 <xref:System.Uri.IdnHost%2A> 已添加到了 <xref:System.Uri> 类，以更好地支持国际域名和 PunyCode。

- **在 Windows 窗体控件中调整大小。**

  此功能已在 .NET Framework 4.6 中展开，以包括绘制 <xref:System.Drawing.Design.UITypeEditor> 时所使用的 <xref:System.Drawing.Design.PaintValueEventArgs.Bounds%2A> 属性所指定的 <xref:System.Windows.Forms.DomainUpDown><xref:System.Windows.Forms.NumericUpDown><xref:System.Windows.Forms.DataGridViewComboBoxColumn><xref:System.Windows.Forms.DataGridViewColumn> 和 <xref:System.Windows.Forms.ToolStripSplitButton> 类型和矩形。

  这是一项可以选择使用的功能。 若要启用它，在应用程序配置 (app.config) 文件中将 `EnableWindowsFormsHighDpiAutoResizing` 元素设置为 `true`：

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

- **对代码页编码的支持**

  .NET Core 主要支持 Unicode 编码，在代码页编码方面默认提供一定程度的支持。 可以使用 <xref:System.Text.Encoding.RegisterProvider%2A?displayProperty=nameWithType> 方法注册代码页编码，从而支持 .NET Framework 可用但 .NET Core 不支持的代码页编码。 有关详细信息，请参阅 <xref:System.Text.CodePagesEncodingProvider?displayProperty=nameWithType>。

- **.NET Native**

  定位 .NET Core 且用 C# 或 Visual Basic 编写的 Windows 10 相关 Windows 应用程序可以利用将应用程序编译为本机代码（而非 IL）的新技术。 它们所生成的应用程序具有启动和执行时间更快速的特点。 有关详细信息，请参阅[使用 .NET Native 编译应用](../net-native/index.md)。 有关探讨与 JIT 编译和 NGEN 的差别以及对你的代码的意义的 .NET Native 概述，请参阅 [.NET Native 和编译](../net-native/net-native-and-compilation.md)。

  使用 Visual Studio 2015 或更高版本进行编译时，默认将应用程序编译为本机代码。 有关详细信息，请参阅 [.NET Native 入门](../net-native/getting-started-with-net-native.md)。

  为了支持调试 .NET Native 应用，已向非托管调试 API 添加大量新的接口和枚举。 有关详细信息，请参阅[调试（非托管 API 参考）](../unmanaged-api/debugging/index.md)主题。

- **开放源代码 .NET Framework 包**

  .NET Core 包（如不可变集合）、[SIMD API](https://www.nuget.org/packages/Microsoft.Bcl.Simd) 以及网络 API（如在 <xref:System.Net.Http> 命名空间中找到的网络 API）现在都可在 [GitHub](https://github.com/) 上用作开放源代码程序包。 要访问代码，请参阅 [GitHub 上的 .NET](https://github.com/dotnet/runtime)。 有关详细信息以及如何参与这些包的方法，请参阅 [.NET 简介](../../core/introduction.md)、[GitHub 上的 .NET 主页](https://github.com/dotnet/home)。

<a name="v452"></a>

## <a name="whats-new-in-net-framework-452"></a>.NET Framework 4.5.2 中的新增功能

- **ASP.NET 应用的新 API。** 新的 <xref:System.Web.HttpResponse.AddOnSendingHeaders%2A?displayProperty=nameWithType> 和 <xref:System.Web.HttpResponseBase.AddOnSendingHeaders%2A?displayProperty=nameWithType> 方法使你可以在响应刷新到客户端应用时检查并修改响应标头和状态代码。 考虑使用这些方法，而不是 <xref:System.Web.HttpApplication.PreSendRequestHeaders> 和 <xref:System.Web.HttpApplication.PreSendRequestContent> 事件；它们更为高效可靠。

  <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem%2A?displayProperty=nameWithType> 方法使你可以规划小型后台工作项目。 ASP.NET 跟踪这些项目，并防止 IIS 在所有后台工作项目完成之前突然中止辅助进程。 无法在 ASP.NET 托管的应用域之外调用此方法。

  新的 <xref:System.Web.HttpResponse.HeadersWritten?displayProperty=nameWithType> 和 <xref:System.Web.HttpResponseBase.HeadersWritten?displayProperty=nameWithType> 属性返回用于指示是否已编写响应标头的布尔值。 你可以使用这些属性确保对诸如 <xref:System.Web.HttpResponse.StatusCode%2A?displayProperty=nameWithType>（如果已编写标头，它将引发异常）等 API 的调用将成功。

- **在 Windows 窗体控件中调整大小。** 此功能已扩展。 你现在可以使用系统 DPI 设置调整下面其他控件的组件大小（例如，组合框中的下拉箭头）：

  - <xref:System.Windows.Forms.ComboBox>
  - <xref:System.Windows.Forms.ToolStripComboBox>
  - <xref:System.Windows.Forms.ToolStripMenuItem>
  - <xref:System.Windows.Forms.Cursor>
  - <xref:System.Windows.Forms.DataGridView>
  - <xref:System.Windows.Forms.DataGridViewComboBoxColumn>

  这是一项可以选择使用的功能。 若要启用它，在应用程序配置 (app.config) 文件中将 `EnableWindowsFormsHighDpiAutoResizing` 元素设置为 `true`：

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

- **新工作流功能。** 使用 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> 方法（并且因此实现 <xref:System.Transactions.IPromotableSinglePhaseNotification> 接口）的资源管理器可以使用新的 <xref:System.Transactions.Transaction.PromoteAndEnlistDurable%2A?displayProperty=nameWithType> 方法来请求以下内容：

  - 将该事物提升为 Microsoft 分布式事务处理协调器 (MSDTC) 事物。

  - 使用 <xref:System.Transactions.IPromotableSinglePhaseNotification> 替换 <xref:System.Transactions.ISinglePhaseNotification>，它是支持单阶段提交的持久性登记。

  此操作可以在相同的应用域内执行，而且不需要任何用于与 MSDTC 交互的额外非托管代码即可执行提升。 仅当存在从 <xref:System.Transactions?displayProperty=nameWithType> 对由可提升登记实现的 <xref:System.Transactions.IPromotableSinglePhaseNotification>`Promote` 方法进行的未处理调用时，才可调用新方法。

- **分析改进。** 以下新的非托管分析 API 提供更强大的分析功能：

  - [COR_PRF_ASSEMBLY_REFERENCE_INFO 结构](../unmanaged-api/profiling/cor-prf-assembly-reference-info-structure.md)
  - [COR_PRF_HIGH_MONITOR 枚举](../unmanaged-api/profiling/cor-prf-high-monitor-enumeration.md)
  - [GetAssemblyReferences 方法](../unmanaged-api/profiling/icorprofilercallback6-getassemblyreferences-method.md)
  - [GetEventMask2 方法](../unmanaged-api/profiling/icorprofilerinfo5-geteventmask2-method.md)
  - [SetEventMask2 方法](../unmanaged-api/profiling/icorprofilerinfo5-seteventmask2-method.md)
  - [AddAssemblyReference 方法](../unmanaged-api/profiling/icorprofilerassemblyreferenceprovider-addassemblyreference-method.md)

  之前的 `ICorProfiler` 实现支持依赖程序集的延迟加载。 新的分析 API 需要立即加载由探查器注入的依赖程序集，而不是在应用完全初始化后加载。 此更改不会影响现有 `ICorProfiler` API 的用户。

- **调试改进。** 以下新的未托管调试 API 提供与探查器更好的集成。 你现在可以访问由探查器插入的元数据，以及转储调试时编译器 ReJIT 请求所生成的本地变量和代码。

  - [SetWriteableMetadataUpdateMode 方法](../unmanaged-api/debugging/icordebugprocess7-setwriteablemetadataupdatemode-method.md)
  - [EnumerateLocalVariablesEx 方法](../unmanaged-api/debugging/icordebugilframe4-enumeratelocalvariablesex-method.md)
  - [GetLocalVariableEx 方法](../unmanaged-api/debugging/icordebugilframe4-getlocalvariableex-method.md)
  - [GetCodeEx 方法](../unmanaged-api/debugging/icordebugilframe4-getcodeex-method.md)
  - [GetActiveReJitRequestILCode 方法](../unmanaged-api/debugging/icordebugfunction3-getactiverejitrequestilcode-method.md)
  - [GetInstrumentedILMap 方法](../unmanaged-api/debugging/icordebugilcode2-getinstrumentedilmap-method.md)

- **事件跟踪更改。** .NET Framework 4.5.2 为较大的表面区域启用进程外的基于 Windows 事件跟踪 (ETW) 的活动跟踪。 这将使高级电源管理 (APM) 供应商提供轻型工具，这些工具可精确跟踪跨线程单个请求和活动的成本。  仅当 ETW 控制器启用它们时，才会引发这些事件；因此，这些更改不会影响之前编写的 ETW 代码或在禁用 ETW 的情况下运行的代码。

- **提升事务并将其转换为持久登记**

  <xref:System.Transactions.Transaction.PromoteAndEnlistDurable%2A?displayProperty=nameWithType> 是 .NET Framework 4.5.2 和 4.6 中新增的 API：

  ```csharp
  [System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name = "FullTrust")]
  public Enlistment PromoteAndEnlistDurable(Guid resourceManagerIdentifier,
                                            IPromotableSinglePhaseNotification promotableNotification,
                                            ISinglePhaseNotification enlistmentNotification,
                                            EnlistmentOptions enlistmentOptions)
  ```

  ```vb
  <System.Security.Permissions.PermissionSetAttribute(System.Security.Permissions.SecurityAction.LinkDemand, Name:="FullTrust")>
  public Function PromoteAndEnlistDurable(resourceManagerIdentifier As Guid,
                                          promotableNotification As IPromotableSinglePhaseNotification,
                                          enlistmentNotification As ISinglePhaseNotification,
                                          enlistmentOptions As EnlistmentOptions) As Enlistment
  ```

  该方法可能会由登记使用，该登记先前通过 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A?displayProperty=nameWithType> 创建以响应 <xref:System.Transactions.ITransactionPromoter.Promote%2A?displayProperty=nameWithType> 方法。 它要求 `System.Transactions` 将事务提升为 MSDTC 事务，并将可提升的登记“转换”为持久登记。 此方法成功完成后，`System.Transactions` 将不再引用 <xref:System.Transactions.IPromotableSinglePhaseNotification> 接口，将来的所有通知都将到达所提供的 <xref:System.Transactions.ISinglePhaseNotification> 接口。 相关登记必须作为持久登记，以支持事务日志记录和恢复。 请参阅 <xref:System.Transactions.Transaction.EnlistDurable%2A?displayProperty=nameWithType> 了解详细信息。 此外，登记必须支持 <xref:System.Transactions.ISinglePhaseNotification>。  此方法 *只能* 在处理 <xref:System.Transactions.ITransactionPromoter.Promote%2A?displayProperty=nameWithType> 调用时调用。 如果不是这种情况，则会引发 <xref:System.Transactions.TransactionException> 异常。

<a name="v451"></a>

## <a name="whats-new-in-net-framework-451"></a>.NET Framework 4.5.1 中的新增功能

**2014 年 4 月版更新**：

- [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/p/?LinkId=393658) 包括对可移植类库模板的更新，以支持以下方案：

  - 你可以使用面向 Windows 8.1、Windows Phone 8.1 和 Windows Phone Silverlight 8.1 的可移植库中的 Windows 运行时 API。

  - 在面向 Windows 8.1 或 Windows Phone 8.1 时，你可以在可移植库中包含 XAML（Windows.UI.XAML 类型）。 支持以下 XAML 模板：空白页、资源字典、模板控件和用户控件。

  - 你可以创建可移植 Windows 运行时组件（.winmd 文件）以用于面向 Windows 8.1 和 Windows Phone 8.1 的应用商店应用。

  - 你可以重定 Windows 应用商店或 Windows Phone 应用商店类库（例如可移植类库）的目标。

  有关这些更改的详细信息，请参阅[可移植类库](../cross-platform/portable-class-library.md)。

- .NET Framework 内容集现在包括 .NET Native 相关文档，这是一种用于生成和部署 Windows 应用程序的预编译技术。 为了提升性能，.NET Native 将应用程序直接编译为本机代码，而不是中间语言 (IL)。 有关详细信息，请参阅[使用 .NET Native 编译应用](../net-native/index.md)。

- [.NET Framework 引用源](https://referencesource.microsoft.com/)提供新的浏览体验和增强功能。 现在可以联机浏览 .NET Framework 源代码，[下载引用](https://referencesource.microsoft.com/download.html)以供脱机查看，并在调试时逐步执行源（包括修补程序和更新）。 有关详细信息，请参阅日志 [.NET 引用源的全新外观](https://devblogs.microsoft.com/dotnet/a-new-look-for-net-reference-source/)。

.NET Framework 4.5.1 基类中的新增功能和增强包括：

- 程序集的自动绑定重定向。 自 Visual Studio 2013 起，编译定位 .NET Framework 4.5.1 的应用程序时，如果应用程序或其组件引用同一程序集的多个版本，那么绑定重定向可能会被添加到应用程序配置文件中。 你也可以对面向 .NET framework 的早期版本的项目启用此功能。 有关详细信息，请参阅[如何：启用和禁用自动绑定重定向](../configure-apps/how-to-enable-and-disable-automatic-binding-redirection.md)。

- 可以收集诊断信息，以帮助开发人员提高服务器和云应用程序的性能。 有关详细信息，请参阅 <xref:System.Diagnostics.Tracing.EventSource.WriteEventWithRelatedActivityId%2A> 类中的 <xref:System.Diagnostics.Tracing.EventSource.WriteEventWithRelatedActivityIdCore%2A> 和 <xref:System.Diagnostics.Tracing.EventSource> 方法。

- 可以在垃圾回收过程中显式压缩大对象堆 (LOH)。 有关更多信息，请参见 <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType> 属性。

- 其他性能改进，例如 ASP.NET 应用挂起、多核 JIT 改进，以及更新 .NET Framework 后更快的应用启动。 有关详细信息，请参阅 [.NET Framework 4.5.1 公告](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)和 [ASP.NET 应用挂起](https://devblogs.microsoft.com/dotnet/asp-net-app-suspend-responsive-shared-net-web-hosting/)博客文章。

Windows 窗体的改进包括：

- 在 Windows 窗体控件中调整大小。 你可以使用系统 DPI 设置调整控件（例如，显示在属性网格中的图标）组件的大小，方法是使用应用的应用程序配置文件 (app.config) 中的条目选择使用该功能。 此功能当前在以下 Windows 窗体控件中受支持：

  - <xref:System.Windows.Forms.PropertyGrid>
  - <xref:System.Windows.Forms.TreeView>
  - <xref:System.Windows.Forms.DataGridView> 的某些方面（有关支持的其他控件，请参阅 [4.5.2 中的新功能](#v452)）

  若要启用此功能，请将新的 \<appSettings> 元素添加到配置文件 (app.config) 并将 `EnableWindowsFormsHighDpiAutoResizing` 元素设置为 `true`：

  ```xml
  <appSettings>
      <add key="EnableWindowsFormsHighDpiAutoResizing" value="true" />
  </appSettings>
  ```

有关在 Visual Studio 2013 中调试 .NET Framework 应用程序的改进包括：

- 返回 Visual Studio 调试器中的值。 在 Visual Studio 2013 中调试托管应用程序时，“自动”窗口会显示方法的返回类型和值。 此信息可用于桌面、Windows 应用商店和 Windows Phone 应用程序。 有关详细信息，请参阅[检查方法调用的返回值](/previous-versions/visualstudio/visual-studio-2013/dn323257(v=vs.120))。

- 针对 64 位应用程序的“编辑并继续”。 Visual Studio 2013 支持对桌面、Windows 应用商店和 Windows Phone 相关 64 位托管应用程序使用“编辑并继续”功能。 现有的限制对 32 位和 64 位应用仍然有效（请参阅[支持的代码更改 (C#)](/visualstudio/debugger/supported-code-changes-csharp) 文章的最后一节）。

- 异步识别调试。 为了简化在 Visual Studio 2013 中调试异步应用程序，调用堆栈隐藏了编译器提供的基础结构代码以支持异步编程，并链入了逻辑父框架，以便你可以更加明确逻辑程序的执行情况。 “任务”窗口将替换“并行任务”窗口，并显示与特定断点相关的任务，还会显示应用程序中当前处于活动状态或计划状态的任何其他任务。 可以在 [.NET Framework 4.5.1 公告](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)的“异步识别调试”一节中了解此功能。

- 改进对 Windows 运行时组件的异常支持。 在 Windows 8.1 中，Windows 应用商店应用抛出的异常保留了导致异常抛出的错误的相关信息（甚至可跨语言）。 可以在 [.NET Framework 4.5.1 公告](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)的“Windows 应用商店应用开发”一节中了解此功能。

自 Visual Studio 2013 起，可以使用[托管配置文件引导式优化工具 (Mpgo.exe)](../tools/mpgo-exe-managed-profile-guided-optimization-tool.md) 来优化 Windows 8.x 应用商店应用和桌面应用程序。

有关 ASP.NET 4.5.1 中的新功能，请参阅[适用于 Visual Studio 2013 的 ASP.NET 和 Web 工具发行说明](/aspnet/visual-studio/overview/2013/release-notes).

<a name="v45"></a>

## <a name="whats-new-in-net-framework-45"></a>.NET Framework 4.5 中的新增功能

### <a name="base-classes"></a>基类

- 能够在部署期间通过检测并关闭 .NET Framework 4 应用程序来减少系统重启。 请参阅[在 .NET Framework 4.5 安装期间减少系统重新启动](../deployment/reducing-system-restarts.md)。

- 支持 64 位平台上大于 2 GB 的数组。 此功能可在应用程序配置文件中启用。 请查看 [\<gcAllowVeryLargeObjects> 元素](../configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md)，其中还列出了有关对象大小和数组大小的其他限制。

- 通过服务器的后台垃圾回收来改进性能。 在 .NET Framework 4.5 中使用服务器垃圾回收功能时，后台垃圾回收功能会自动启用。 请参阅[垃圾回收的基础](../../standard/garbage-collection/fundamentals.md)主题的“后台服务器垃圾回收”一节。

- 后台实时 (JIT) 编译，可在多核处理器上使用此功能改进应用程序性能。 请参阅 <xref:System.Runtime.ProfileOptimization>。

- 可以限制正则表达式引擎在超时之前持续尝试解析正则表达式的时间。请参阅 <xref:System.Text.RegularExpressions.Regex.MatchTimeout%2A?displayProperty=nameWithType> 属性。

- 定义应用程序域的默认区域性的能力。 请参阅 <xref:System.Globalization.CultureInfo> 类。

- Unicode (UTF-16) 编码的控制台支持。 请参阅 <xref:System.Console> 类。

- 支持对区域性字符串排序和比较数据进行版本控制。 请参阅 <xref:System.Globalization.SortVersion> 类。

- 改进检索资源时的性能。 请参阅[打包和部署资源](../resources/packaging-and-deploying-resources-in-desktop-apps.md)。

- Zip 压缩改进，可减少压缩文件的大小。 请参阅 <xref:System.IO.Compression?displayProperty=nameWithType> 命名空间。

- 可以通过 <xref:System.Reflection.Context.CustomReflectionContext> 类自定义用于重写默认反射行为的反射上下文。

- 在 Windows 8 上使用 <xref:System.Globalization.IdnMapping?displayProperty=nameWithType> 类时支持应用程序国际化域名 (IDNA) 标准的 2008 版。

- 在 Windows 8 上使用 .NET Framework 时，可以将字符串比较委托给操作系统（这将实现 Unicode 6.0）。 在其他平台上运行时，.NET Framework 包括其自己的字符串比较数据，这将实现 Unicode 5.x。 请参阅 <xref:System.String> 类和 <xref:System.Globalization.SortVersion> 类的“备注”部分。

- 能够为每个应用程序域计算字符串的哈希代码。 请查看 [\<UseRandomizedStringHashAlgorithm> 元素](../configure-apps/file-schema/runtime/userandomizedstringhashalgorithm-element.md)。

- 类型反射支持 <xref:System.Type> 和 <xref:System.Reflection.TypeInfo> 类之间的拆分。 请参阅 [.NET Framework 中用于 Windows 应用商店应用的反射](../reflection-and-codedom/reflection-for-windows-store-apps.md)。

### <a name="managed-extensibility-framework-mef"></a>Managed Extensibility Framework (MEF)

在 .NET Framework 4.5 中，Managed Extensibility Framework (MEF) 新增了以下功能：

- 对泛型类型的支持。

- 利用基于约定的编程模型，你可以基于命名约定而非特性创建各个部分。

- 多个范围。

- 创建 Windows 8.x 应用商店应用时可以使用一部分 MEF。 此子集可作为 NuGet 库中的[可下载程序包](https://www.nuget.org/packages/Microsoft.Composition)提供。 若要安装此程序包，请在 Visual Studio 中打开项目，从“项目”菜单中选择“管理 NuGet 包”，然后联机搜索 `Microsoft.Composition` 程序包。

有关详细信息，请参阅 [Managed Extensibility Framework (MEF)](../mef/index.md)。

### <a name="asynchronous-file-operations"></a>异步文件操作

在 .NET Framework 4.5 中，已向 C# 和 Visual Basic 语言添加新的异步功能。 这些功能将添加用于执行异步操作的基于任务的模型。 若要使用此新模型，请使用 I/O 类中的异步方法。 请参阅[异步文件 I/O](../../standard/io/asynchronous-file-i-o.md)。

<a name="tools"></a>

### <a name="tools"></a>工具

在 .NET Framework 4.5 中，使用资源文件生成器 (Resgen.exe)，可以通过 .NET Framework 程序集中嵌入的 .resources 文件创建用于 Windows 8.x 应用商店应用的 .resw 文件。 有关详细信息，请参阅 [Resgen.exe（资源文件生成器）](../tools/resgen-exe-resource-file-generator.md)。

利用按托管配置优化 (Mpgo.exe) 工具，你可以通过优化本机映像程序集来改进应用程序的启动时间、内存使用率（工作集大小）和吞吐量。 该命令行工具会针对本机映像应用程序程序集生成配置文件数据。 请参阅 [Mpgo.exe（按托管配置文件优化工具）](../tools/mpgo-exe-managed-profile-guided-optimization-tool.md)。 自 Visual Studio 2013 起，可以使用 Mpgo.exe 优化 Windows 8.x 应用商店应用和桌面应用程序。

<a name="parallel"></a>

### <a name="parallel-computing"></a>并行计算

.NET Framework 4.5 新增了多项有关并行计算的功能和改进。 其中包括改进的性能、增强的控件、对异步编程的增强支持、新的数据流库以及对并行调试和性能分析的增强支持。 请参阅“使用 .NET 进行并行编程”博客中的 [.NET Framework 4.5 中有关并行的新增功能](https://devblogs.microsoft.com/pfxteam/whats-new-for-parallelism-in-net-4-5/)条目。

<a name="web"></a>

### <a name="web"></a>Web

ASP.NET 4.5 和 4.5.1 为 Web 窗体、WebSocket 支持、异步处理程序、性能增强和许多其他功能添加了模型绑定。 有关更多信息，请参见以下资源：

- [ASP.NET 4.5 和 Visual Studio 2012](/previous-versions/aspnet/hh420390(v=vs.110))

- [适用于 Visual Studio 2013 的 ASP.NET 和 Web 工具发行说明](/aspnet/visual-studio/overview/2013/release-notes)

### <a name="networking"></a>网络连接<a name="networking"></a>

.NET Framework 4.5 新增了 HTTP 应用程序的编程接口。 有关详细信息，请参阅新的 <xref:System.Net.Http?displayProperty=nameWithType> 和 <xref:System.Net.Http.Headers?displayProperty=nameWithType> 命名空间。

还包含针对用于接受 WebSocket 连接并与之交互（通过使用现有 <xref:System.Net.HttpListener> 和相关类）的新编程接口的支持。 有关详细信息，请参阅新的 <xref:System.Net.WebSockets> 命名空间和 <xref:System.Net.HttpListener> 类。

此外，.NET Framework 4.5 新增了以下网络改进：

- 与 RFC 兼容的 URI 支持。 有关详细信息，请参阅 <xref:System.Uri> 和相关类。

- 对国际域名 (IDN) 分析的支持。 有关详细信息，请参阅 <xref:System.Uri> 和相关类。

- 对电子邮件地址国际化 (EAI) 的支持。 有关更多信息，请参见 <xref:System.Net.Mail> 命名空间。

- 改进的 IPv6 支持。 有关更多信息，请参见 <xref:System.Net.NetworkInformation> 命名空间。

- 双重模式套接字支持。 有关更多信息，请参见 <xref:System.Net.Sockets.Socket> 和 <xref:System.Net.Sockets.TcpListener> 类。

<a name="client"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

在 .NET Framework 4.5 中，Windows Presentation Foundation (WPF) 在以下几个领域进行了更改和改进：

- 利用新的 <xref:System.Windows.Controls.Ribbon.Ribbon> 控件，你可以实现承载快速访问工具栏、应用程序菜单和选项卡的功能区用户界面。

- 支持同步和异步数据验证的新 <xref:System.ComponentModel.INotifyDataErrorInfo> 接口。

- <xref:System.Windows.Controls.VirtualizingPanel> 和 <xref:System.Windows.Threading.Dispatcher> 类的新功能。

- 通过在非 UI 线程上访问集合，改进了在显示大型分组数据集时的性能。

- 针对静态属性的数据绑定、针对实现 <xref:System.Reflection.ICustomTypeProvider> 接口的自定义类型的数据绑定，以及从绑定表达式中检索数据绑定信息。

- 在值发生更改时重新定位数据（实时数据整理）。

- 能够检查项目容器的数据上下文是否已断开连接。

- 能够设置属性更改和数据源更新之间的时间间隔。

- 改进了对实现弱事件模式的支持。 此外，事件现在可以接受标记扩展。

<a name="windows_communication_foundation"></a>

### <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

为了更轻松地编写和维护 Windows Communication Foundation (WCF) 应用程序，.NET Framework 4.5 中新增了以下功能：

- 简化生成的配置文件。

- 对协定优先开发的支持。

- 能够更轻松地配置 ASP.NET 兼容模式。

- 对默认传输属性值的更改，可减小你必须设置这些值的可能性。

- 对 <xref:System.Xml.XmlDictionaryReaderQuotas> 类进行更新，可减小你必须手动为 XML 字典读取器配置配额的可能性。

- 作为生成过程的一部分，由 Visual Studio 验证 WCF 配置文件，以便你可以在运行应用程序之前检测配置错误。

- 新的异步流支持。

- 新的 HTTPS 协议映射，使你能够更轻松地通过 Internet Information Services (IIS) 在 HTTPS 上公开终结点。

- 能够通过将 `?singleWSDL` 追加到服务 URL，来在单个 WSDL 文档中生成元数据。

- WebSockets 支持，通过端口 80 和 443 启用真正的双向通信，其性能特性与 TCP 传输类似。

- 对在代码中配置服务的支持。

- XML 编辑器工具提示。

- <xref:System.ServiceModel.ChannelFactory> 缓存支持。

- 二进制文件编码器压缩支持。

- 对 UDP 传输的支持，这可使开发人员编写使用“发后不理”消息的服务。 客户端向服务发送消息，且不希望从该服务获得响应。

- 能够在使用 HTTP 传输和传输安全性时，支持单个 WCF 终结点上的多个身份验证模式。

- 对使用国际域名 (IDN) 的 WCF 服务的支持。

有关详细信息，请参阅 [Windows Communication Foundation 中的新增功能](../wcf/whats-new.md)。

<a name="windows_workflow_foundation"></a>

### <a name="windows-workflow-foundation-wf"></a>Windows Workflow Foundation (WF)

在 .NET Framework 4.5 中，已向 Windows Workflow Foundation (WF) 添加多项新功能，包括：

- 首次作为 .NET Framework 4.0.1（[.NET Framework 4 平台更新 1](/archive/blogs/endpoint/microsoft-net-framework-4-platform-update-1)）的一部分引入的状态机工作流。 此更新包括可使开发人员创建状态机工作流的多个新类和活动。 这些类和活动已针对 .NET Framework 4.5 更新为包含：

  - 对状态设置断点的能力。

  - 在工作流设计器中复制和粘贴转换的能力。

  - 对共享的触发器转换创建的设计器支持。

  - 创建状态机工作流的活动，包括：<xref:System.Activities.Statements.StateMachine>、<xref:System.Activities.Statements.State> 和 <xref:System.Activities.Statements.Transition>。

- 增强的工作流设计器功能如下:

  - Visual Studio 中增强的工作流搜索功能，包括“快速查找”和“在文件中查找”。

  - 将第二个子活动添加到容器活动中时自动创建“序列”活动以及在“序列”活动中包括这两个活动的能力。

  - 平移支持，可让工作流的可见部分发生更改，而无需使用滚动条。

  - 新“文档大纲”视图，它在树样式的大纲视图中显示工作流组件并允许在“文档大纲”视图中选择组件。

  - 向活动中添加批注的能力。

  - 通过使用工作流设计器定义和使用活动委托的能力。

  - 状态机和流程图工作流中活动和转换的自动连接和自动插入。

- XAML 文件的单个元素中工作流的视图状态信息存储，以便你可以轻松定位和编辑视图状态信息。

- 可防止子活动持久化的 NoPersistScope 容器活动。

- 对 C# 表达式的支持：

  - 使用 Visual Basic 的工作流项目将使用 Visual Basic 表达式，C# 工作流项目将使用 C# 表达式。

  - 在 Visual Studio 2010 中创建并具有 Visual Basic 表达式的 C# 工作流项目与使用 C# 表达式的 C# 工作流项目兼容。

- 版本控制增强功能：

  - 新 <xref:System.Activities.WorkflowIdentity> 类，它提供了保留的工作流实例与其工作流定义之间的映射。

  - 同一主机中多个工作流版本的并行执行，包括 <xref:System.ServiceModel.Activities.WorkflowServiceHost>。

  - 在动态更新中，修改保留的工作流实例的定义的能力。

- 协定优先工作流服务开发，它为自动生成活动以匹配现有服务协定提供支持。

有关详细信息，请参阅 [Windows Workflow Foundation 中的新增功能](../windows-workflow-foundation/whats-new-in-wf-in-dotnet.md)。

<a name="tailored"></a>

### <a name="net-for-windows-8x-store-apps"></a>适用于 Windows 8.x 应用商店应用的 .NET

Windows 8.x 应用商店应用专为特定外形规格而设计，并利用 Windows 操作系统的强大技术支持。 可以使用一部分 .NET Framework 4.5 或 4.5.1 生成用 C# 或 Visual Basic 编写的 Windows 相关 Windows 8.x 应用商店应用。 该部分被称作适用于 Windows 8.x 应用商店应用的 .NET，详见[概述](/previous-versions/windows/apps/br230302(v=vs.140))

### <a name="portable-class-libraries"></a>可移植类库<a name="portable"></a>

利用 Visual Studio 2012（及更高版本）中的可移植类库项目，可以编写和生成在多个 .NET Framework 平台上运行的托管程序集。 使用可移植类库项目，可以选择目标平台（如 Windows Phone 和适用于 Windows 8.x 应用商店应用的 .NET）。 项目中的可用类型和成员自动限制为这些平台中的公共类型和成员。 有关详细信息，请参阅[可移植类库](../cross-platform/portable-class-library.md)。

## <a name="see-also"></a>请参阅

- [.NET Framework 和带外版本](../get-started/the-net-framework-and-out-of-band-releases.md)
- [.NET Framework 中辅助功能的新增功能](whats-new-in-accessibility.md)
- [Visual Studio 2019 中的新增功能](/visualstudio/ide/whats-new-visual-studio-2019)
- [ASP.NET](/aspnet)
- [Visual Studio 中的 C++ 新变化](/cpp/what-s-new-for-visual-cpp-in-visual-studio)
- [下载 .NET SDK](https://dotnet.microsoft.com/download)
