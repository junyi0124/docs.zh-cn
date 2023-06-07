---
title: .NET Core 和 .NET 5 中的跨平台加密
description: 了解 .NET 支持的平台上的加密功能。
ms.date: 06/19/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- cryptography, cross-platform
- encryption, cross-platform
ms.openlocfilehash: 7269b32e509039fdd767446bd6e10202b089c094
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90550014"
---
# <a name="cross-platform-cryptography-in-net-core-and-net-5"></a>.NET Core 和 .NET 5 中的跨平台加密

.NET Core 和 .NET 5 中的加密操作由操作系统 (操作系统) 库完成。 此依赖项具有以下优势：

* .NET 应用从操作系统可靠性中获益。 为操作系统供应商提供了高优先级的安全加密库，使其免受漏洞的侵害。 为此，它们提供了系统管理员应应用的更新。
* 如果 OS 库是 FIPS 验证的，则 .NET 应用有权访问经过 FIPS 验证的算法。

对 OS 库的依赖也意味着，.NET 应用只能使用操作系统支持的加密功能。 虽然所有平台都支持某些核心功能，但 .NET 支持的某些功能无法在某些平台上使用。 本文列出了每个平台支持的功能。

本文假设你已熟悉如何在 .NET 中进行加密。 有关详细信息，请参阅 [.Net 加密模型](cryptography-model.md) 和 [.net 加密服务](cryptographic-services.md)。

## <a name="hash-algorithms"></a>哈希算法

所有哈希算法和基于哈希的消息身份验证 (HMAC) 类（包括 `*Managed` 类）遵从操作系统库。 尽管各种 OS 库的性能有所不同，但它们应该是兼容的。

## <a name="symmetric-encryption"></a>对称加密

基本的密码和链接由系统库完成，所有平台都支持所有这些操作。

| 密码 + 模式 | Windows | Linux | macOS |
|---------------|---------|-------|-------|
| AES-CBC       | ✔️     | ✔️    | ✔️   |
| AES-ECB       | ✔️     | ✔️    | ✔️   |
| 3DES-CBC      | ✔️     | ✔️    | ✔️   |
| 3DES-ECB      | ✔️     | ✔️    | ✔️   |
| DES-CBC       | ✔️     | ✔️    | ✔️   |
| DES-ECB       | ✔️     | ✔️    | ✔️   |

## <a name="authenticated-encryption"></a>经过身份验证的加密

经过身份验证的加密 (通过和类为 AES-CCM 和 AES 提供) 支持 <xref:System.Security.Cryptography.AesCcm?displayProperty=fullName> <xref:System.Security.Cryptography.AesGcm?displayProperty=fullName> 。

在 Windows 和 Linux 上，由 OS 库提供 AES-CCM 和 AES GCM 的实现。

### <a name="aes-ccm-and-aes-gcm-on-macos"></a>MacOS 上的 AES-CCM 和 AES-GCM

在 macOS 上，系统库不支持第三方代码的 AES-CCM 或 AES-GCM，因此 <xref:System.Security.Cryptography.AesCcm> 和 <xref:System.Security.Cryptography.AesGcm> 类使用 OpenSSL 来提供支持。 MacOS 上的用户需要获取 OpenSSL (libcrypto) 的相应副本，这些类型才能正常工作，并且它必须位于系统默认加载库的路径中。 建议从包管理器（例如 Homebrew）安装 OpenSSL。

`libcrypto.0.9.7.dylib` `libcrypto.0.9.8.dylib` MacOS 中包含的和库来自早期版本的 OpenSSL，不会使用。 `libcrypto.35.dylib`、 `libcrypto.41.dylib` 和 `libcrypto.42.dylib` 库来自 LibreSSL，不会使用。

### <a name="aes-ccm-keys-nonces-and-tags"></a>AES-CCM 密钥、nonce 和标记

* 密钥大小

  AES-CCM 适用于128、192和256位密钥。

* Nonce 大小

  <xref:System.Security.Cryptography.AesCcm>类支持56、64、72、80、88、96和 104 (7、8、9、10、11、12和13字节) nonce。

