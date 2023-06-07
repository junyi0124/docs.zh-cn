---
ms.openlocfilehash: 877f9d99b660c4af843e4d8d525219c1df6c99a9
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2020
ms.locfileid: "83720913"
---
### <a name="net-core-30-prefers-openssl-11x-to-openssl-10x"></a>.NET Core 3.0 倾向于使用 OpenSSL 1.1.x 而不是 OpenSSL 1.0.x

跨多个 Linux 分发工作的适用于 Linux 的 .NET Core 可同时支持 OpenSSL 1.0.x 和 OpenSSL 1.1.x。  .NET Core 2.1 和 .NET Core 2.2 首先查找 1.0.x，然后回退到 1.1.x；.NET Core 3.0 首先查找 1.1.x。 进行此更改是为了增加对新加密标准的支持。

此更改可能会影响库或应用程序，这些库或应用程序与 .NET Core 中的 OpenSSL 特定互操作类型进行平台互操作。

#### <a name="change-description"></a>更改描述

在 .NET Core 2.2 及更早版本中，运行时倾向于加载 OpenSSL 1.0.x 而不是加载 1.1.x。 这意味着，与 OpenSSL 互操作的 <xref:System.IntPtr> 和 <xref:System.Runtime.InteropServices.SafeHandle> 类型倾向于与 libcrypto.so.1.0.0 / libcrypto.so.1.0 / libcrypto.so.10 一起使用。

从 .NET Core 3.0 开始，运行时倾向于加载 OpenSSL 1.1.x 而不是 OpenSSL 1.0.x，因此与 OpenSSL 互操作的 <xref:System.IntPtr> 和 <xref:System.Runtime.InteropServices.SafeHandle> 类型倾向于与 libcrypto.so.1.1 / libcrypto.so.11 / libcrypto.so.1.1.0 / libcrypto.so.1.1.1 一起使用。 因此，从 .NET Core 2.1 或 .NET Core 2.2 升级时，与 OpenSSL 直接互操作的库和应用程序的指针可能与 .NET Core 公开的值不兼容。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

需要小心使用直接与 OpenSSL 操作的库和应用程序，确保它们使用的 OpenSSL 版本与 .NET Core 运行时相同。

所有将 .NET Core 加密类型中的 <xref:System.IntPtr> 或 <xref:System.Runtime.InteropServices.SafeHandle> 值直接用于 OpenSSL 的库或应用程序都应当将其使用的库的版本与新的 <xref:System.Security.Cryptography.SafeEvpPKeyHandle.OpenSslVersion?displayProperty=nameWithType> 属性进行比较，以确保指针兼容。

#### <a name="category"></a>类别

密码

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.SafeEvpPKeyHandle.%23ctor%2A>
- <xref:System.Security.Cryptography.RSAOpenSsl.%23ctor(System.IntPtr)>
- <xref:System.Security.Cryptography.RSAOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)>
- <xref:System.Security.Cryptography.RSAOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.DSAOpenSsl.%23ctor(System.IntPtr)>
- <xref:System.Security.Cryptography.DSAOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)>
- <xref:System.Security.Cryptography.DSAOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDsaOpenSsl.%23ctor(System.IntPtr)>
- <xref:System.Security.Cryptography.ECDsaOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)>
- <xref:System.Security.Cryptography.ECDsaOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl.%23ctor(System.IntPtr)>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl.%23ctor(System.Security.Cryptography.SafeEvpPKeyHandle)>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl.DuplicateKeyHandle?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.X509Certificates.X509Certificate.Handle?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:System.Security.Cryptography.SafeEvpPKeyHandle.#ctor`
- `M:System.Security.Cryptography.RSAOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.RSAOpenSsl.#ctor(System.Security.Cryptography.SafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.RSAOpenSsl.DuplicateKeyHandle`
- `M:System.Security.Cryptography.DSAOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.DSAOpenSsl.#ctor(System.Security.Cryptography.SafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.DSAOpenSsl.DuplicateKeyHandle`
- `M:System.Security.Cryptography.ECDsaOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.ECDsaOpenSsl.#ctor(System.Security.CryptographySafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.ECDsaOpenSsl.DuplicateKeyHandle`
- `M:System.Security.Cryptography.ECDiffieHellmanOpenSsl.#ctor(System.IntPtr)`
- `M:System.Security.Cryptography.ECDiffieHellmanOpenSsl.#ctor(System.Security.Cryptography.SafeEvpPKeyHandle)`
- `M:System.Security.Cryptography.ECDiffieHellmanOpenSsl.DuplicateKeyHandle`
- `P:System.Security.Cryptography.X509Certificates.X509Certificate.Handle`

-->
