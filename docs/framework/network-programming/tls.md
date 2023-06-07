---
title: .NET Framework 中的传输层安全性 (TLS) 最佳做法
description: 介绍在 .NET Framework 中使用传输层安全性 (TLS) 的最佳做法
ms.date: 10/22/2018
helpviewer_keywords:
- sending data, Internet security
- protocols, Internet security
- Network security
- network resources, Internet security
- receiving data, Internet security
- authentication [.NET Framework], Internet security
- Internet, security
- security [.NET Framework], Internet
- permissions [.NET Framework], Internet
ms.openlocfilehash: 8de15dc033ecda3137f5f3ea37b9e35ac9df7e13
ms.sourcegitcommit: ae2e8a61a93c5cf3f0035c59e6b064fa2f812d14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89359293"
---
# <a name="transport-layer-security-tls-best-practices-with-the-net-framework"></a>.NET Framework 中的传输层安全性 (TLS) 最佳做法

传输层安全性 (TLS) 协议是一个行业标准，旨在帮助保护通过 Internet 所传输信息的私密性。 [TLS 1.2](https://tools.ietf.org/html/rfc5246) 标准与以前版本相比在安全性方面有了很多提升。 TLS 1.2 最终将被最新发布的标准 [TLS 1.3](https://tools.ietf.org/html/rfc8446) 取代，后者速度更快，安全性更高。 文本介绍了如何保护使用 TLS 协议的 .NET Framework 应用程序安全的建议。

为确保 .NET Framework 应用程序的安全性，TLS 版本不应  被硬编码。 .NET Framework 应用程序应使用操作系统 (OS) 支持的 TLS 版本。

此文档面向以下开发人员：

- 直接使用 <xref:System.Net> API（例如，<xref:System.Net.Http.HttpClient?displayProperty=nameWithType> 和 <xref:System.Net.Security.SslStream?displayProperty=nameWithType>）。
- 直接使用 WCF 客户端和使用 <xref:System.ServiceModel?displayProperty=nameWithType> 命名空间的服务。

我们建议：

- 在你的应用上面向 NET Framework 4.7 或更高版本。 在你的 WCF 应用上面向 NET Framework 4.7.1 或更高版本。
- 不要指定 TLS 版本。 配置你的代码，让操作系统来决定 TLS 版本。
- 执行全面的代码审核，以验证你未指定 TLS 或 SSL 版本。

当你的应用让操作系统来选择 TLS 版本时：

- 它将会自动利用以后添加的新协议（例如 TLS 1.3）。
- 操作系统将阻止发现不安全的协议。

[审核代码并对代码进行更改](#audit-your-code-and-make-code-changes)这一部分介绍了如何审核和更新你的代码。

本文阐释如何针对应用所面向并在其上运行的 .NET Framework 的版本启用可用的最强安全性。 当应用显式设置安全协议和版本时，它将选择退出任何其他替代项，并选择退出 .NET Framework 和操作系统默认行为。 如果你希望应用能够协商 TLS 1.2 连接，请显式设置为较低的 TLS 版本，以阻止 TLS 1.2 连接。

如果无法避免硬编码协议版本，我们强烈建议你指定 TLS 1.2。 有关标识和删除 TLS 1.0 依赖项的指南，请下载[解决 TLS 1.0 问题](https://www.microsoft.com/download/details.aspx?id=55266)白皮书。

WCF 支持 TLS1.0、1.1 和 1.2 作为 .NET Framework 4.7 中的默认设置。 从 .NET Framework 4.7.1 开始，WCF 默认为操作系统配置的版本。 如果某个应用程序使用 `SslProtocols.None` 显式配置，则在使用 NetTcp 传输时，WCF 将使用操作系统默认设置。

你可以在 GitHub 问题 [.NET Framework 中的传输层安全性 (TLS) 最佳做法](https://github.com/dotnet/docs/issues/4675)中提问有关此文档的问题。

## <a name="audit-your-code-and-make-code-changes"></a>审核代码并对代码进行更改

对于 ASP.NET 应用程序，检查 _web.config_ 的 `<system.web><httpRuntime targetFramework>` 元素，以验证你所使用的是 .NET Framework 的目标版本。

有关 Windows 窗体和其他应用程序，请参阅[如何：面向 .NET Framework 的某个版本](/visualstudio/ide/visual-studio-multi-targeting-overview)。

使用以下部分验证你未使用特定 TLS 或 SSL 版本。

## <a name="if-your-app-targets-net-framework-47-or-later-versions"></a>如果你的应用面向 .NET Framework 4.7 或更高版本

以下部分说明如何验证你未使用特定 TLS 或 SSL 版本。

### <a name="for-http-networking"></a>对于 HTTP 网络

使用 .NET Framework 4.7 及更高版本的 <xref:System.Net.ServicePointManager> 将使用操作系统中配置的默认安全协议。 若要获取默认操作系统选择，如有可能，请不要设置 <xref:System.Net.ServicePointManager.SecurityProtocol?displayProperty=nameWithType> 属性的值，该值默认为 <xref:System.Net.SecurityProtocolType.SystemDefault?displayProperty=nameWithType>。

由于 <xref:System.Net.SecurityProtocolType.SystemDefault?displayProperty=nameWithType> 设置会导致 <xref:System.Net.ServicePointManager> 使用由操作系统配置的默认安全协议，因此应用程序可能会根据运行的操作系统以不同的方式运行。 例如，Windows 7 SP1 使用 TLS 1.0，而 Windows 8 和 Windows 10 使用 TLS 1.2。

针对 HTTP 网络的面向 .NET Framework 4.7 或更高版本的情况，本文剩余部分与此不相关。

### <a name="for-tcp-sockets-networking"></a>对于 TCP 套接字网络

<xref:System.Net.Security.SslStream>，使用 .NET Framework 4.7 和更高版本，默认为由操作系统选择最佳安全协议和版本。 若要获取默认操作系统最佳选择，如有可能，请不要使用采取显式 <xref:System.Security.Authentication.SslProtocols> 参数的 <xref:System.Net.Security.SslStream> 的方法重载。 否则，将传递 <xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType>。 建议不要使用 <xref:System.Security.Authentication.SslProtocols.Default>；设置 `SslProtocols.Default` 会强制使用 SSL 3.0 /TLS 1.0，而不使用 TLS 1.2。

不要设置 <xref:System.Net.ServicePointManager.SecurityProtocol> 属性的值（针对 HTTP 网络）。

不要使用采取显式 <xref:System.Security.Authentication.SslProtocols> 参数的 <xref:System.Net.Security.SslStream> 的方法重载（针对 TCP 套接字网络）。 当你将应用重定向到 .NET Framework 4.7 或更高版本时，将遵循最佳做法建议。

针对 TCP 套接字网络的面向 .NET Framework 4.7 或更高版本的情况，本主题剩余部分与此不相关。

<a name="wcf-tcp-cert"></a>

### <a name="for-wcf-tcp-transport-using-transport-security-with-certificate-credentials"></a>对于使用具有证书凭据的传输安全性的 WCF TCP 传输

WCF 使用与 .NET Framework 的其余部分相同的网络堆栈。

如果你面向 4.7.1，则 WCF 将被配置为默认由操作系统选择最佳安全协议（除非显式对其配置）：

- 在你的应用程序配置文件中。
-  或者，在你的源代码中的应用程序中。

默认情况下，.NET Framework 4.7 和更高版本将被配置为使用 TLS 1.2，并允许使用 TLS 1.1 或 TLS 1.0 进行连接。 通过将你的绑定配置为使用 <xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType> 来配置 WCF，以允许操作系统选择最佳安全协议。 可在 <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols> 上进行此设置。 `SslProtocols.None` 可以从 <xref:System.ServiceModel.NetTcpSecurity.Transport> 中进行访问。 `NetTcpSecurity.Transport` 可以从 <xref:System.ServiceModel.NetTcpBinding.Security> 中进行访问。

如果你使用自定义绑定：

- 通过将 <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols> 设置为使用 <xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType> 来配置 WCF，以允许操作系统选择最佳安全协议。
-  或者，配置在配置路径 `system.serviceModel/bindings/customBinding/binding/sslStreamSecurity:sslProtocols` 中使用的协议。

如果你没有  使用自定义绑定，  且正使用配置来设置你的 WCF 绑定，则设置配置路径 `system.serviceModel/bindings/netTcpBinding/binding/security/transport:sslProtocols` 中使用的协议。

### <a name="for-wcf-message-security-with-certificate-credentials"></a>对于具有证书凭据的 WCF 消息安全性

默认情况下，.NET Framework 4.7 和更高版本使用 <xref:System.Net.ServicePointManager.SecurityProtocol> 属性中指定的协议。 当 [AppContextSwitch](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)`Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols` 设置为 `true` 时，WCF 将选择最佳协议（最高至 TLS 1.0）。

## <a name="if-your-app-targets-a-net-framework-version-earlier-than-47"></a>如果你的应用面向 .NET Framework 4.7 之前的版本

使用以下部分审核你的代码，以验证你未设置特定 TLS 或 SSL 版本：

### <a name="for-net-framework-46---462-and-not-wcf"></a>对于 .NET Framework 4.6 - 4.6.2（而不是 WFC）

将 `DontEnableSystemDefaultTlsVersions``AppContext` 开关设置为 `false`。 请参阅[通过 AppContext 开关配置安全性](#configuring-security-via-appcontext-switches)。

### <a name="for-wcf-using-net-framework-46---462-using-tcp-transport-security-with-certificate-credentials"></a>对于使用具有证书凭据的 TCP 传输安全性的 .NET Framework 4.6 - 4.6.2 的 WCF

必须安装最新的操作系统修补程序。 请参阅[安全更新](#security-updates)。

除非显式配置协议版本，否则 WCF 框架将自动选择高至 TLS 1.2 的最高协议。 有关详细信息，请参阅上一部分[对于使用具有证书凭据的传输安全性的 WCF TCP 传输](#wcf-tcp-cert)。

### <a name="for-net-framework-35---452-and-not-wcf"></a>对于 .NET Framework 3.5 - 4.5.2（而不是 WFC）

我们建议将你的应用升级到 .NET Framework 4.7 或更高版本。 如果不能升级，请执行以下步骤。

将 [SchUseStrongCrypto](#schusestrongcrypto) 和 [SystemDefaultTlsVersions](#systemdefaulttlsversions) 注册表项设置为 1。 请参阅[通过 Windows 注册表配置安全性](#configuring-security-via-the-windows-registry)。 .NET Framework 3.5 版本仅在传递显式 TLS 值时才支持 `SchUseStrongCrypto` 标志。

如果在 .NET Framework 3.5 上运行，则需安装热修补程序，以便你的程序可以指定 TLS 1.2：

| [KB3154518](https://support.microsoft.com/kb/3154518) | 可靠性汇总 HR-1605 - 在 Windows 7 SP1 和 Server 2008 R2 SP1 上的 .NET Framework 3.5.1 中包含对 TLS 系统默认版本的支持 |
| --- | --- |
| [KB3154519](https://support.microsoft.com/kb/3154519) | 可靠性汇总 HR-1605 - 在 Windows Server 2012 上的 .NET Framework 3.5 中包含对 TLS 系统默认版本的支持 |
| [KB3154520](https://support.microsoft.com/kb/3154520) | 可靠性汇总 HR-1605 - 在 Windows 8.1 and Windows Server 2012 R2 上的 .NET Framework 3.5 中包含对 TLS 系统默认版本的支持 |
| [KB3156421](https://support.microsoft.com/kb/3156421) | 1605 修补程序汇总 3154521 - 针对 Windows 上的 .NET Framework 4.5.2 和 4.5.1 |

### <a name="for-wcf-using-net-framework-35---452-using-tcp-transport-security-with-certificate-credentials"></a>对于使用具有证书凭据的 TCP 传输安全性的 .NET Framework 3.5 - 4.5.2 的 WCF

WCF 框架的这些版本被硬编码为使用值 SSL 3.0 和 TLS 1.0。 这些值不能更改。 必须更新和重定向到 NET Framework 4.6 或更高版本，以使用 TLS 1.1 和 TLS 1.2。

## <a name="if-your-app-targets-net-framework-35"></a>如果应用面向 .NET Framework 3.5

如果必须显式设置安全协议，而不是由 .NET 或操作系统选择安全协议，请将 `SecurityProtocolTypeExtensions` 和 `SslProtocolsExtension` 枚举添加到你的代码中。 `SecurityProtocolTypeExtensions` 和 `SslProtocolsExtension` 包含 `Tls12`、`Tls11` 的值和 `SystemDefault` 值。 有关详细信息，请参阅[在 Windows 8.1 和 Windows Server 2012 R2 上的 .NET Framework 3.5 中包含对 TLS 系统默认版本的支持](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the--net-framework)。

<a name="configuring-security-via-appcontext-switches"></a>

## <a name="configuring-security-via-appcontext-switches-for-net-framework-46-or-later-versions"></a>通过 AppContext 开关配置安全性（适用于 .NET Framework 4.6 或更高版本）

如果你的应用面向 .NET Framework 4.6 或更高版本或在其上运行，则请关注本节中所述的 [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 开关。 无论它们是默认设置还是对其显式设置，开关均应为 `false`（如有可能）。 如果希望通过一个或这两个开关配置安全性，则不要在你的代码中指定安全协议值，执行此操作将会替代此开关。

无论你使用 HTTP 网络 (<xref:System.Net.ServicePointManager>) 还是使用 TCP 套接字网络 (<xref:System.Net.Security.SslStream>)，开关都具有相同的效果。

### <a name="switchsystemnetdontenableschusestrongcrypto"></a>Switch.System.Net.DontEnableSchUseStrongCrypto

`Switch.System.Net.DontEnableSchUseStrongCrypto` 的值为 `false` 将导致你的应用使用强加密。 `DontEnableSchUseStrongCrypto` 的值为 `false` 将使用更为安全的网络协议（TLS 1.2、TLS 1.1 和 TLS 1.0），并阻止不安全的协议。 有关详细信息，请参阅 [SCH_USE_STRONG_CRYPTO 标志](#the-sch_use_strong_crypto-flag)。 值为 `true` 将为你的应用禁用强加密。

如果你的应用面向 .NET Framework 4.6 或更高版本，则该开关默认为 `false`。 这是我们建议使用的安全默认值。 如果你的应用在 .NET Framework 4.6 上运行，但面向早期版本，则开关默认为 `true`。 在这种情况下，应显式将其设置为 `false`。

如果需要连接到不支持强加密且无法升级的旧服务，则 `DontEnableSchUseStrongCrypto` 的值只能为 `true`。

### <a name="switchsystemnetdontenablesystemdefaulttlsversions"></a>Switch.System.Net.DontEnableSystemDefaultTlsVersions

`Switch.System.Net.DontEnableSystemDefaultTlsVersions` 的值为 `false` 将导致你的应用允许操作系统选择协议。 值为 `true` 将导致你的应用使用由 .NET Framework 选取的协议。

如果你的应用面向 .NET Framework 4.7 或更高版本，则此开关默认为 `false`。 这是我们建议使用的安全默认值。 如果你的应用在 .NET Framework 4.7 或更高版本上运行，但面向早期版本，则开关默认为 `true`。 在这种情况下，应显式将其设置为 `false`。

### <a name="switchsystemservicemodeldisableusingservicepointmanagersecurityprotocols"></a>Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols

`Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols` 的值为 `false` 将导致你的应用程序使用 `ServicePointManager.SecurityProtocols` 中定义的值，以确保使用证书凭据的消息的安全性。 值为 `true` 将使用可用的最高协议（高至 TLS1.0）

对于面向 .NET Framework 4.7 和更高版本的应用程序，此值默认为 `false`。 对于面向 .NET Framework 4.6.2 和早期版本的应用程序，此值默认为 `true`。

### <a name="switchsystemservicemodeldontenablesystemdefaulttlsversions"></a>Switch.System.ServiceModel.DontEnableSystemDefaultTlsVersions

`Switch.System.ServiceModel.DontEnableSystemDefaultTlsVersions` 的值为 `false` 将设置默认配置，以允许操作系统选择协议。 值为 `true` 会将默认值设置为可用的最高协议（高至 TLS1.2）。

对于面向 .NET Framework 4.7.1 和更高版本的应用程序，此值默认为 `false`。 对于面向 .NET Framework 4.7 和早期版本的应用程序，此值默认为 `true`。

有关 TLS 协议的详细信息，请参阅[缓解措施：TLS 协议](../migration-guide/mitigation-tls-protocols.md)。 有关 `AppContext` 开关的详细信息，请参阅 [`<AppContextSwitchOverrides> Element`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)。

## <a name="configuring-security-via-the-windows-registry"></a>通过 Windows 注册表配置安全性

> [!WARNING]
> 设置注册表项会影响系统上的所有应用程序。 仅当你完全控制计算机并可以控制对注册表的更改时才可使用此选项。

如果不能设置一个或两个 `AppContext` 开关，可以使用本节中所述的 Windows 注册表项来控制应用所使用的安全协议。 如果你的应用在 .NET Framework 4.5.2 或之前的版本上运行或你无法编辑配置文件，则可能无法使用一个或两个 `AppContext` 开关。 如果想要在注册表中配置安全性，则不要在你的代码中指定安全协议，这样做将会替代该注册表设置。

注册表项的名称类似于相应 `AppContext` 开关的名称，但其不带预置的 `DontEnable`。 例如，`AppContext` 开关 `DontEnableSchUseStrongCrypto` 是名为 [SchUseStrongCrypto](#schusestrongcrypto) 的注册表项。

这些注册表项适用于安装了最新安全修补程序的所有 .NET Framework 版本。 请参阅[安全更新](#security-updates)。

无论你使用 HTTP 网络 (<xref:System.Net.ServicePointManager>) 还是使用 TCP 套接字网络 (<xref:System.Net.Security.SslStream>)，下面介绍的所有注册表项都具有相同的效果。

### <a name="schusestrongcrypto"></a>SchUseStrongCrypto

`HKEY_LOCAL_MACHINE\SOFTWARE\[Wow6432Node\]Microsoft\.NETFramework\<VERSION>: SchUseStrongCrypto` 注册表项具有类型为 DWORD 的值。 值为 1 将导致你的应用使用强加密。 强加密会使用更为安全的网络协议（TLS 1.2、TLS 1.1 和 TLS 1.0），并阻止不安全的协议。 值为 0 将禁用强加密。 有关详细信息，请参阅 [SCH_USE_STRONG_CRYPTO 标志](#the-sch_use_strong_crypto-flag)。

如果你的应用面向 .NET Framework 4.6 或更高版本，则此注册表项默认值为 1。 这是我们建议使用的安全默认值。 如果你的应用面向 .NET Framework 4.5.2 或更高版本，则此注册表项默认为 0。 在这种情况下，应显式将其值设置为 1。

如果需要连接到不支持强加密且无法升级的旧服务，则此注册表项的值只能为 0。

### <a name="systemdefaulttlsversions"></a>SystemDefaultTlsVersions

`HKEY_LOCAL_MACHINE\SOFTWARE\[Wow6432Node\]Microsoft\.NETFramework\<VERSION>: SystemDefaultTlsVersions` 注册表项具有类型为 DWORD 的值。 值为 1 将导致你的应用允许操作系统选择协议。 值为 0 将导致你的应用使用由 .NET Framework 选取的协议。

`<VERSION>` 必须为 v4.0.30319（对于 .NET Framework 4 和更高版本）或 v2.0.50727（对于 .NET Framework 3.5）。

如果你的应用面向 .NET Framework 4.7 或更高版本，则此注册表项默认值为 1。 这是我们建议使用的安全默认值。 如果你的应用面向 .NET Framework 4.6.1 或更高版本，则此注册表项默认为 0。 在这种情况下，应显式将其值设置为 1。

有关详细信息，请参阅 [Windows 10 版本 1511 和 Windows Server 2016 Technical Preview 4 的累积更新：2016 年 5 月 10 日](https://support.microsoft.com/help/3156421/cumulative-update-for-windows-10-version-1511-and-windows-server-2016)。

有关 .NET Framework 3.5.1 中的详细信息，请参阅[在 Windows 7 SP1 和 Server 2008 R2 SP1 上的 .NET Framework 3.5.1 中包含对 TLS 系统默认版本的支持](https://support.microsoft.com/help/3154518/support-for-tls-system-default-versions-included-in-the--net-framework)。

以下 .REG  文件将注册表项及其变量设置为其最安全的值：

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001
```

## <a name="configuring-schannel-protocols-in-the-windows-registry"></a>在 Windows 注册表中配置 Schannel 协议

可以使用注册表细化控制你的客户端和/或服务器应用协商的协议。 你的应用的网络将遍历 Schannel（它是[安全通道](/windows/desktop/SecAuthN/secure-channel)的另一个名称）。 通过配置 `Schannel`，可以配置你的应用的行为。

从 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols` 注册表项开始。 在该注册表项下，可以在集 `SSL 2.0`、`SSL 3.0`、`TLS 1.0`、`TLS 1.1` 和 `TLS 1.2` 中创建任何子项。 在每个子项下，可以创建子项 `Client` 和/或 `Server`。 在 `Client` 和 `Server` 下，可创建 DWORD 值 `DisabledByDefault`（0 或 1）和 `Enabled`（0 或 1）。

## <a name="the-sch_use_strong_crypto-flag"></a><a name="the-sch_use_strong_crypto-flag"></a>SCH_USE_STRONG_CRYPTO 标志

启用后（默认情况下，通过 `AppContext` 开关或 Windows 注册表启动），当你的应用请求 TLS 安全协议时，.NET Framework 将使用 `SCH_USE_STRONG_CRYPTO` 标志。 可以通过 `AppContext` 开关或注册表来默认启用 `SCH_USE_STRONG_CRYPTO` 标志。 操作系统将标志传递到 `Schannel`，以指示它禁用已知弱加密算法、密码套件和 TLS/SSL 协议版本（否则，可能会启用该协议以获得更好的互操作性）。 有关详细信息，请参见:

- [安全通道](/windows/desktop/SecAuthN/secure-channel)
- [SCHANNEL_CRED 结构](/windows/win32/api/schannel/ns-schannel-schannel_cred)

当你显式使用 <xref:System.Net.SecurityProtocolType> 或 <xref:System.Security.Authentication.SslProtocols> 的 `Tls` (TLS 1.0)、`Tls11` 或 `Tls12` 枚举的值时，`SCH_USE_STRONG_CRYPTO` 标志还将被传递到 `Schannel`。

## <a name="security-updates"></a>安全更新

本文中的最佳做法将取决于最新安装的安全更新。 这些更新包括使用高级 .NET Framework 4.7 和更高版本的功能。 如果你的应用在 .NET Framework 4.7 和更高版本上运行，则最新安全更新很重要（即使它面向早期版本）。

若要更新 .NET Framework，以允许操作系统选择要使用的 TLS 的最佳版本，必须至少安装：

- [.NET Framework 2017 年 8 月质量汇总预览](https://devblogs.microsoft.com/dotnet/net-framework-august-2017-preview-of-quality-rollup/)。
-  或 [.NET Framework 2017 年 9 月安全和质量汇总](https://devblogs.microsoft.com/dotnet/net-framework-september-2017-security-and-quality-rollup/)。

另请参阅：

- [.NET Framework 版本和依赖关系](../migration-guide/versions-and-dependencies.md)
- [如何：确定已安装的 .NET Framework 版本](../migration-guide/how-to-determine-which-versions-are-installed.md)。

## <a name="support-for-tls-12"></a>支持 TLS 1.2

为使你的应用与 TLS 1.2 协商，操作系统和 .NET Framework 版本均需支持 TLS 1.2。

**支持 TLS 1.2 的操作系统要求**

若要在支持它们的系统上启用或重新启用 TLS 1.2 和/或 TLS 1.1，请参阅[传输层安全性 (TLS) 注册表设置](/windows-server/security/tls/tls-registry-settings)。

| 操作系统  | TLS 1.2 支持  |
| --- | --- |
| Windows 10<br>Windows 2016 Server | 默认情况下支持和启用。 |
| Windows 8.1<br>Windows Server 2012 R2 | 默认情况下支持和启用。 |
| Windows 8.0<br>Windows Server 2012 | 默认情况下支持和启用。 |
| Windows 7 SP1<br>Windows Server 2008 R2 SP1 | 默认情况下支持但不启用。 请参阅[传输层安全性 (TLS) 注册表设置](/windows-server/security/tls/tls-registry-settings)网页，以详细了解如何启用 TLS 1.2。 |
| Windows Server 2008 | 支持 TLS 1.2 和 TLS 1.1 需要更新。 请参阅[更新以在 Windows Server 2008 SP2 中添加对 TLS 1.1 和 TLS 1.2 的支持](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s)。 |
| Windows Vista | 不支持。 |

若要详细了解在每个版本的 Windows 上默认启用的 TLS/SSL 协议，请参阅 [TLS/SSL 中的协议 (Schannel SSP)](/windows/desktop/SecAuthN/protocols-in-tls-ssl--schannel-ssp-)。

**在 .NET Framework 3.5 中支持 TLS 1.2 的要求**

下表显示在 .NET Framework 3.5 中支持 TLS 1.2 所需的操作系统更新。 我们建议你应用所有操作系统更新。

| 操作系统  | **.NET Framework 3.5 中支持 TLS 1.2 所需的最低更新** |
| --- | --- |
| Windows 10<br>Windows 2016 Server | [Windows 10 版本 1511 和 Windows Server 2016 Technical Preview 4 的累积更新：2016 年 5 月 10 日](https://support.microsoft.com/help/3156421/cumulative-update-for-windows-10-version-1511-and-windows-server-2016) |
| Windows 8.1<br>Windows Server 2012 R2 | [在 Windows 8.1 和 Windows Server 2012 R2 上的 .NET Framework 3.5 中包含对 TLS 系统默认版本的支持](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows 8.0<br>Windows Server 2012 | [在 Windows Server 2012 R2 上的 .NET Framework 3.5 中包含对 TLS 系统默认版本的支持](https://support.microsoft.com/help/3154519/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows 7 SP1<br>Windows Server 2008 R2 SP1 | [在 Windows 7 SP1 和 Server 2008 R2 SP1 上的 .NET Framework 3.5.1 中包含对 TLS 系统默认版本的支持](https://support.microsoft.com/help/3154518/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows Server 2008 | [在 Windows Vista SP2 和 Server 2008 SP2 上的 .NET Framework 2.0 SP2 中包含对 TLS 系统默认版本的支持](https://support.microsoft.com/help/3154517/support-for-tls-system-default-versions-included-in-the--net-framework) |
| Windows Vista | 不支持 |
