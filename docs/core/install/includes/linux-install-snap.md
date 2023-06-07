---
ms.openlocfilehash: 83808f2f3a05333ed5d9e3809cbc2fd6e230d02c
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031748"
---

[.NET Core 可以从 Snap Store 中获取。](https://snapcraft.io/dotnet-sdk)

Snap 是应用及其依赖项的捆绑包，无需修改即可在多个不同的 Linux 发行版中正常运行。 可以从 Snap Store 中发现和安装 Snap。 若要详细了解 Snap，请参阅[开始使用 Snap](https://snapcraft.io/docs/getting-started)。

只有受支持的 .NET Core 版本，才能通过 Snap 获取。

### <a name="install-the-sdk"></a>安装 SDK

适用于 .NET SDK 的 Snap 包都是在同一标识符（即 `dotnet-sdk`）下发布的。 可以通过指定通道来安装特定版本的 SDK。 SDK 包括相应的运行时。 下表列出了通道：

| .NET 版本 | Snap 包             |
|--------------|--------------------------|
| 5.0          | `5.0` 或 `latest/stable` |
| 3.1 (LTS)    | `3.1` 或 `lts/stable`    |
| 2.1 (LTS)    | `2.1`                    |

若要安装适用于 .NET SDK 的 Snap 包，请运行 `snap install` 命令。 使用 `--channel` 参数来指明要安装哪个版本。 如果省略此参数，则使用 `latest/stable`。 在下面的示例中，指定的是 `5.0`：

```bash
sudo snap install dotnet-sdk --classic --channel=5.0
```

接下来，使用 `snap alias` 命令为系统注册 `dotnet` 命令：

```bash
sudo snap alias dotnet-sdk.dotnet dotnet
```

此命令的格式为 `sudo snap alias {package}.{command} {alias}`。 可以选择所需的任何 `{alias}` 名称。 例如，可以根据 Snap 安装的特定版本来命名此命令：`sudo snap alias dotnet-sdk.dotnet dotnet50`。 运行命令 `dotnet50` 时，将调用这一特定版本的 .NET。 但这与大多数教程和示例不兼容，因为它们需要使用 `dotnet` 命令。

### <a name="install-the-runtime"></a>安装运行时

适用于 .NET Core Runtime 的 Snap 包都是在各自的包标识符下发布的。 下表列出了这些包标识符：

| .NET 版本      | Snap 包        |
|-------------------|---------------------|
| 5.0               | `dotnet-runtime-50` |
| 3.1 (LTS)         | `dotnet-runtime-31` |
| 3.0               | `dotnet-runtime-30` |
| 2.2               | `dotnet-runtime-22` |
| 2.1 (LTS)         | `dotnet-runtime-21` |

若要安装适用于 .NET Runtime 的 Snap 包，请运行 `snap install` 命令。 在下面的示例中，安装的是 .NET 5.0：

```bash
sudo snap install dotnet-runtime-50 --classic
```

接下来，使用 `snap alias` 命令为系统注册 `dotnet` 命令：

```bash
sudo snap alias dotnet-runtime-50.dotnet dotnet
```

此命令的格式为 `sudo snap alias {package}.{command} {alias}`。 可以选择所需的任何 `{alias}` 名称。 例如，可以根据 Snap 安装的特定版本来命名此命令：`sudo snap alias dotnet-runtime-50.dotnet dotnet50`。 运行命令 `dotnet50` 时，将调用这一特定版本的 .NET。 但这与大多数教程和示例不兼容，因为它们需要使用 `dotnet` 命令。

### <a name="ssl-certificate-errors"></a>SSL 证书错误

通过 Snap 安装 .NET 后，可能会在某些发行版上找不到 .NET SSL 证书，并且可能会在 `restore` 期间看到如下错误：

```bash
Processing post-creation actions...
Running 'dotnet restore' on /home/myhome/test/test.csproj...
  Restoring packages for /home/myhome/test/test.csproj...
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error : Unable to load the service index for source https://api.nuget.org/v3/index.json. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The SSL connection could not be established, see inner exception. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The remote certificate is invalid according to the validation procedure. [/home/myhome/test/test.csproj]
```

若要解决此问题，请设置一些环境变量：

```bash
export SSL_CERT_FILE=[path-to-certificate-file]
export SSL_CERT_DIR=/dev/null
```

证书位置因发行版而异。 下面是我们在发行版中遇到此问题的位置。

* Fedora - `/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem`
* OpenSUSE - `/etc/ssl/ca-bundle.pem`
* Solus - `/etc/ssl/certs/ca-certificates.crt`