* 标记大小

  <xref:System.Security.Cryptography.AesCcm>类支持创建或处理32、48、64、80、96、112和 128 (4、8、10、12、14和16字节) 标记。

### <a name="aes-gcm-keys-nonces-and-tags"></a>AES-GCM 密钥、nonce 和标记

* 密钥大小

  AES-GCM 适用于128、192和256位密钥。

* Nonce 大小

  <xref:System.Security.Cryptography.AesGcm>类仅支持96位 (12 字节的) nonce。

* 标记大小

  <xref:System.Security.Cryptography.AesGcm>类支持创建或处理96、104、112、120和 128 (12、13、14、15和16字节的) 标记。

## <a name="asymmetric-cryptography"></a>非对称加密

本部分包括以下小节：

* [RSA](#rsa)
* [ECDSA](#ecdsa)
* [ECDH](#ecdh)
* [DSA](#dsa)

### <a name="rsa"></a>RSA

RSA (Rivest – Rivest-shamir-adleman – Rivest-shamir-adleman) 密钥生成由 OS 库执行，并受其大小限制和性能特征的限制。

RSA 密钥操作由 OS 库执行，可以加载的密钥类型受操作系统要求的限制。

.NET 不会 (未填充) RSA 操作公开 "原始"。

OS 库用于加密和解密填充。 并非所有平台都支持相同的填充选项：

| 填充模式                          | Windows (CNG)  | Linux (OpenSSL)  | macOS | Windows (CAPI)  |
|---------------------------------------|---------------|-----------------|-------|----------------|
| PKCS1 加密                      | ✔️           | ✔️              | ✔️   | ✔️             |
| OAEP-SHA-1                          | ✔️           | ✔️              | ✔️   | ✔️             |
| OAEP-SHA-1 (SHA256、SHA384、SHA512)  | ✔️           | ✔️              | ✔️   | ❌             |
| PKCS1 签名 (MD5，SHA-1)           | ✔️           | ✔️              | ✔️   | ✔️             |
| PKCS1 签名 (SHA-1)                | ✔️           | ✔️              | ✔️   | ⚠️\*           |
| PSS                                   | ✔️           | ✔️              | ✔️   | ❌             |

\* Windows CryptoAPI (CAPI) 能够具有 SHA-1 算法的 PKCS1 签名。 但可以在加密服务提供程序中加载单个 RSA 对象 (CSP) 不支持该对象。

#### <a name="rsa-on-windows"></a>Windows 上的 RSA

* 使用时，将使用 Windows CryptoAPI (CAPI) [`new RSACryptoServiceProvider()`](xref:System.Security.Cryptography.RSACryptoServiceProvider) 。
* 使用时，Windows 加密 API 下一代 (CNG) 使用 [`new RSACng()`](xref:System.Security.Cryptography.RSACng) 。
* 返回的对象由 <xref:System.Security.Cryptography.RSA.Create%2A?displayProperty=nameWithType> WINDOWS CNG 在内部提供支持。 Windows CNG 的这种用法是实现详细信息，可能会有所更改。
* 的 <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey%2A> 扩展方法 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 返回 <xref:System.Security.Cryptography.RSACng> 实例。 的这种用法 <xref:System.Security.Cryptography.RSACng> 是实现详细信息，可能会有所更改。
* <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey%2A> <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 当前首选实例的扩展方法 <xref:System.Security.Cryptography.RSACng> ，但如果 <xref:System.Security.Cryptography.RSACng> 无法打开密钥，则 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 会尝试。 首选提供程序是实现详细信息，可能会发生更改。

#### <a name="rsa-native-interop"></a>RSA 本机互操作

.NET 公开了各种类型，使程序能够与 .NET 加密代码所使用的 OS 库进行互操作。 涉及的类型不在平台之间转换，只应在必要时直接使用。

| 类型                                                         | Windows | Linux         | macOS         |
|--------------------------------------------------------------|---------|---------------|---------------|
| <xref:System.Security.Cryptography.RSACryptoServiceProvider> | ✔️     | ⚠️<sup>1</sup>| ⚠️<sup>1</sup> |
| <xref:System.Security.Cryptography.RSACng>                   | ✔️     | ❌            | ❌            |
| <xref:System.Security.Cryptography.RSAOpenSsl>               | ❌     | ✔️            | ⚠️<sup>pps-2</sup> |

<sup>1</sup> 在 MacOS 和 Linux 上， <xref:System.Security.Cryptography.RSACryptoServiceProvider> 可用于与现有程序的兼容性。 在这种情况下，任何需要 OS 互操作的方法（如打开已命名的密钥）都将引发 <xref:System.PlatformNotSupportedException> 。

<sup>2</sup> 在 macOS 上， <xref:System.Security.Cryptography.RSAOpenSsl> 适用于是否安装了 OpenSSL，并可通过动态库加载找到合适的 libcrypto .dylib。 如果找不到合适的库，则会引发异常。

### <a name="ecdsa"></a>ECDSA

ECDSA (椭圆曲线数字签名算法) 密钥生成由 OS 库完成，并受其大小限制和性能特征的限制。

ECDSA 关键曲线由 OS 库定义，并受其限制的限制。

| 椭圆曲线                     | Windows 10    | Windows 7-8。1 | Linux         | macOS         |
|------------------------------------|---------------|-----------------|---------------|---------------|
| NIST P-256 (secp256r1)              | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-384 (secp384r1)              | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-521 (secp521r1)              | ✔️           | ✔️              | ✔️           | ✔️            |
| Brainpool 曲线 (命名曲线)  | ✔️           | ❌              | ⚠️<sup>1</sup>| ❌           |
| 其他命名曲线                 | ⚠️<sup>pps-2</sup>| ❌             | ⚠️<sup>1</sup>| ❌           |
| 显式曲线                    | ✔️           | ❌              | ✔️           | ❌            |
| 作为显式导出或导入       | ✔️           | ❌<sup>三维空间</sup>  | ✔️           | ❌<sup>三维空间</sup>|

<sup>1</sup> Linux 分发版并不都支持相同的命名曲线。

<sup>2</sup> 向 windows 10 中的 windows CNG 添加了对命名曲线的支持。 有关详细信息，请参阅 [CNG 命名椭圆曲线](/windows/win32/seccng/cng-named-elliptic-curves)。 命名曲线在 Windows 的早期版本中不可用，但 Windows 7 中的三条曲线除外。

<sup>3</sup> 使用显式曲线参数导出需要 OS 库支持，该支持在 macOS 或早期版本的 Windows 上不可用。

#### <a name="ecdsa-native-interop"></a>ECDSA 本机互操作

.NET 公开了各种类型，使程序能够与 .NET 加密代码所使用的 OS 库进行互操作。 涉及的类型不在平台之间转换，只应在必要时直接使用。

| 类型                                             | Windows | Linux | macOS |
|--------------------------------------------------|---------|-------|-------|
| <xref:System.Security.Cryptography.ECDsaCng>     | ✔️     | ❌    | ❌    |
| <xref:System.Security.Cryptography.ECDsaOpenSsl> | ❌     | ✔️    | ⚠️\*  |

\* 在 macOS 上， <xref:System.Security.Cryptography.ECDsaOpenSsl> 如果在系统中安装了 OpenSSL 并且可以通过动态库加载找到合适的 libcrypto .dylib，则有效。 如果找不到合适的库，则会引发异常。

### <a name="ecdh"></a>ECDH

ECDH (椭圆曲线 Diffie-hellman) 密钥生成由 OS 库完成，并受其大小限制和性能特征的限制。

<xref:System.Security.Cryptography.ECDiffieHellman>类不返回 ECDH 计算的 "原始" 值。 所有返回的数据都是从密钥派生函数中进行的：

*  (Z) 哈希
* 哈希 (预置 | |Z | |追加) 
* HMAC (键，Z) 
* HMAC (键，预置 | |Z | |追加) 
* HMAC (Z，Z) 
* HMAC (Z，预置 | |Z | |追加) 
* Tls11Prf (标签，种子) 

ECDH 关键曲线由 OS 库定义，并受其限制的限制。

| 椭圆曲线                     | Windows 10    | Windows 7-8。1 | Linux         | macOS         |
|------------------------------------|---------------|-----------------|---------------|---------------|
| NIST P-256 (secp256r1)              | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-384 (secp384r1)              | ✔️           | ✔️              | ✔️           | ✔️            |
| NIST P-521 (secp521r1)              | ✔️           | ✔️              | ✔️           | ✔️            |
| brainpool 曲线 (命名曲线)  | ✔️           | ❌              | ⚠️<sup>1</sup>| ❌           |
| 其他命名曲线                 | ⚠️<sup>pps-2</sup>| ❌             | ⚠️<sup>1</sup>| ❌           |
| 显式曲线                    | ✔️           | ❌              | ✔️           | ❌            |
| 作为显式导出或导入       | ✔️           | ❌<sup>三维空间</sup>  | ✔️           | ❌<sup>三维空间</sup>|

<sup>1</sup> Linux 分发版并不都支持相同的命名曲线。

<sup>2</sup> 向 windows 10 中的 windows CNG 添加了对命名曲线的支持。 有关详细信息，请参阅 [CNG 命名椭圆曲线](/windows/win32/seccng/cng-named-elliptic-curves)。 命名曲线在 Windows 的早期版本中不可用，但 Windows 7 中的三条曲线除外。

<sup>3</sup> 使用显式曲线参数导出需要 OS 库支持，该支持在 macOS 或早期版本的 Windows 上不可用。

#### <a name="ecdh-native-interop"></a>ECDH 本机互操作

.NET 公开了类型，使程序能够与 .NET 所使用的 OS 库进行互操作。 涉及的类型不在平台之间转换，只应在必要时直接使用。

| 类型                                                       | Windows | Linux | macOS |
|------------------------------------------------------------|---------|-------|-------|
| <xref:System.Security.Cryptography.ECDiffieHellmanCng>     | ✔️     | ❌    | ❌   |
| <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl> | ❌     | ✔️    | ⚠️\* |

\* 在 macOS 上， <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl> 如果已安装 OpenSSL 并可通过动态库加载找到合适的 libcrypto .dylib，则有效。 如果找不到合适的库，则会引发异常。

### <a name="dsa"></a>DSA

DSA (数字签名算法) 密钥生成由系统库执行，并受其大小限制和性能特征的限制。

| 函数                      | Windows CNG | Linux | macOS         | Windows CAPI |
|-------------------------------|-------------|-------|---------------|--------------|
| 密钥创建 ( # B0 = 1024 位)    | ✔️         | ✔️    | ❌            | ✔️           |
| 密钥创建 ( # A0 1024 位)     | ✔️         | ✔️    | ❌            | ❌            |
|  ( # B0 = 1024 bits 加载密钥)    | ✔️         | ✔️    | ✔️            | ✔️           |
| 加载密钥 ( # A0 1024 位)     | ✔️         | ✔️    | ⚠️\*          | ❌            |
| FIPS 186-2                    | ✔️         | ✔️    | ✔️            | ✔️           |
| FIPS 186-3 (SHA-1 签名)  | ✔️         | ✔️    | ❌            | ❌            |

\* macOS 加载大于1024位的 DSA 密钥，但这些密钥的行为未定义。 它们不会按照 FIPS 186-3 的方式运行。

#### <a name="dsa-on-windows"></a>Windows 上的 DSA

* 使用时，将使用 Windows CryptoAPI (CAPI) [`new DSACryptoServiceProvider()`](xref:System.Security.Cryptography.DSACryptoServiceProvider) 。
* 使用时，Windows 加密 API 下一代 (CNG) 使用 [`new DSACng()`](xref:System.Security.Cryptography.DSACng) 。
* 返回的对象由 <xref:System.Security.Cryptography.DSA.Create%2A?displayProperty=nameWithType> WINDOWS CNG 在内部提供支持。 Windows CNG 的这种用法是实现详细信息，可能会有所更改。
* 的 <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPublicKey%2A> 扩展方法用于 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 返回 <xref:System.Security.Cryptography.DSACng> 实例。 的这种用法 <xref:System.Security.Cryptography.DSACng> 是实现详细信息，可能会有所更改。
* <xref:System.Security.Cryptography.X509Certificates.DSACertificateExtensions.GetDSAPrivateKey%2A>用于 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 首选实例的扩展方法 <xref:System.Security.Cryptography.DSACng> ，但如果 <xref:System.Security.Cryptography.DSACng> 不能打开该密钥，则 <xref:System.Security.Cryptography.DSACryptoServiceProvider> 会尝试。  首选提供程序是实现详细信息，可能会发生更改。

#### <a name="dsa-native-interop"></a>DSA 本机互操作

.NET 公开了各种类型，使程序能够与 .NET 加密代码所使用的 OS 库进行互操作。 涉及的类型不在平台之间转换，只应在必要时直接使用。

| 类型                                                         | Windows | Linux         | macOS         |
|--------------------------------------------------------------|---------|---------------|---------------|
| <xref:System.Security.Cryptography.DSACryptoServiceProvider> | ✔️     | ⚠️<sup>1</sup> | ⚠️<sup>1</sup> |
| <xref:System.Security.Cryptography.DSACng>                   | ✔️     | ❌             | ❌            |
| <xref:System.Security.Cryptography.DSAOpenSsl>               | ❌      | ✔️            | ⚠️<sup>pps-2</sup> |

<sup>1</sup> 在 MacOS 和 Linux 上， <xref:System.Security.Cryptography.DSACryptoServiceProvider> 可用于与现有程序的兼容性。 在这种情况下，任何需要系统互操作的方法（如打开已命名的密钥）都将引发 <xref:System.PlatformNotSupportedException> 。

<sup>2</sup> 在 macOS 上， <xref:System.Security.Cryptography.DSAOpenSsl> 适用于是否安装了 OpenSSL，并可通过动态库加载找到合适的 libcrypto .dylib。 如果找不到合适的库，则会引发异常。

## <a name="x509-certificates"></a>X.509 证书

对于 .NET 中的 x.509 证书，大多数支持都来自操作系统库。 若要将证书加载到 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> <xref:System.Security.Cryptography.X509Certificates.X509Certificate> .net 中的或实例，证书必须由基础 OS 库加载。

### <a name="read-a-pkcs12pfx"></a>读取 PKCS12/PFX

| 方案                                     | Windows | Linux | macOS |
|----------------------------------------------|---------|-------|-------|
| 空                                        | ✔️     | ✔️    | ✔️   |
| 一个证书，无私钥              | ✔️     | ✔️    | ✔️   |
| 一个证书，带私钥            | ✔️     | ✔️    | ✔️   |
| 多个证书，无私钥       | ✔️     | ✔️    | ✔️   |
| 多个证书，一个私钥       | ✔️     | ✔️    | ✔️   |
| 多个证书，多个私钥 | ✔️     | ⚠️\*  | ✔️   |

\* 在 .NET 5 预览版本中可用。

### <a name="write-a-pkcs12pfx"></a>编写 PKCS12/PFX

| 方案                                     | Windows | Linux | macOS |
|----------------------------------------------|---------|-------|-------|
| 空                                        | ✔️     | ✔️    | ⚠️\* |
| 一个证书，无私钥              | ✔️     | ✔️    | ⚠️\* |
| 一个证书，带私钥            | ✔️     | ✔️    | ✔️   |
| 多个证书，无私钥       | ✔️     | ✔️    | ⚠️\* |
| 多个证书，一个私钥       | ✔️     | ✔️    | ✔️   |
| 多个证书，多个私钥 | ✔️     | ⚠️\*  | ✔️   |
| 暂时加载                            | ✔️     | ✔️    | ⚠️\* |

\* 在 .NET 5 预览版本中可用。

macOS 无法加载没有密钥链对象的证书私钥，这需要写入磁盘。 密钥链是自动创建的，用于 PFX 加载，在不再使用时被删除。 由于 <xref:System.Security.Cryptography.X509Certificates.X509KeyStorageFlags.EphemeralKeySet?displayProperty=nameWithType> 选项意味着不应将私钥写入磁盘，因此在 macOS 上断言该标志会导致 <xref:System.PlatformNotSupportedException> 。

### <a name="write-a-pkcs7-certificate-collection"></a>写入 PKCS7 证书集合

Windows 和 Linux 都发出 DER 编码的 PKCS7 blob。 macOS 发出无限长度-CER 编码的 PKCS7 blob。

### <a name="x509store"></a>X509Store

在 Windows 上， <xref:System.Security.Cryptography.X509Certificates.X509Store> 类是 Windows 证书存储区 api 的表示形式。 这些 Api 在 .NET Core 和 .NET 5 中的工作方式与在 .NET Framework 中的工作方式相同。

在 Linux 上， <xref:System.Security.Cryptography.X509Certificates.X509Store> 类是系统信任决策的预测 (只读) 、用户信任决策 (读写) ，以及用户密钥存储 (读写) 。

在 macOS 上， <xref:System.Security.Cryptography.X509Certificates.X509Store> 类是系统信任决策的投影 (只读) 、用户信任决策 (只读) ，以及用户密钥存储 (读写) 。

下表显示了每个平台支持的方案。 对于表)  (不受支持 ❌ 的方案， <xref:System.Security.Cryptography.CryptographicException> 将引发。

#### <a name="the-my-store"></a>我的应用商店

| 方案                                         | Windows | Linux | macOS |
|--------------------------------------------------|---------|-------|-------|
| 打开 CurrentUser\My (ReadOnly)                    | ✔️     | ✔️    | ✔️   |
| 打开 CurrentUser\My (ReadWrite)                   | ✔️     | ✔️    | ✔️   |
| 打开 CurrentUser\My (ExistingOnly)                | ✔️     | ⚠️    | ✔️   |
| 打开 LocalMachine\My                             | ✔️     | ❌    | ✔️   |

在 Linux 上，存储是在首次写入时创建的，默认情况下不存在用户存储区，因此打开 `CurrentUser\My` 时 `ExistingOnly` 可能会失败。

在 macOS 上， `CurrentUser\My` 存储是用户的默认密钥链， `login.keychain` 默认情况下为。 `LocalMachine\My`存储区是 `System.keychain` 。

#### <a name="the-root-store"></a>根存储区

| 方案                              | Windows | Linux | macOS           |
|---------------------------------------|---------|-------|-----------------|
| 打开 CurrentUser\Root (ReadOnly)       | ✔️     | ✔️    | ✔️             |
| 打开 CurrentUser\Root (ReadWrite)      | ✔️     | ✔️    | ❌              |
| 打开 CurrentUser\Root (ExistingOnly)   | ✔️     | ⚠️    | 如果为 ReadOnly，则✔️ ()  |
| 打开 LocalMachine\Root (ReadOnly)      | ✔️     | ✔️    | ✔️             |
| 打开 LocalMachine\Root (ReadWrite)     | ✔️     | ❌    | ❌              |
| 打开 LocalMachine\Root (ExistingOnly)  | ✔️     | ⚠️    | 如果为 ReadOnly，则✔️ ()  |

在 Linux 上， `LocalMachine\Root` 存储是 OpenSSL 的默认路径中的 CA 捆绑包的解释。

在 macOS 上， `CurrentUser\Root` 存储是 `SecTrustSettings` 用户信任域结果的解释。 `LocalMachine\Root`存储是对 `SecTrustSettings` 管理员和系统信任域的结果的解释。

#### <a name="the-intermediate-store"></a>中间存储

| 方案                                      | Windows | Linux | macOS           |
|-----------------------------------------------|---------|-------|-----------------|
| 打开 CurrentUser\Intermediate (ReadOnly)       | ✔️     | ✔️    | ✔️             |
| 打开 CurrentUser\Intermediate (ReadWrite)      | ✔️     | ✔️    | ❌              |
| 打开 CurrentUser\Intermediate (ExistingOnly)   | ✔️     | ⚠️    | 如果为 ReadOnly，则✔️ ()  |
| 打开 LocalMachine\Intermediate (ReadOnly)      | ✔️     | ✔️    | ✔️             |
| 打开 LocalMachine\Intermediate (ReadWrite)     | ✔️     | ❌    | ❌              |
| 打开 LocalMachine\Intermediate (ExistingOnly)  | ✔️     | ⚠️    | 如果为 ReadOnly，则✔️ ()  |

在 Linux 上， `CurrentUser\Intermediate` 当通过其授权信息访问记录在成功的 X509Chain 生成时，将存储用作缓存。 该 `LocalMachine\Intermediate` 存储是 OpenSSL 的默认路径中的 CA 捆绑包的解释。

#### <a name="the-disallowed-store"></a>禁止存储

| 方案                                    | Windows | Linux | macOS           |
|---------------------------------------------|---------|-------|-----------------|
| 打开 CurrentUser\Disallowed (ReadOnly)       | ✔️     | ⚠️    | ✔️             |
| 打开 CurrentUser\Disallowed (ReadWrite)      | ✔️     | ⚠️    | ❌              |
| 打开 CurrentUser\Disallowed (ExistingOnly)   | ✔️     | ⚠️    | 如果为 ReadOnly，则✔️ ()  |
| 打开 LocalMachine\Disallowed (ReadOnly)      | ✔️     | ❌    | ✔️             |
| 打开 LocalMachine\Disallowed (ReadWrite)     | ✔️     | ❌    | ❌              |
| 打开 LocalMachine\Disallowed (ExistingOnly)  | ✔️     | ❌    | 如果为 ReadOnly，则✔️ ()  |

在 Linux 上， `Disallowed` 不在链生成中使用该存储区，尝试向其添加内容会导致 <xref:System.Security.Cryptography.CryptographicException> 。 <xref:System.Security.Cryptography.CryptographicException> `Disallowed` 如果存储区已经获取内容，则会引发。

在 macOS 上，CurrentUser\Disallowed 和 LocalMachine\Disallowed 存储是对其信任设置为的证书的相应 SecTrustSettings 结果的解释 `Always Deny` 。

#### <a name="nonexistent-store"></a>不存在的存储

| 方案                                         | Windows | Linux | macOS |
|--------------------------------------------------|---------|-------|-------|
| 打开不存在的存储 (ExistingOnly)            | ❌     | ❌     | ❌    |
|  (ReadWrite 打开 CurrentUser 不存在的存储)   | ✔️     | ✔️     | ⚠️   |
| 打开 LocalMachine 不存在的存储 (ReadWrite)  | ✔️     | ❌     | ❌    |

在 macOS 上，只支持在位置创建具有 X509Store API 的自定义存储 `CurrentUser` 。 它将创建一个新的密钥链，在用户的密钥链目录中没有密码 (*~/Library/Keychains*) 。 若要创建具有密码的密钥链，可以使用 P/Invoke `SecKeychainCreate` 。 同样，可 `SecKeychainOpen` 用于在不同位置打开密钥链。 `IntPtr`可以将生成的传递给 [`new X509Store(IntPtr)`](xref:System.Security.Cryptography.X509Certificates.X509Store.%23ctor(System.IntPtr)) ，以获取支持读写存储，受限于当前用户的权限。

### <a name="x509chain"></a>X509Chain

macOS 不支持脱机 CRL 使用率，因此 `X509RevocationMode.Offline` 将被视为 `X509RevocationMode.Online` 。

macOS 不支持 CRL 上用户启动的超时 (证书吊销列表) /OCSP (联机证书状态协议) /AIA (授权信息访问) 下载，因此 `X509ChainPolicy.UrlRetrievalTimeout` 会被忽略。

## <a name="additional-resources"></a>其他资源

* [.NET 加密模型](cryptography-model.md)
* [.NET 加密服务](cryptographic-services.md)
* [使用填充对 CBC 模式对称解密的漏洞进行计时](vulnerabilities-cbc-mode.md)
* [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
