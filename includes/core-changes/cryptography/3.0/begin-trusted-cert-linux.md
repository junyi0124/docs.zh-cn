---
ms.openlocfilehash: 3c6142fd536bad5676f02570fecd4eb0605db829
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2020
ms.locfileid: "83720891"
---
### <a name="begin-trusted-certificate-syntax-no-longer-supported-for-root-certificates-on-linux"></a>Linux 上的根证书不再支持“BEGIN TRUSTED CERTIFICATE”语法

Linux 和其他类似 Unix 的系统（但不是 macOS）上的根证书可以采用两种形式显示：标准 `BEGIN CERTIFICATE` PEM 标头和特定于 OpenSSL 的 `BEGIN TRUSTED CERTIFICATE` PEM 标头。 后一种语法允许进行其他配置，这些配置已导致与 .NET Core 的 <xref:System.Security.Cryptography.X509Certificates.X509Chain?displayProperty=fullName> 类之间的兼容性问题。 从 .NET Core 3.0 开始，链引擎不再加载 `BEGIN TRUSTED CERTIFICATE` 根证书内容。

#### <a name="change-description"></a>更改描述

以前，`BEGIN CERTIFICATE` 和 `BEGIN TRUSTED CERTIFICATE` 语法均用于填充根信任列表。 如果使用了 `BEGIN TRUSTED CERTIFICATE` 语法，并且在文件中指定了其他选项，则 <xref:System.Security.Cryptography.X509Certificates.X509Chain> 可能报告已明确禁止链信任 (<xref:System.Security.Cryptography.X509Certificates.X509ChainStatusFlags.ExplicitDistrust?displayProperty=nameWithType>)。 但是，如果在以前加载的文件中同时使用 `BEGIN CERTIFICATE` 语法指定了证书，则允许链信任。

从 .NET Core 3.0 开始，不再读取 `BEGIN TRUSTED CERTIFICATE` 内容。 如果还没有通过标准 `BEGIN CERTIFICATE` 语法指定证书，则 <xref:System.Security.Cryptography.X509Certificates.X509Chain> 会报告根不受信任 (<xref:System.Security.Cryptography.X509Certificates.X509ChainStatusFlags.UntrustedRoot?displayProperty=nameWithType>)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

大多数应用程序不受此更改的影响，但是由于权限问题而无法同时看到两个根证书源的应用程序可能会在升级后遇到意外的 `UntrustedRoot` 错误。

许多 Linux 分发版（或发行版）将根证书写入两个位置：每个文件一个证书目录和一个文件串联。 在某些发行版上，每个文件一个证书目录使用 `BEGIN TRUSTED CERTIFICATE` 语法，而一个文件串联则使用标准 `BEGIN CERTIFICATE` 语法。 请确保将任何自定义根证书作为 `BEGIN CERTIFICATE` 添加到这些位置中的至少一个位置，并且你的应用程序可以读取这两个位置。

典型的目录是 /etc/ssl/certs/ */* ，典型的串联文件是 /etc/ssl/cert.pem。 使用命令 `openssl version -d` 来确定特定于平台的根目录，这可能不同于 /etc/ssl/。 例如，在 Ubuntu 18.04 上，目录是 /usr/lib/ssl/certs/，文件是 /usr/lib/ssl/cert.pem。 不过，/usr/lib/ssl/certs/ 是 /etc/ssl/certs/ 的符号链接，并且 /usr/lib/ssl/cert.pem 不存在。

```bash
$ openssl version -d
OPENSSLDIR: "/usr/lib/ssl"
$ ls -al /usr/lib/ssl
total 12
drwxr-xr-x  3 root root 4096 Dec 12 17:10 .
drwxr-xr-x 73 root root 4096 Feb 20 15:18 ..
lrwxrwxrwx  1 root root   14 Mar 27  2018 certs -> /etc/ssl/certs
drwxr-xr-x  2 root root 4096 Dec 12 17:10 misc
lrwxrwxrwx  1 root root   20 Nov 12 16:58 openssl.cnf -> /etc/ssl/openssl.cnf
lrwxrwxrwx  1 root root   16 Mar 27  2018 private -> /etc/ssl/private
```

#### <a name="category"></a>类别

密码

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.X509Certificates.X509Chain?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Security.Cryptography.X509Certificates.X509Chain`

-->
