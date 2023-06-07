---
title: FindPrivateKey 示例
ms.date: 12/04/2017
helpviewer_keywords:
- FindPrivateKey
ms.assetid: 16b54116-0ceb-4413-af0c-753bb2a785a6
ms.openlocfilehash: 0ed1e5e81a5d2f7f3586e5dce306e8244b5ebd48
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346017"
---
# <a name="findprivatekey-sample"></a>FindPrivateKey 示例

查找与证书存储区中的特定 X.509 证书关联的私钥文件的位置和名称可能很困难。 使用 FindPrivateKey.exe 工具可以更快地完成此过程。

> [!IMPORTANT]
> FindPrivateKey 是一个需要在使用之前进行编译的样本。 有关如何生成 FindPrivateKey 工具的说明，请参阅[生成 FindPrivateKey 项目](#to-build-the-findprivatekey-project)部分。

X.509 证书是由计算机管理员或计算机上的任何用户安装的。 但是，证书可以通过在不同帐户下运行的服务进行访问。 例如，NETWORK SERVICE 帐户。

此帐户可能没有访问私钥文件的权限，因为证书原来并不是由它安装的。 FindPrivateKey 工具可以为您提供给定 X.509 证书的私钥文件的位置。 您可以在知道特定 X.509 证书的私钥文件的位置后立即添加或移除对此文件的权限。

使用证书进行安全*设置*的示例使用 FindPrivateKey 文件中的工具。 找到私钥文件后，可以使用其他工具（如*Cacls* ）将适当的访问权限设置到该文件。

在用户帐户下运行 Windows Communication Foundation （WCF）服务（如自承载可执行文件）时，请确保用户帐户对文件具有只读访问权限。 在 Internet Information Services （IIS）下运行 WCF 服务时，运行服务的默认帐户是 IIS 7 和更早版本上的网络服务，或者是 IIS 7.5 及更高版本上的应用程序池标识。 有关详细信息，请参阅[应用程序池标识](/iis/manage/configuring-security/application-pool-identities)。

## <a name="examples"></a>示例

在访问该进程没有读取权限的证书时，将看到类似于以下示例的异常消息：

```output
System.ArgumentException was unhandled
Message="The certificate 'CN=localhost' must have a private key that is capable of key exchange.  The process must have access rights for the private key."
Source="System.ServiceModel"
```

出现这种情况时，请使用 FindPrivateKey 工具查找私钥文件，然后为运行服务的进程设置访问权限。 例如，可以通过 Cacls 工具来完成此操作，如以下示例中所示：

```console
cacls.exe "C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys\8aeda5eb81555f14f8f9960745b5a40d_38f7de48-5ee9-452d-8a5a-92789d7110b1" /E /G "NETWORK SERVICE":R
```

#### <a name="to-build-the-findprivatekey-project"></a>生成 FindPrivateKey 项目

若要下载此项目，请访问[.NET Framework 4 Windows Communication Foundation （WCF）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)。

1. 打开文件资源管理器并导航到安装示例的目录位置下的*WF_WCF_Samples \wcf\setup\findprivatekey\cs* "文件夹。

2. 双击 .sln 文件图标，在 Visual Studio 中打开该文件。

3. 在 "**生成**" 菜单中，选择 "**重新生成解决方案**"。

4. 生成解决方案将生成如下文件：FindPrivateKey.exe。

## <a name="conventionscommand-line-entries"></a>约定-命令行条目

 "[*选项*]" 代表一组可选参数。

 "{*option*}" 代表一组必需的参数。

 "*选项 1* &#124; *选项 2*" 表示在选项集之间选择。

 "\<*值*>" 表示要输入的参数值。

## <a name="usage"></a>用量

```console
FindPrivateKey <storeName> <storeLocation> [{ {-n <subjectName>} | {-t <thumbprint>} } [-f | -d | -a]]
```

其中：

| 参数         | 描述                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| `<subjectName>` | 证书的使用者名称                                               |
| `<thumbprint>`  | 证书的指纹（你可以使用 Certmgr.msc 工具来查找此文件） |
| `-f`            | 仅输出文件名                                                             |
| `-d`            | 仅输出目录                                                             |
| `-a`            | 输出绝对文件名                                                         |

如果未在命令提示符处指定任何参数，则会显示具有此信息的帮助文本。

## <a name="examples"></a>示例

此示例在当前用户的个人存储中查找使用者名称为 "CN = localhost" 的证书的文件名。

```console
FindPrivateKey My CurrentUser -n "CN=localhost"
```

此示例在当前用户的个人存储中查找使用者名称为 "CN = localhost" 的证书的文件名，并输出完整目录路径。

```console
FindPrivateKey My CurrentUser -n "CN=localhost" -a
```

此示例在“本地计算机”的“个人”存储区中查找指纹为 "03 33 98 63 d0 47 e7 48 71 33 62 64 76 5c 4c 9d 42 1d 6b 52" 的证书的文件名。

```console
FindPrivateKey My LocalMachine -t "03 33 98 63 d0 47 e7 48 71 33 62 64 76 5c 4c 9d 42 1d 6b 52"
```
