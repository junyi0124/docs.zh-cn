---
ms.openlocfilehash: a4476fbff572c004632153e5a98812c241efca57
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031978"
---
### <a name="openssl-versions-on-macos"></a>macOS 上的 OpenSSL 版本

对于 <xref:System.Security.Cryptography.AesCcm>、<xref:System.Security.Cryptography.AesGcm>、<xref:System.Security.Cryptography.DSAOpenSsl>、<xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl>、<xref:System.Security.Cryptography.ECDsaOpenSsl>、<xref:System.Security.Cryptography.RSAOpenSsl> 和 <xref:System.Security.Cryptography.SafeEvpPKeyHandle> 类型，macOS 上的 .NET Core 3.0 及更高版本的运行时现在首选 OpenSSL 1.1.x 版而非 OpenSSL 1.0.x 版。

.NET Core 2.1 运行时现在支持 OpenSSL 1.1.x 版本，但仍首选 OpenSSL 1.0.x 版。

#### <a name="change-description"></a>更改描述

以前，.NET Core 运行时在 macOS 上使用 OpenSSL 1.0.x 版处理与 OpenSSL 交互的类型。 最新的 OpenSSL 1.0.x 版 OpenSSL 1.0.2 现已不受支持。 若要在支持的 OpenSSL 版本上保留使用 OpenSSL 的类型，.NET Core 3.0 及更高版本的运行时现需在 macOS 上使用较新版本的 OpenSSL。

通过此更改，macOS 上的 .NET Core 运行时的行为如下所示：

- .NET Core 3.0 及更高版本运行时使用 OpenSSL 1.1.x（如果可用），并且仅在没有 1.1.x 版本可用的情况下才回退到 OpenSSL 1.0.x。

  对于将 OpenSSL 互操作类型与自定义 P/Invoke 一起使用的调用方，请按照 <xref:System.Security.Cryptography.SafeEvpPKeyHandle.OpenSslVersion?displayProperty=nameWithType> 注释中的指南进行操作。 如果不检查 <xref:System.Security.Cryptography.SafeEvpPKeyHandle.OpenSslVersion> 值，你的应用可能会出现故障。

- .NET Core 2.1 运行时使用 OpenSSL 1.0.x（如果可用），并且仅在没有 1.0.x 版本可用的情况下才回退到 OpenSSL 1.1.x。

  2\.1 运行时首选早期版本的 OpenSSL，因为 .NET Core 2.1 中不存在 <xref:System.Security.Cryptography.SafeEvpPKeyHandle.OpenSslVersion?displayProperty=nameWithType> 属性，因此无法在运行时可靠地确定 OpenSSL 版本。

#### <a name="version-introduced"></a>引入的版本

- .NET Core 2.1.16
- .NET Core 3.0.3
- .NET Core 3.1.2

#### <a name="recommended-action"></a>建议操作

- 如果不再需要 OpenSSL 版本 1.0.2，请将其卸载。

- 如果你使用 <xref:System.Security.Cryptography.AesCcm>、<xref:System.Security.Cryptography.AesGcm>、<xref:System.Security.Cryptography.DSAOpenSsl>、<xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl>、<xref:System.Security.Cryptography.ECDsaOpenSsl>、<xref:System.Security.Cryptography.RSAOpenSsl> 或 <xref:System.Security.Cryptography.SafeEvpPKeyHandle> 类型，请安装 OpenSSL 1.1.x。

- 如果你将 OpenSSL 互操作类型与自定义 P/Invoke 一起使用，请按照 <xref:System.Security.Cryptography.SafeEvpPKeyHandle.OpenSslVersion?displayProperty=nameWithType> 注释中的指南进行操作。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.AesCcm?displayProperty=fullName>
- <xref:System.Security.Cryptography.AesGcm?displayProperty=fullName>
- <xref:System.Security.Cryptography.DSAOpenSsl?displayProperty=fullName>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl?displayProperty=fullName>
- <xref:System.Security.Cryptography.ECDsaOpenSsl?displayProperty=fullName>
- <xref:System.Security.Cryptography.RSAOpenSsl?displayProperty=fullName>
- <xref:System.Security.Cryptography.SafeEvpPKeyHandle?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Security.Cryptography.AesCcm``
- `T:System.Security.Cryptography.AesGcm`
- `T:System.Security.Cryptography.DSAOpenSsl`
- `T:System.Security.Cryptography.ECDiffieHellmanOpenSsl`
- `T:System.Security.Cryptography.ECDsaOpenSsl`
- `T:System.Security.Cryptography.RSAOpenSsl`
- `T:System.Security.Cryptography.SafeEvpPKeyHandle`

-->
