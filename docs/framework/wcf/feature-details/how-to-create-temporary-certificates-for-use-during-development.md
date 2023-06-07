---
title: 如何：创建开发期间使用的临时证书
description: 了解如何使用 PowerShell cmdlet 创建两个临时的 x.509 证书，以用于开发安全 WCF 服务或客户端。
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], creating temporary certificates
- temporary certificates [WCF]
ms.assetid: bc5f6637-5513-4d27-99bb-51aad7741e4a
ms.openlocfilehash: a249f0de00c45b1588762ffa0f826e890f961334
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "91607767"
---
# <a name="how-to-create-temporary-certificates-for-use-during-development"></a>如何：创建开发期间使用的临时证书

使用 Windows Communication Foundation (WCF) 开发安全服务或客户端时，通常需要提供要用作凭据的 x.509 证书。 该证书通常是证书链的一部分，在计算机的受信任的根证书颁发机构存储区中可找到根证书颁发机构。 拥有一个证书链，使您可以限定一组证书，其中根证书颁发机构通常来自于您的组织或业务单元。 若要在开发时模拟此情况，请创建两个证书以满足安全要求。 第一个证书是自签名证书，放置在受信任的根证书颁发机构存储区中；第二个证书是从第一个证书创建的，放置在本地计算机位置的个人存储区中或当前用户位置的个人存储区中。 本主题将指导完成使用 PowerShell [new-selfsignedcertificate) ](/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet 创建这两个证书的步骤。

> [!IMPORTANT]
> 新的 New-selfsignedcertificate cmdlet 生成的证书仅供测试之用。 部署服务或客户程序时，请确保使用证书颁发机构提供的适当证书。 这可能是来自组织或第三方的 Windows Server 证书服务器。
>
> 默认情况下， [new-selfsignedcertificate](/powershell/module/pkiclient/new-selfsignedcertificate) cmdlet 创建自签名证书，这些证书是不安全的。 将自签名证书放置在 "受信任的根证书颁发机构" 存储中使您能够创建更密切地模拟您的部署环境的开发环境。

 有关创建和使用证书的详细信息，请参阅使用 [证书](working-with-certificates.md)。 有关使用证书作为凭据的详细信息，请参阅 [保护服务和客户端](securing-services-and-clients.md)。 有关使用 Microsoft Authenticode 技术的教程，请参阅 [Authenticode Overviews and Tutorials](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537360(v=vs.85))（Authenticode 概述和教程）。

## <a name="to-create-a-self-signed-root-authority-certificate-and-export-the-private-key"></a>创建一个自签名根证书颁发机构证书并导出私钥

以下命令在当前用户个人存储中创建使用者名称为 "Rootca.cer" 的自签名证书。

```powershell
$rootcert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "RootCA" -TextExtension @("2.5.29.19={text}CA=true") -KeyUsage CertSign,CrlSign,DigitalSignature
```

我们需要将该证书导出到 PFX 文件中，以便可以将其导入到后面的步骤中所需的位置。 导出带私钥的证书时，需要使用密码来保护密码。 我们将密码保存在中 `SecureString` ，并使用 [get-pfxcertificate](/powershell/module/pkiclient/export-pfxcertificate) cmdlet 将具有关联私钥的证书导出到 PFX 文件。 我们还使用 [导出证书](/powershell/module/pkiclient/export-certificate) cmdlet 将公共证书仅保存到 CRT 文件中。

```powershell
[System.Security.SecureString]$rootcertPassword = ConvertTo-SecureString -String "password" -Force -AsPlainText
[String]$rootCertPath = Join-Path -Path 'cert:\CurrentUser\My\' -ChildPath "$($rootcert.Thumbprint)"
Export-PfxCertificate -Cert $rootCertPath -FilePath 'RootCA.pfx' -Password $rootcertPassword
Export-Certificate -Cert $rootCertPath -FilePath 'RootCA.crt'
```

## <a name="to-create-a-new-certificate-signed-by-a-root-authority-certificate"></a>创建一个由根证书颁发机构证书签名的新证书

下面的命令使用颁发者的私钥创建一个由使用者 `RootCA` 名称 "SignedByRootCA" 签名的证书。

```powershell
$testCert = New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName "SignedByRootCA" -KeyExportPolicy Exportable -KeyLength 2048 -KeyUsage DigitalSignature,KeyEncipherment -Signer $rootCert
```

同样，我们使用私钥将签名证书保存到 PFX 文件中，并将公钥保存到 CRT 文件中。

```powershell
[String]$testCertPath = Join-Path -Path 'cert:\LocalMachine\My\' -ChildPath "$($testCert.Thumbprint)"
Export-PfxCertificate -Cert $testCertPath -FilePath testcert.pfx -Password $rootcertPassword
Export-Certificate -Cert $testCertPath -FilePath testcert.crt
```

## <a name="installing-a-certificate-in-the-trusted-root-certification-authorities-store"></a>在受信任的根证书颁发机构存储区中安装证书

创建自签名证书后，您可以将它安装到受信任的根证书颁发机构存储区中。 任何使用该证书签名的证书在此处都受计算机的信任。 为此，当您不再需要该证书时可立即将它从存储区中删除。 当您删除此根证书颁发机构证书时，则由它签名的所有其他证书将成为未经授权的。 根证书颁发机构证书只是一种机制，必要时可限定一组证书。 例如，在对等应用程序中，通常不需要根证书颁发机构，因为您只信任由对方提供的证书的个体标识。

### <a name="to-install-a-self-signed-certificate-in-the-trusted-root-certification-authorities"></a>在受信任的根证书颁发机构中安装自签名证书

1. 打开证书管理单元。 有关详细信息，请参阅[如何：使用 MMC 管理单元查看证书](how-to-view-certificates-with-the-mmc-snap-in.md)。

2. 打开要存储证书的文件夹， **“本地计算机”** 或 **“当前用户”**。

3. 打开 **“受信任的根证书颁发机构”** 文件夹。

4. 右击 **“证书”** 文件夹，再单击 **“所有任务”**，然后单击 **“导入”**。

5. 按照屏幕向导说明，将 Rootca.cer 导入到存储中。

## <a name="using-certificates-with-wcf"></a>在 WCF 中使用证书

一旦安装了临时证书，就可以使用这些证书开发指定证书作为客户端凭据类型的 WCF 解决方案。 例如，下面的 XML 配置指定消息安全模式，并指定证书作为客户端凭据类型。

### <a name="to-specify-a-certificate-as-the-client-credential-type"></a>指定证书作为客户端凭据类型

1. 在服务的配置文件中，使用下面的 XML 将安全模式设置为消息，并将客户端凭据类型设置为证书。

    ```xml
    <bindings>
      <wsHttpBinding>
        <binding name="CertificateForClient">
          <security>
            <message clientCredentialType="Certificate" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    ```

2. 在客户端的配置文件中，使用以下 XML 来指定在用户的存储中找到该证书，并通过在 SubjectName 字段中搜索值 "CohoWinery" 来找到该证书。

    ```xml
    <behaviors>
      <endpointBehaviors>
        <behavior name="CertForClient">
          <clientCredentials>
            <clientCertificate findValue="CohoWinery" x509FindType="FindBySubjectName" />
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>
    ```

有关在 WCF 中使用证书的详细信息，请参阅 [Working with Certificates](working-with-certificates.md)。

## <a name="net-framework-security"></a>.NET Framework 安全性

请确保通过右击证书，再单击 **“删除”** ，从 **“受信任的根证书颁发机构”** 和 **“个人”** 文件夹中删除所有临时根证书颁发机构证书。

## <a name="see-also"></a>另请参阅

- [使用证书](working-with-certificates.md)
- [如何：使用 MMC 管理单元查看证书](how-to-view-certificates-with-the-mmc-snap-in.md)
- [保护服务和客户端的安全](securing-services-and-clients.md)
