---
title: 自承载 gRPC 应用程序-WCF 开发人员 gRPC
description: 将 ASP.NET Core gRPC 应用程序部署为自承载服务。
ms.date: 12/15/2020
ms.openlocfilehash: a5e2316b8d76593f4eb53760d2609b5bbbc9d2c5
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938528"
---
# <a name="self-hosted-grpc-applications"></a>自承载的 gRPC 应用程序

尽管 ASP.NET Core 5.0 应用程序可在 Windows Server 上的 IIS 中进行托管，但目前无法在 IIS 中承载 gRPC 应用程序，因为某些 HTTP/2 功能不受支持。 此功能是将来对 Windows Server 进行更新的目标。

你可以将应用程序作为 Windows 服务运行。 或者，你可以将其作为由 [systemd](https://en.wikipedia.org/wiki/Systemd)控制的 Linux 服务运行，因为 .net 5.0 托管扩展中的新增功能。

## <a name="run-your-app-as-a-windows-service"></a>以 Windows 服务的形式运行你的应用

若要将 ASP.NET Core 应用程序配置为作为 Windows 服务运行，请从 NuGet 安装 [WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices) 包。 然后，将对的调用添加到 `UseWindowsService` `CreateHostBuilder` 中的方法 `Program.cs` 。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .UseWindowsService() // Enable running as a Windows service
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

> [!NOTE]
> 如果应用程序不是作为 Windows 服务运行，则该 `UseWindowsService` 方法不执行任何操作。

现在，使用以下方法之一发布应用程序：

* 在 Visual Studio 中，右键单击项目，然后选择快捷菜单上的 " **发布** "。
* 从 .NET CLI。

发布 .NET 应用程序时，可以选择创建 *依赖于框架的* 部署或 *独立* 部署。 依赖于框架的部署要求在运行的主机上安装 .NET 共享运行时。 自包含的部署是使用 .NET 运行时和框架的完整副本发布的，可以在任何主机上运行。 有关详细信息，包括每种方法的优点和缺点，请参阅 [.net 应用程序部署](../../core/deploying/index.md) 文档。

若要发布不需要在主机上安装 .NET 5.0 运行时的应用程序的自包含生成，请指定要包含在该应用程序中的运行时。 使用 `-r` (或 `--runtime`) 标志。

```dotnetcli
dotnet publish -c Release -r win-x64 -o ./publish
```

若要发布依赖于框架的生成，请省略 `-r` 标志。

```dotnetcli
dotnet publish -c Release -o ./publish
```

将目录的完整内容复制 `publish` 到安装文件夹中。 然后，使用 [sc 工具](/windows/desktop/services/controlling-a-service-using-sc) 为可执行文件创建 Windows 服务。

```console
sc create MyService binPath=C:\MyService\MyService.exe
```

### <a name="log-to-the-windows-event-log"></a>记录到 Windows 事件日志

`UseWindowsService`方法自动添加将日志消息写入 Windows 事件日志的[日志记录](/aspnet/core/fundamentals/logging/)提供程序。 你可以通过将 `EventLog` 条目添加到 `Logging` `appsettings.json` 或其他配置源的部分来配置此提供程序的日志记录。

通过在这些设置中设置属性，可以重写事件日志中使用的源名称 `SourceName` 。 如果未指定名称，则默认的应用程序名称 (将使用的可执行程序集名称) 。

本章末尾介绍了日志记录的详细信息。

## <a name="run-your-app-as-a-linux-service-with-systemd"></a>使用 systemd 将应用作为 Linux 服务运行

若要将 ASP.NET Core 应用程序配置为在 Linux 行话) 中以 Linux 服务 (或 *守护* 程序运行，请从 NuGet 安装 [Microsoft.Extensions.Hosting.Systemd](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.Systemd) 包。 然后，将对的调用添加到 `UseSystemd` `CreateHostBuilder` 中的方法 `Program.cs` 。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .UseSystemd() // Enable running as a Systemd service
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

> [!NOTE]
> 如果应用程序未作为 Linux 服务运行，则该 `UseSystemd` 方法不执行任何操作。

现在发布应用程序。 应用程序可以是依赖于框架的依赖项，也可以独立于适用的 Linux 运行时 (例如 `linux-x64`) 。 您可以使用以下方法之一发布：

* 在 Visual Studio 中，右键单击项目，然后选择快捷菜单上的 " **发布** "。
* 在 .NET CLI 中，使用以下命令：

  ```dotnetcli
  dotnet publish -c Release -r linux-x64 -o ./publish
  ```
  
将目录的完整内容复制 `publish` 到 Linux 主机上的安装文件夹中。 注册服务需要将一个名为 *unit file* 的特殊文件添加到 `/etc/systemd/system` 目录中。 你需要 root 权限才能在此文件夹中创建文件。 使用要使用的标识符和扩展名来命名该文件 `systemd` `.service` 。 例如，使用 `/etc/systemd/system/myapp.service`。

服务文件使用 INI 格式，如以下示例中所示：

```ini
[Unit]
Description=My gRPC Application

[Service]
Type=notify
ExecStart=/usr/sbin/myapp

[Install]
WantedBy=multi-user.target
```

`Type=notify`属性指示 `systemd` 应用程序在启动和关闭时将通知它。 此 `WantedBy=multi-user.target` 设置将导致在 Linux 系统达到 "运行级别 2" 时启动服务，这意味着非图形、多用户 shell 处于活动状态。

`systemd`需要重新加载其配置，然后才能识别该服务。 您可以 `systemd` 使用命令进行控制 `systemctl` 。 重新加载后，请使用 `status` 子命令确认应用程序已成功注册。

```console
sudo systemctl daemon-reload
sudo systemctl status myapp
```

如果已正确配置服务，则会看到以下输出：

```text
myapp.service - My gRPC Application
 Loaded: loaded (/etc/systemd/system/myapp.service; disabled; vendor preset: enabled)
 Active: inactive (dead)
```

使用 `start` 命令启动服务。

```console
sudo systemctl start myapp.service
```

> [!TIP]
> `.service`使用时，扩展插件是可选的 `systemctl start` 。

若要通知 `systemd` 在系统启动时自动启动该服务，请使用 `enable` 命令。

```console
sudo systemctl enable myapp
```

### <a name="log-to-journald"></a>登录到 journald

Windows 事件日志的 Linux 等效项是 `journald` 中包含的结构化日志记录系统服务 `systemd` 。 由 Linux 后台程序写入标准输出的日志消息将自动写入到 `journald` 中。 若要配置日志记录级别，请使用 `Console` 日志记录配置的部分。 `UseSystemd`主机生成器方法自动配置控制台输出格式以适合日志。

由于 `journald` 是 Linux 日志的标准，因此各种工具与之集成。 可以轻松地将日志从路由 `journald` 到外部日志记录系统。 在本地运行的主机上，可以使用 `journalctl` 命令从命令行中查看日志。

```console
sudo journalctl -u myapp
```

> [!TIP]
> 如果主机上有可用的 GUI 环境，则可以使用一些图形日志查看器，如 *QJournalctl* 和 *gnome 日志*。

若要了解有关 `systemd` 使用从命令行查询日志的详细信息 `journalctl` ，请参阅 [manpages](https://manpages.debian.org/buster/systemd/journalctl.1)。

## <a name="https-certificates-for-self-hosted-applications"></a>用于自承载应用程序的 HTTPS 证书

在生产环境中运行 gRPC 应用程序时，应使用来自受信任证书颁发机构 (CA) 的 TLS 证书。 此 CA 可以是公共 CA，也可以是组织的内部 ca。

在 Windows 主机上，你可以使用类从安全的 [证书存储区](/windows/win32/seccrypto/managing-certificates-with-certificate-stores) 中加载证书 <xref:System.Security.Cryptography.X509Certificates.X509Store> 。 你还可以将 `X509Store` 类用于某些 Linux 主机上的 OpenSSL 密钥存储。

还可以通过使用 [X509Certificate2 构造函数](xref:System.Security.Cryptography.X509Certificates.X509Certificate2.%23ctor%2A)之一创建证书，方法如下：

* 文件，如 `.pfx` 受强密码保护的文件
* 从安全存储服务（如[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ）检索的二进制数据

可以通过两种方式将 Kestrel 配置为使用证书：通过配置或代码。

### <a name="set-https-certificates-by-using-configuration"></a>使用配置设置 HTTPS 证书

配置方法要求在 `.pfx` Kestrel 配置节中设置证书文件的路径和密码。 在中 `appsettings.json` ，这将如下所示：

```json
{
  "Kestrel": {
    "Certificates": {
      "Default": {
        "Path": "cert.pfx",
        "Password": "DO NOT STORE PLAINTEXT PASSWORDS IN APPSETTINGS FILES"
      }
    }
  }
}
```

使用安全配置源（如 Azure Key Vault 或 Hashicorp 保管库）提供密码。

> [!IMPORTANT]
> 不要在配置文件中存储未加密的密码。

### <a name="set-https-certificates-in-code"></a>在代码中设置 HTTPS 证书

若要在代码中的 Kestrel 上配置 HTTPS，请使用 `ConfigureKestrel` `IWebHostBuilder` 类中的方法 `Program` 。

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
            webBuilder.ConfigureKestrel(kestrel =>
            {
                kestrel.ConfigureHttpsDefaults(https =>
                {
                    https.ServerCertificate = new X509Certificate2("mycert.pfx", "password");
                });
            });
        });
```

同样，请务必将文件的密码存储 `.pfx` 在中，并从安全配置源中检索该文件。

>[!div class="step-by-step"]
>[上一页](grpc-in-production.md)
>[下一页](docker.md)
