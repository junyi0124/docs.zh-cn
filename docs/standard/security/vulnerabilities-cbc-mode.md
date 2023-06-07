---
title: CBC 解密漏洞
description: 了解如何使用密码块链 (CBC) 模式对称解密（使用填充）来检测和减少计时漏洞。
ms.date: 07/15/2020
author: blowdart
ms.openlocfilehash: e9bbeeefa2ef6ef90f3a752742667d30aec0a3da
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557198"
---
# <a name="timing-vulnerabilities-with-cbc-mode-symmetric-decryption-using-padding"></a>使用填充对 CBC 模式对称解密的漏洞进行计时

Microsoft 相信，如果应用了可验证的填充，则在应用可验证的填充时，不能安全地解密用密码块链加密的数据 (CBC)  此毋庸置疑基于当前已知的加密研究。

## <a name="introduction"></a>简介

填充 oracle 攻击是一种针对加密数据的攻击，攻击者可以利用这些数据来解密数据的内容，而无需知道密钥。

Oracle 指的是 "判断"，它向攻击者提供有关其执行操作是否正确的信息。 想象一下使用儿童玩棋盘或纸牌游戏。 当他们的脸看得很好，因为他们认为这是一种很好的搬迁，就是 oracle。 作为对手，你可以使用此 oracle 来计划下一次移动。

填充是特定的加密术语。 一些密码（用于对数据进行加密的算法）用于处理数据块，其中每个块都是固定大小的数据块。 如果要加密的数据不是填充块的正确大小，则会将数据填充到该数据块。 许多填充形式都需要始终显示填充，即使原始输入的大小正确也是如此。 这允许在解密时始终安全删除填充。

将这两个东西组合在一起，使用 oracle 填充的软件实现可以显示解密的数据是否具有有效的填充。 Oracle 可能是一个简单的操作，就像返回一个表示 "无效填充" 的值，或者使用显著不同的时间来处理有效的块而不是无效的块。

基于块的密码有另一个称为模式的属性，该属性确定第一个块中的数据与第二个块中的数据的关系等等。 最常使用的模式之一是 CBC。 CBC 引入了一个初始随机块（称为初始化向量） (IV) ，并将前一个块与静态加密的结果组合在一起，这样就可以使用相同的密钥来加密相同的消息，而不会始终产生相同的加密输出。

攻击者可以使用 oracle 填充方式与 CBC 数据的结构化方式，将略有变化的消息发送给公开 oracle 的代码，并持续发送数据，直到 oracle 向用户指示数据正确为止。 攻击者通过此响应，可以通过字节对消息进行解密。

新式计算机网络的质量很高，攻击者可检测到在远程系统上执行时间 (小于 0.1 ms) 。假设只有在数据未被篡改时才会发生成功解密的应用程序可能易受到旨在观察解密成功和不成功的工具的攻击。 虽然这种计时差异在某些语言或库中可能比其他一些语言或库更重要，但现在我们认为这是在应用程序发生故障时，所有语言和库的实际威胁。

此攻击依赖于更改加密数据和通过 oracle 测试结果的能力。 完全缓解攻击的唯一方法是检测对加密数据所做的更改，并拒绝对其执行任何操作。 执行此操作的标准方法是为数据创建签名，并在执行任何操作之前验证该签名。 签名必须是可验证的，不能由攻击者创建，否则他们将更改加密数据，然后基于更改的数据计算新的签名。 一种常见类型的适当签名称为 (HMAC) 的键控哈希消息身份验证代码。 HMAC 不同于校验和，因为它采用密钥，只对生成 HMAC 的人员和验证该密钥的人员非常熟悉。 如果不拥有密钥，则不能生成正确的 HMAC。 接收到数据时，需要使用加密数据，使用机密密钥和发件人共享来独立计算 HMAC，然后将它们发送的 HMAC 与计算出的 HMAC 进行比较。 此比较必须是固定时间，否则，你添加了另一个可检测的 oracle，允许不同类型的攻击。

总而言之，若要安全地使用填充的 CBC 块密码，你必须将它们与 HMAC (或另一个数据完整性检查) 进行合并，然后再尝试对数据进行解密。 由于所有更改的消息都需要相同的时间来生成响应，因此会阻止攻击。

## <a name="who-is-vulnerable"></a>谁有漏洞

此漏洞适用于正在执行其自己的加密和解密的托管和本机应用程序。 这包括：

- 一种应用程序，用于加密 cookie 以便以后在服务器上解密。
- 一种数据库应用程序，该应用程序使用户能够将数据插入到其列以后被解密的表中。
- 一种数据传输应用程序，它使用共享密钥来保护传输中的数据。
- 用于加密和解密 TLS 隧道中的消息的应用程序。

请注意，在这些情况下，不能使用 TLS。

