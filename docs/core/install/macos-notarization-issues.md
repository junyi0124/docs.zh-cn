---
title: 处理 macOS Catalina 公证
description: 本文介绍在安装 .NET core 运行时、SDK 以及使用 .NET Core 生成的应用时，如何处理 macOS 的公证和证书问题。
author: adegeo
ms.author: adegeo
ms.date: 02/14/2020
ms.openlocfilehash: 616f163856cca48ccc6d1a14e0c6e68d56379c0c
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90538297"
---
# <a name="macos-catalina-notarization-and-the-impact-on-net-core-downloads-and-projects"></a>macOS Catalina 公证以及对 .NET Core 下载和项目的影响

自 macOS Catalina（版本 10.15）开始，所有在 2019 年 6 月 1 日之后生成并使用开发者 ID 扩散的软件都必须经过公证。 此要求应用于 .NET Core 运行时、.NET Core SDK 以及使用 .NET Core 创建的软件。 本文介绍了 .NET Core 和 macOS 公证可能会遇到的常见情况。

## <a name="installing-net-core"></a>安装 .NET Core

自 2020 年 2 月 18 日起，.NET Core（运行时和 SDK）版本 3.1、3.0 和 2.1 的安装程序都已经过公证。 以前发布的版本没有经过公证。 通过先下载安装程序，然后使用 `sudo installer` 命令，可以手动安装 .NET Core 的未公证版本。 有关详细信息，请参阅[下载并手动安装 macOS](./macos.md#download-and-manually-install)。

自以下版本开始，.NET Core 安装程序未经过公证：

- .NET Core 运行时
  - 2.1.16
  - 3.0.3
  - 3.1.2

- .NET Core SDK
  - 2.1.512
  - 3.0.103
  - 3.1.102

## <a name="apphost-is-disabled-by-default"></a>默认禁用 appHost

默认情况下，当项目编译、发布或运行时，.NET Core SDK 3.0 及更高版本的未公证版本会生成一个本机 Mach-O 可执行文件（即 appHost）。 此可执行文件是运行应用的一种简便方法。 否则必须通过运行 `dotnet <filename.dll>` 启动应用。 启用 appHost 后，会在 appHost 的上下文中调用 `dotnet run` 命令。 有关详细信息，请参阅 [appHost 的上下文](#context-of-the-apphost)。

从 .NET Core SDK 3.0 及更高版本的已公证版本开始，默认情况下不生成 appHost 可执行文件。 可以通过项目文件中的 [`UseAppHost`](../project-sdk/msbuild-props.md#useapphost) 布尔值设置来启用 appHost 生成。 还可以在运行的特定 `dotnet` 命令的命令行上通过 `-p:UseAppHost` 参数切换 appHost 的启用状态：

- 项目文件

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- 命令行参数

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

发布[独立](../deploying/index.md#publish-self-contained)应用时，始终会创建 appHost。

有关 `UseAppHost` 设置的详细信息，请参阅 [Microsoft.NET.Sdk 的 MSBuild 属性](../project-sdk/msbuild-props.md#useapphost)。

### <a name="context-of-the-apphost"></a>AppHost 的上下文

在项目中启用了 appHost 并使用 `dotnet run` 命令运行应用时，将在 appHost 的上下文中调用应用，而不是在默认主机（默认主机对应的是 `dotnet` 命令）的上下文中调用。 如果在项目中禁用了 appHost，则 `dotnet run` 命令将在默认主机的上下文中运行应用。 即使禁用了 appHost，如果发布独立应用，也会生成一份 appHost 可执行文件，且用户也可以使用该可执行文件运行应用。 使用 `dotnet <filename.dll>` 运行应用会通过默认主机（共享运行时）调用应用。

调用使用 appHost 的应用时，应用访问的证书分区与已公证的默认主机不同。 如果应用必须访问通过默认主机安装的证书，请使用 `dotnet run` 命令从应用的项目文件运行它，或使用 `dotnet <filename.dll>` 命令直接启动该应用。

有关此方案的详细信息，请参阅 [ASP.NET Core 和 macOS 和证书](#aspnet-core-and-macos-and-certificates)部分。

## <a name="aspnet-core-and-macos-and-certificates"></a>ASP.NET Core 和 macOS 和证书

使用 .NET Core，可以使用 <xref:System.Security.Cryptography.X509Certificates> 类在 macOS Keychain 中管理证书。 在决定要考虑哪个分区时，对 macOS Keychain 的访问将使用应用程序标识作为主键。 例如，未签名的应用程序会将机密存储在未签名分区中，而已签名应用程序会将其机密存储在仅能由它们访问的分区中。 要使用的分区取决于调用应用的执行源。

.NET Core 提供三个执行源：[appHost](#apphost-is-disabled-by-default)、默认主机（`dotnet` 命令）和自定义主机。 每个执行模型都可能具有不同的标识（已签名或未签名），并可以访问 Keychain 中的不同分区。 通过一种模式导入的证书可能不能通过另一种模式访问。 例如，已公证版本的 .NET Core 具有已签名的默认主机。 根据证书的标识将证书导入到安全分区中。 由于 appHost 是未签名的，因此无法从生成的 appHost 访问这些证书。

再举一例，默认情况下，ASP.NET Core 通过默认主机导入默认 SSL 证书。 使用 appHost 的 ASP.NET Core 应用程序将不能访问此证书，并且当 .NET Core 检测到无法访问该证书时，将收到错误消息。 该错误消息中会提供关于如何解决此问题的说明。

如果证书共享是必需的，macOS 会提供带有 `security` 实用工具的配置选项。

如需详细了解如何解决 ASP.NET Core 证书问题，请参阅[在 ASP.NET Core 中强制执行 HTTPS](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.1&tabs=visual-studio#troubleshoot-certificate-problems)。

## <a name="default-entitlements"></a>默认权利

.NET Core 的默认主机（`dotnet` 命令）具有一组默认权利。 需要这些权利才能正常运行 .NET Core。 你的应用程序可能需要其他权利，在这种情况下，需要生成并使用 [appHost](#apphost-is-disabled-by-default)，然后在本地添加必要的权利。

.NET Core 的默认权利包括：

- `com.apple.security.cs.allow-jit`
- `com.apple.security.cs.allow-unsigned-executable-memory`
- `com.apple.security.cs.allow-dyld-environment-variables`
- `com.apple.security.cs.disable-library-validation`

## <a name="notarize-a-net-core-app"></a>对 .NET Core 应用进行公证

如果希望应用程序在 macOS Catalina（版本 10.15）或更高版本上运行，则需要对应用进行公证。 为了进行公证而随应用程序提交的 appHost 应至少具备与 .NET Core 相同的[默认权利](#default-entitlements)。

## <a name="next-steps"></a>后续步骤

- [.NET Core 依赖项和要求](macos.md#dependencies)。
- [安装 .NET Core 运行时和 SDK](macos.md)。