易受攻击的应用程序：

- 使用具有可验证填充模式（如 PKCS # 7 或 ANSI 923）的 CBC 密码模式对数据进行解密。
- 执行解密，而无需通过 MAC 或非对称数字签名)  (执行数据完整性检查。

这也适用于基于这些基元在抽象之上构建的应用程序，例如 (PKCS # 7/CMS) EnvelopedData 结构的加密消息语法。

## <a name="related-areas-of-concern"></a>相关关注领域

研究使 Microsoft 进一步关注在消息具有已知或可预测的页脚结构时使用 ISO 10126 等效填充填充的 CBC 消息。 例如，在 W3C XML 加密语法和处理建议的规则下准备的内容将 (xmlenc，System.security.cryptography.xml.encryptedxml.encrypt) 。 虽然对消息进行签名的 W3C 指导，但在这种情况下，已将加密视为适当，Microsoft 现在建议始终执行加密-then-sign。

应用程序开发人员应始终警惕验证非对称签名密钥的适用性，因为非对称密钥与任意消息之间没有内在的信任关系。

## <a name="details"></a>详细信息

从历史上看，已达成共识，使用 HMAC 或 RSA 签名等方法对重要数据进行加密和身份验证至关重要。 不过，对于如何对加密和身份验证操作进行排序，没有明确的指导。 由于本文详细介绍了这一漏洞，Microsoft 指南现在始终使用 "加密-后号" 范例。 也就是说，首先使用对称密钥加密数据，然后通过密码文本 (加密数据) 计算 MAC 或非对称签名。 在对数据进行解密时，请执行反向。 首先，确认密码文本的 MAC 或签名，然后将其解密。

众所周知，一类称为 "需要填充 oracle 攻击" 的漏洞存在超过10年。 利用这些漏洞，攻击者可以对对称块算法加密的数据进行解密，如 AES 和3DES，使用每个数据块的次数不超过4096。 这些漏洞利用了这样一个事实：块密码最常用于结尾的可验证填充数据。 已发现，如果攻击者可以篡改密码文本，并找出篡改在结尾时是否导致了空白格式出错，则攻击者可以解密数据。

最初，实际攻击基于根据填充是否有效而返回不同错误代码的服务，如 ASP.NET 漏洞[MS10-070](/security-updates/SecurityBulletins/2010/ms10-070)。 但是，Microsoft 认为，只使用处理有效和无效填充之间的时间差异来执行类似的攻击是不切实际的。

如果加密方案使用了签名，并且使用固定的运行时对给定长度的数据执行签名验证 (不管) 的内容，则可以验证数据完整性，而不会通过[侧通道](https://en.wikipedia.org/wiki/Side-channel_attack)向攻击者发送任何信息。 由于完整性检查拒绝任何篡改的消息，因此会减轻 oracle 填充的威胁。

## <a name="guidance"></a>指南

首先，Microsoft 建议所有具有机密的数据都需要通过传输层安全传输 (TLS) 、安全套接字层 (SSL) 的后续版本。

接下来，分析你的应用程序以：

- 准确了解要执行的加密，以及所使用的平台和 Api 提供的加密。
- 请确保对称[块加密算法](https://en.wikipedia.org/wiki/Block_cipher#Notable_block_ciphers)的每个层（如 AES 和3des）在 CBC 模式下的每个使用都结合使用机密密钥数据完整性检查 (非对称签名、HMAC 或将密码模式改为[经过身份验证的加密](https://en.wikipedia.org/wiki/Authenticated_encryption) (AE) 模式，如 GCM 或 CCM) 。

基于当前研究，一般认为，当为非自动曝光模式单独执行身份验证和加密步骤时，对密码文本进行身份验证 (加密-sign) 是最佳的常规选项。 但是，没有一个大小合适的对加密的正确答案，此通用化并不像来自专业 cryptographer 的定向通知。

鼓励无法更改消息传送格式但执行未经身份验证的 CBC 解密的应用程序尝试合并缓解措施，例如：

- 解密时不允许解密器来验证或删除填充：
  - 仍需要删除或忽略应用的任何填充。这会将负担转移到应用程序中。
  - 优点是可以将填充验证和删除合并到其他应用程序数据验证逻辑中。 如果可以在固定时间内完成填充验证和数据验证，则会降低威胁。
  - 由于填充的解释会更改感知的消息长度，因此可能仍会从此方法发出时间信息。
- 将解密填充模式更改为 ISO10126：
  - ISO10126 解密填充与 PKCS7 加密填充和 ANSIX923 加密填充兼容。
  - 更改模式会将 "oracle 知识填充" 简化为1个字节，而不是整个块。 但是，如果内容具有已知的页脚（如结束 XML 元素），则相关攻击可以继续攻击消息的其余部分。
  - 这也不会阻止明文恢复，这种情况下，攻击者可使用不同的消息偏移量强制多次加密相同的纯文本。
- 指示对 dampen 计时信号的解密调用的评估：
  - 对于包含填充的任何数据段，保存时间的计算必须至少超过解密操作所需的最长时间。
  - 时间计算应按照[获取高分辨率时间戳](/windows/desktop/sysinfo/acquiring-high-resolution-time-stamps)的指导完成，而不是使用 <xref:System.Environment.TickCount?displayProperty=nameWithType> (受制于) 或减去两个系统时间戳， (受到 NTP 调整错误) 。
  - 时间计算必须包含解密操作，包括托管或 c + + 应用程序中的所有潜在异常，而不只是填充到末尾。
  - 如果已确定成功或失败，则计时入口需要在过期时返回故障。
- 执行未经身份验证的解密的服务应就地监视，以检测是否有大量的 "无效" 消息。
  - 请记住，此信号会携带误报 (合法损坏的数据) 和漏报， (在足够长时间内传播攻击，以规避检测) 。

## <a name="finding-vulnerable-code---native-applications"></a>查找易受攻击的代码-本机应用程序

对于基于 Windows 加密的程序：下一代 (CNG) 库：

- 解密调用为[BCryptDecrypt](/windows/desktop/api/bcrypt/nf-bcrypt-bcryptdecrypt)，并指定 `BCRYPT_BLOCK_PADDING` 标志。
- 已通过调用[BCryptSetProperty](/windows/desktop/api/bcrypt/nf-bcrypt-bcryptsetproperty)并将[BCRYPT_CHAINING_MODE](/windows/desktop/SecCNG/cng-property-identifiers#BCRYPT_CHAINING_MODE)设置为来初始化密钥句柄 `BCRYPT_CHAIN_MODE_CBC` 。
  - 由于 `BCRYPT_CHAIN_MODE_CBC` 是默认值，因此受影响的代码可能没有为指定任何值 `BCRYPT_CHAINING_MODE` 。

对于针对较旧的 Windows 加密 API 构建的程序：

- 解密调用将与[CryptDecrypt](/windows/desktop/api/wincrypt/nf-wincrypt-cryptdecrypt) `Final=TRUE` 。
- 已通过调用[CryptSetKeyParam](/windows/desktop/api/wincrypt/nf-wincrypt-cryptsetkeyparam)并将[KP_MODE](/windows/desktop/api/wincrypt/nf-wincrypt-cryptgetkeyparam)设置为来初始化密钥句柄 `CRYPT_MODE_CBC` 。
  - 由于 `CRYPT_MODE_CBC` 是默认值，因此受影响的代码可能没有为指定任何值 `KP_MODE` 。

## <a name="finding-vulnerable-code---managed-applications"></a>查找易受攻击的代码管理应用程序

- 解密调用 <xref:System.Security.Cryptography.SymmetricAlgorithm.CreateDecryptor> 针对的或 <xref:System.Security.Cryptography.SymmetricAlgorithm.CreateDecryptor(System.Byte[],System.Byte[])> 方法 <xref:System.Security.Cryptography.SymmetricAlgorithm?displayProperty=nameWithType> 。
  - 这包括 .NET 中的以下派生类型，但也可能包含第三方类型：
    - <xref:System.Security.Cryptography.Aes>
    - <xref:System.Security.Cryptography.AesCng>
    - <xref:System.Security.Cryptography.AesCryptoServiceProvider>
    - <xref:System.Security.Cryptography.AesManaged>
    - <xref:System.Security.Cryptography.DES>
    - <xref:System.Security.Cryptography.DESCryptoServiceProvider>
    - <xref:System.Security.Cryptography.RC2>
    - <xref:System.Security.Cryptography.RC2CryptoServiceProvider>
    - <xref:System.Security.Cryptography.Rijndael>
    - <xref:System.Security.Cryptography.RijndaelManaged>
    - <xref:System.Security.Cryptography.TripleDES>
    - <xref:System.Security.Cryptography.TripleDESCng>
    - <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider>
- <xref:System.Security.Cryptography.SymmetricAlgorithm.Padding?displayProperty=nameWithType>属性设置为 <xref:System.Security.Cryptography.PaddingMode.PKCS7?displayProperty=nameWithType> 、 <xref:System.Security.Cryptography.PaddingMode.ANSIX923?displayProperty=nameWithType> 或 <xref:System.Security.Cryptography.PaddingMode.ISO10126?displayProperty=nameWithType> 。
  - 由于 <xref:System.Security.Cryptography.PaddingMode.PKCS7?displayProperty=nameWithType> 是默认值，因此受影响的代码可能永远都不会分配 <xref:System.Security.Cryptography.SymmetricAlgorithm.Padding?displayProperty=nameWithType> 属性。
- <xref:System.Security.Cryptography.SymmetricAlgorithm.Mode?displayProperty=nameWithType>属性设置为<xref:System.Security.Cryptography.CipherMode.CBC?displayProperty=nameWithType>
  - 由于 <xref:System.Security.Cryptography.CipherMode.CBC?displayProperty=nameWithType> 是默认值，因此受影响的代码可能永远都不会分配 <xref:System.Security.Cryptography.SymmetricAlgorithm.Mode?displayProperty=nameWithType> 属性。

## <a name="finding-vulnerable-code---cryptographic-message-syntax"></a>查找易受攻击的代码加密消息语法

未经过身份验证的 CMS EnvelopedData 消息，其加密内容使用 AES (2.16.840.1.101.3.4.1.2、2.16.840.1.101.3.4.1.22、2.16.840.1.101.3.4.1.42) 、DES (1.3.14.3.2.7) 、3DES (1.2.840.113549.3.7) 或 RC2 (1.2.840.113549.3.2) 的 CBC 模式，以及使用 CBC 模式下的任何其他块加密算法的消息。

尽管流密码不容易受到此特定漏洞的影响，但 Microsoft 建议始终通过检查 ContentEncryptionAlgorithm 值对数据进行身份验证。

对于托管应用程序，可以使用传递给的任何值来检测 CMS EnvelopedData blob <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.Decode(System.Byte[])?displayProperty=fullName> 。

对于本机应用程序，可以通过[CryptMsgUpdate](/windows/desktop/api/wincrypt/nf-wincrypt-cryptmsgupdate)将 cms EnvelopedData blob 作为提供给 cms 句柄的任何值来检测，其生成[CMSG_TYPE_PARAM](/windows/desktop/api/wincrypt/nf-wincrypt-cryptmsggetparam)为 `CMSG_ENVELOPED` 和/或 CMS 句柄稍后 `CMSG_CTRL_DECRYPT` 通过[CryptMsgControl](/windows/desktop/api/wincrypt/nf-wincrypt-cryptmsgcontrol)发送指令。

## <a name="vulnerable-code-example---managed"></a>易受攻击的代码示例-托管

此方法读取 cookie 并对其进行解密，且不显示任何数据完整性检查。 因此，通过此方法读取的 cookie 的内容可能会受到接收该 cookie 的用户或已获得加密的 cookie 值的任何攻击者的攻击。

```csharp
private byte[] DecryptCookie(string cookieName)
{
    HttpCookie cookie = Request.Cookies[cookieName];

    if (cookie == null)
    {
        return null;
    }

    using (ICryptoTransform decryptor = _aes.CreateDecryptor())
    using (MemoryStream memoryStream = new MemoryStream())
    using (CryptoStream cryptoStream =
        new CryptoStream(memoryStream, decryptor, CryptoStreamMode.Write))
    {
        byte[] readCookie = Convert.FromBase64String(cookie.Value);
        cryptoStream.Write(readCookie, 0, readCookie.Length);
        cryptoStream.FlushFinalBlock();
        return memoryStream.ToArray();
    }
}
```

## <a name="example-code-following-recommended-practices---managed"></a>遵循建议做法的代码示例-托管

下面的示例代码使用非标准的消息格式

`cipher_algorithm_id || hmac_algorithm_id || hmac_tag || iv || ciphertext`

其中 `cipher_algorithm_id` ，和 `hmac_algorithm_id` 算法标识符为应用程序本地 (非标准) 这些算法的表示形式。 这些标识符可能在现有消息传送协议的其他部分中是有意义的，而不是作为一种连接 bytestream。

此示例还使用单个主密钥来派生加密密钥和 HMAC 密钥。 提供此方法是为了方便将单个密钥应用程序转换为双重密钥应用程序，并鼓励将两个密钥保留为不同的值。 它进一步保证 HMAC 密钥和加密密钥不会同步。

此示例并不接受 <xref:System.IO.Stream> 加密或解密。 由于 `hmac_tag` 值在密码文本之前，因此当前数据格式很难进行一次传递加密。 但是，这种格式已被选择，因为它会保留开头的所有固定大小的元素，以使分析器更简单。 使用此数据格式时，可以通过一次传递解密，但在调用 TransformFinalBlock 之前，实施者注意事项调用 GetHashAndReset 并验证结果。 如果流式加密非常重要，则可能需要不同的自动曝光模式。

```csharp
// ==++==
//
//   Copyright (c) Microsoft Corporation.  All rights reserved.
//
//   Shared under the terms of the Microsoft Public License,
//   https://opensource.org/licenses/MS-PL
//
// ==--==

using System;
using System.Diagnostics;
using System.IO;
using System.Runtime.CompilerServices;
using System.Security.Cryptography;

namespace Microsoft.Examples.Cryptography
{
    public enum AeCipher : byte
    {
        Unknown,
        Aes256CbcPkcs7,
    }

    public enum AeMac : byte
    {
        Unknown,
        HMACSHA256,
        HMACSHA384,
    }

    /// <summary>
    /// Provides extension methods to make HashAlgorithm look like .NET Core's
    /// IncrementalHash
    /// </summary>
    internal static class IncrementalHashExtensions
    {
        public static void AppendData(this HashAlgorithm hash, byte[] data)
        {
            hash.TransformBlock(data, 0, data.Length, null, 0);
        }

        public static void AppendData(
            this HashAlgorithm hash,
            byte[] data,
            int offset,
            int length)
        {
            hash.TransformBlock(data, offset, length, null, 0);
        }

        public static byte[] GetHashAndReset(this HashAlgorithm hash)
        {
            hash.TransformFinalBlock(Array.Empty<byte>(), 0, 0);
            return hash.Hash;
        }
    }

    public static partial class AuthenticatedEncryption
    {
        /// <summary>
        /// Use <paramref name="masterKey"/> to derive two keys (one cipher, one HMAC)
        /// to provide authenticated encryption for <paramref name="message"/>.
        /// </summary>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <param name="message">The message to encrypt</param>
        /// <returns>
        /// A concatenation of
        /// [cipher algorithm+chainmode+padding][mac algorithm][authtag][IV][ciphertext],
        /// suitable to be passed to <see cref="Decrypt"/>.
        /// </returns>
        /// <remarks>
        /// <paramref name="masterKey"/> should be a 128-bit (or bigger) value generated
        /// by a secure random number generator, such as the one returned from
        /// <see cref="RandomNumberGenerator.Create()"/>.
        /// This implementation chooses to block deficient inputs by length, but does not
        /// make any attempt at discerning the randomness of the key.
        ///
        /// If the master key is being input by a prompt (like a password/passphrase)
        /// then it should be properly turned into keying material via a Key Derivation
        /// Function like PBKDF2, represented by Rfc2898DeriveBytes. A 'password' should
        /// never be simply turned to bytes via an Encoding class and used as a key.
        /// </remarks>
        public static byte[] Encrypt(byte[] masterKey, byte[] message)
        {
            if (masterKey == null)
                throw new ArgumentNullException(nameof(masterKey));
            if (masterKey.Length < 16)
                throw new ArgumentOutOfRangeException(
                    nameof(masterKey),
                    "Master Key must be at least 128 bits (16 bytes)");
            if (message == null)
                throw new ArgumentNullException(nameof(message));

            // First, choose an encryption scheme.
            AeCipher aeCipher = AeCipher.Aes256CbcPkcs7;

            // Second, choose an authentication (message integrity) scheme.
            //
            // In this example we use the master key length to change from HMACSHA256 to
            // HMACSHA384, but that is completely arbitrary. This mostly represents a
            // "cryptographic needs change over time" scenario.
            AeMac aeMac = masterKey.Length < 48 ? AeMac.HMACSHA256 : AeMac.HMACSHA384;

            // It's good to be able to identify what choices were made when a message was
            // encrypted, so that the message can later be decrypted. This allows for
            // future versions to add support for new encryption schemes, but still be
            // able to read old data. A practice known as "cryptographic agility".
            //
            // This is similar in practice to PKCS#7 messaging, but this uses a
            // private-scoped byte rather than a public-scoped Object IDentifier (OID).
            // Please note that the scheme in this example adheres to no particular
            // standard, and is unlikely to survive to a more complete implementation in
            // the .NET Framework.
            //
            // You may be well-served by prepending a version number byte to this
            // message, but may want to avoid the value 0x30 (the leading byte value for
            // DER-encoded structures such as X.509 certificates and PKCS#7 messages).
            byte[] algorithmChoices = { (byte)aeCipher, (byte)aeMac };
            byte[] iv;
            byte[] cipherText;
            byte[] tag;

            // Using our algorithm choices, open an HMAC (as an authentication tag
            // generator) and a SymmetricAlgorithm which use different keys each derived
            // from the same master key.
            //
            // A custom implementation may very well have distinctly managed secret keys
            // for the MAC and cipher, this example merely demonstrates the master to
            // derived key methodology to encourage key separation from the MAC and
            // cipher keys.
            using (HMAC tagGenerator = GetMac(aeMac, masterKey))
            {
                using (SymmetricAlgorithm cipher = GetCipher(aeCipher, masterKey))
                using (ICryptoTransform encryptor = cipher.CreateEncryptor())
                {
                    // Since no IV was provided, a random one has been generated
                    // during the call to CreateEncryptor.
                    //
                    // But note that it only does the auto-generation once. If the cipher
                    // object were used again, a call to GenerateIV would have been
                    // required.
                    iv = cipher.IV;

                    cipherText = Transform(encryptor, message, 0, message.Length);
                }

                // The IV and ciphertest both need to be included in the MAC to prevent
                // tampering.
                //
                // By including the algorithm identifiers, we have technically moved from
                // simple Authenticated Encryption (AE) to Authenticated Encryption with
                // Additional Data (AEAD). By including the algorithm identifiers in the
                // MAC, it becomes harder for an attacker to change them as an attempt to
                // perform a downgrade attack.
                //
                // If you've added a data format version field, it can also be included
                // in the MAC to further inhibit an attacker's options for confusing the
                // data processor into believing the tampered message is valid.
                tagGenerator.AppendData(algorithmChoices);
                tagGenerator.AppendData(iv);
                tagGenerator.AppendData(cipherText);
                tag = tagGenerator.GetHashAndReset();
            }

            // Build the final result as the concatenation of everything except the keys.
            int totalLength =
                algorithmChoices.Length +
                tag.Length +
                iv.Length +
                cipherText.Length;

            byte[] output = new byte[totalLength];
            int outputOffset = 0;

            Append(algorithmChoices, output, ref outputOffset);
            Append(tag, output, ref outputOffset);
            Append(iv, output, ref outputOffset);
            Append(cipherText, output, ref outputOffset);

            Debug.Assert(outputOffset == output.Length);
            return output;
        }

        /// <summary>
        /// Reads a message produced by <see cref="Encrypt"/> after verifying it hasn't
        /// been tampered with.
        /// </summary>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <param name="cipherText">
        /// The output of <see cref="Encrypt"/>: a concatenation of a cipher ID, mac ID,
        /// authTag, IV, and cipherText.
        /// </param>
        /// <returns>The decrypted content.</returns>
        /// <exception cref="ArgumentNullException">
        /// <paramref name="masterKey"/> is <c>null</c>.
        /// </exception>
        /// <exception cref="ArgumentNullException">
        /// <paramref name="cipherText"/> is <c>null</c>.
        /// </exception>
        /// <exception cref="CryptographicException">
        /// <paramref name="cipherText"/> identifies unknown algorithms, is not long
        /// enough, fails a data integrity check, or fails to decrypt.
        /// </exception>
        /// <remarks>
        /// <paramref name="masterKey"/> should be a 128-bit (or larger) value
        /// generated by a secure random number generator, such as the one returned from
        /// <see cref="RandomNumberGenerator.Create()"/>. This implementation chooses to
        /// block deficient inputs by length, but doesn't make any attempt at
        /// discerning the randomness of the key.
        ///
        /// If the master key is being input by a prompt (like a password/passphrase),
        /// then it should be properly turned into keying material via a Key Derivation
        /// Function like PBKDF2, represented by Rfc2898DeriveBytes. A 'password' should
        /// never be simply turned to bytes via an Encoding class and used as a key.
        /// </remarks>
        public static byte[] Decrypt(byte[] masterKey, byte[] cipherText)
        {
            // This example continues the .NET practice of throwing exceptions for
            // failures. If you consider message tampering to be normal (and thus
            // "not exceptional") behavior, you may like the signature
            // bool Decrypt(byte[] messageKey, byte[] cipherText, out byte[] message)
            // better.
            if (masterKey == null)
                throw new ArgumentNullException(nameof(masterKey));
            if (masterKey.Length < 16)
                throw new ArgumentOutOfRangeException(
                    nameof(masterKey),
                    "Master Key must be at least 128 bits (16 bytes)");
            if (cipherText == null)
                throw new ArgumentNullException(nameof(cipherText));

            // The format of this message is assumed to be public, so there's no harm in
            // saying ahead of time that the message makes no sense.
            if (cipherText.Length < 2)
            {
                throw new CryptographicException();
            }

            // Use the message algorithm headers to determine what cipher algorithm and
            // MAC algorithm are going to be used. Since the same Key Derivation
            // Functions (KDFs) are being used in Decrypt as Encrypt, the keys are also
            // the same.
            AeCipher aeCipher = (AeCipher)cipherText[0];
            AeMac aeMac = (AeMac)cipherText[1];

            using (SymmetricAlgorithm cipher = GetCipher(aeCipher, masterKey))
            using (HMAC tagGenerator = GetMac(aeMac, masterKey))
            {
                int blockSizeInBytes = cipher.BlockSize / 8;
                int tagSizeInBytes = tagGenerator.HashSize / 8;
                int headerSizeInBytes = 2;
                int tagOffset = headerSizeInBytes;
                int ivOffset = tagOffset + tagSizeInBytes;
                int cipherTextOffset = ivOffset + blockSizeInBytes;
                int cipherTextLength = cipherText.Length - cipherTextOffset;
                int minLen = cipherTextOffset + blockSizeInBytes;

                // Again, the minimum length is still assumed to be public knowledge,
                // nothing has leaked out yet. The minimum length couldn't just be calculated
                // without reading the header.
                if (cipherText.Length < minLen)
                {
                    throw new CryptographicException();
                }

                // It's very important that the MAC be calculated and verified before
                // proceeding to decrypt the ciphertext, as this prevents any sort of
                // information leaking out to an attacker.
                //
                // Don't include the tag in the calculation, though.

                // First, everything before the tag (the cipher and MAC algorithm ids)
                tagGenerator.AppendData(cipherText, 0, tagOffset);

                // Skip the data before the tag and the tag, then read everything that
                // remains.
                tagGenerator.AppendData(
                    cipherText,
                    tagOffset + tagSizeInBytes,
                    cipherText.Length - tagSizeInBytes - tagOffset);

                byte[] generatedTag = tagGenerator.GetHashAndReset();

                // The time it took to get to this point has so far been a function only
                // of the length of the data, or of non-encrypted values (e.g. it could
                // take longer to prepare the *key* for the HMACSHA384 MAC than the
                // HMACSHA256 MAC, but the algorithm choice wasn't a secret).
                //
                // If the verification of the authentication tag aborts as soon as a
                // difference is found in the byte arrays then your program may be
                // acting as a timing oracle which helps an attacker to brute-force the
                // right answer for the MAC. So, it's very important that every possible
                // "no" (2^256-1 of them for HMACSHA256) be evaluated in as close to the
                // same amount of time as possible. For this, we call CryptographicEquals
                if (!CryptographicEquals(
                    generatedTag,
                    0,
                    cipherText,
                    tagOffset,
                    tagSizeInBytes))
                {
                    // Assuming every tampered message (of the same length) took the same
                    // amount of time to process, we can now safely say
                    // "this data makes no sense" without giving anything away.
                    throw new CryptographicException();
                }

                // Restore the IV into the symmetricAlgorithm instance.
                byte[] iv = new byte[blockSizeInBytes];
                Buffer.BlockCopy(cipherText, ivOffset, iv, 0, iv.Length);
                cipher.IV = iv;

                using (ICryptoTransform decryptor = cipher.CreateDecryptor())
                {
                    return Transform(
                        decryptor,
                        cipherText,
                        cipherTextOffset,
                        cipherTextLength);
                }
            }
        }

        private static byte[] Transform(
            ICryptoTransform transform,
            byte[] input,
            int inputOffset,
            int inputLength)
        {
            // Many of the implementations of ICryptoTransform report true for
            // CanTransformMultipleBlocks, and when the entire message is available in
            // one shot this saves on the allocation of the CryptoStream and the
            // intermediate structures it needs to properly chunk the message into blocks
            // (since the underlying stream won't always return the number of bytes
            // needed).
            if (transform.CanTransformMultipleBlocks)
            {
                return transform.TransformFinalBlock(input, inputOffset, inputLength);
            }

            // If our transform couldn't do multiple blocks at once, let CryptoStream
            // handle the chunking.
            using (MemoryStream messageStream = new MemoryStream())
            using (CryptoStream cryptoStream =
                new CryptoStream(messageStream, transform, CryptoStreamMode.Write))
            {
                cryptoStream.Write(input, inputOffset, inputLength);
                cryptoStream.FlushFinalBlock();
                return messageStream.ToArray();
            }
        }

        /// <summary>
        /// Open a properly configured <see cref="SymmetricAlgorithm"/> conforming to the
        /// scheme identified by <paramref name="aeCipher"/>.
        /// </summary>
        /// <param name="aeCipher">The cipher mode to open.</param>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <returns>
        /// A SymmetricAlgorithm object with the right key, cipher mode, and padding
        /// mode; or <c>null</c> on unknown algorithms.
        /// </returns>
        private static SymmetricAlgorithm GetCipher(AeCipher aeCipher, byte[] masterKey)
        {
            SymmetricAlgorithm symmetricAlgorithm;

            switch (aeCipher)
            {
                case AeCipher.Aes256CbcPkcs7:
                    symmetricAlgorithm = Aes.Create();
                    // While 256-bit, CBC, and PKCS7 are all the default values for these
                    // properties, being explicit helps comprehension more than it hurts
                    // performance.
                    symmetricAlgorithm.KeySize = 256;
                    symmetricAlgorithm.Mode = CipherMode.CBC;
                    symmetricAlgorithm.Padding = PaddingMode.PKCS7;
                    break;
                default:
                    // An algorithm we don't understand
                    throw new CryptographicException();
            }

            // Instead of using the master key directly, derive a key for our chosen
            // HMAC algorithm based upon the master key.
            //
            // Since none of the symmetric encryption algorithms currently in .NET
            // support key sizes greater than 256-bit, we can use HMACSHA256 with
            // NIST SP 800-108 5.1 (Counter Mode KDF) to derive a value that is
            // no smaller than the key size, then Array.Resize to trim it down as
            // needed.

            using (HMAC hmac = new HMACSHA256(masterKey))
            {
                // i=1, Label=ASCII(cipher)
                byte[] cipherKey = hmac.ComputeHash(
                    new byte[] { 1, 99, 105, 112, 104, 101, 114 });

                // Resize the array to the desired keysize. KeySize is in bits,
                // and Array.Resize wants the length in bytes.
                Array.Resize(ref cipherKey, symmetricAlgorithm.KeySize / 8);

                symmetricAlgorithm.Key = cipherKey;
            }

            return symmetricAlgorithm;
        }

        /// <summary>
        /// Open a properly configured <see cref="HMAC"/> conforming to the scheme
        /// identified by <paramref name="aeMac"/>.
        /// </summary>
        /// <param name="aeMac">The message authentication mode to open.</param>
        /// <param name="masterKey">The master key from which other keys derive.</param>
        /// <returns>
        /// An HMAC object with the proper key, or <c>null</c> on unknown algorithms.
        /// </returns>
        private static HMAC GetMac(AeMac aeMac, byte[] masterKey)
        {
            HMAC hmac;

            switch (aeMac)
            {
                case AeMac.HMACSHA256:
                    hmac = new HMACSHA256();
                    break;
                case AeMac.HMACSHA384:
                    hmac = new HMACSHA384();
                    break;
                default:
                    // An algorithm we don't understand
                    throw new CryptographicException();
            }

            // Instead of using the master key directly, derive a key for our chosen
            // HMAC algorithm based upon the master key.
            // Since the output size of the HMAC is the same as the ideal key size for
            // the HMAC, we can use the master key over a fixed input once to perform
            // NIST SP 800-108 5.1 (Counter Mode KDF):
            hmac.Key = masterKey;

            // i=1, Context=ASCII(MAC)
            byte[] newKey = hmac.ComputeHash(new byte[] { 1, 77, 65, 67 });

            hmac.Key = newKey;
            return hmac;
        }

        // A simple helper method to ensure that the offset (writePos) always moves
        // forward with new data.
        private static void Append(byte[] newData, byte[] combinedData, ref int writePos)
        {
            Buffer.BlockCopy(newData, 0, combinedData, writePos, newData.Length);
            writePos += newData.Length;
        }

        /// <summary>
        /// Compare the contents of two arrays in an amount of time which is only
        /// dependent on <paramref name="length"/>.
        /// </summary>
        /// <param name="a">An array to compare to <paramref name="b"/>.</param>
        /// <param name="aOffset">
        /// The starting position within <paramref name="a"/> for comparison.
        /// </param>
        /// <param name="b">An array to compare to <paramref name="a"/>.</param>
        /// <param name="bOffset">
        /// The starting position within <paramref name="b"/> for comparison.
        /// </param>
        /// <param name="length">
        /// The number of bytes to compare between <paramref name="a"/> and
        /// <paramref name="b"/>.</param>
        /// <returns>
        /// <c>true</c> if both <paramref name="a"/> and <paramref name="b"/> have
        /// sufficient length for the comparison and all of the applicable values are the
        /// same in both arrays; <c>false</c> otherwise.
        /// </returns>
        /// <remarks>
        /// An "insufficient data" <c>false</c> response can happen early, but otherwise
        /// a <c>true</c> or <c>false</c> response take the same amount of time.
        /// </remarks>
        [MethodImpl(MethodImplOptions.NoInlining | MethodImplOptions.NoOptimization)]
        private static bool CryptographicEquals(
            byte[] a,
            int aOffset,
            byte[] b,
            int bOffset,
            int length)
        {
            Debug.Assert(a != null);
            Debug.Assert(b != null);
            Debug.Assert(length >= 0);

            int result = 0;

            if (a.Length - aOffset < length || b.Length - bOffset < length)
            {
                return false;
            }

            unchecked
            {
                for (int i = 0; i < length; i++)
                {
                    // Bitwise-OR of subtraction has been found to have the most
                    // stable execution time.
                    //
                    // This cannot overflow because bytes are 1 byte in length, and
                    // result is 4 bytes.
                    // The OR propagates all set bytes, so the differences are only
                    // present in the lowest byte.
                    result = result | (a[i + aOffset] - b[i + bOffset]);
                }
            }

            return result == 0;
        }
    }
}
```

## <a name="see-also"></a>请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
