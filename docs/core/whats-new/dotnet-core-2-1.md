---
title: .NET Core 2.1 的新增功能
description: 了解 .NET Core 2.1 的新增功能。
dev_langs:
- csharp
- vb
ms.date: 10/10/2018
ms.openlocfilehash: 5c15b663cf69de334771e83c1a14db8d66d30458
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437914"
---
# <a name="whats-new-in-net-core-21"></a>.NET Core 2.1 的新增功能

.NET Core 2.1 提供以下几个方面的增强功能和新功能：

- [工具](#tooling)
- [前滚](#roll-forward)
- [部署](#deployment)
- [Windows 兼容包](#windows-compatibility-pack)
- [JIT 编译改进](#jit-compiler-improvements)
- [API 更改](#api-changes)

## <a name="tooling"></a>工具

.NET Core 2.1 SDK (v 2.1.300)，该工具与 .NET Core 2.1 一起提供，包括以下更改和增强功能：

### <a name="build-performance-improvements"></a>生成性能改进

.NET Core 2.1 的主要关注点是改进生成时性能，特别是增量生成。 这些性能改进适用于使用 `dotnet build` 的两个命令行生成和 Visual Studio 中的生成。 一些个别的改进领域包括：

- 对于包资产解决方法，只解决由生成使用的资产而不是所有资产。

- 程序集引用缓存。

- 使用长时间运行的 SDK 生成服务器，这些是跨各个 `dotnet build` 调用的过程。 每次 `dotnet build` 运行时不再需要 JIT 编译大量代码块。 生成服务器进程可以使用以下命令自动终止：

   ```dotnetcli
   dotnet buildserver shutdown
   ```

### <a name="new-cli-commands"></a>新的 CLI 命令

许多使用 `DotnetCliToolReference` 的仅在每个项目的基础上可用的工具现作为 .NET Core SDK 的一部分提供。 这些工具包括：

- `dotnet watch` 提供文件系统观察程序，该程序在执行指定的命令集之前会首先等待文件更改。 例如，下面的命令将自动重新生成当前项目，并在其中的文件发生更改时生成详细输出：

   ```dotnetcli
   dotnet watch -- --verbose build
   ```

   请注意 `--verbose` 选项前面的 `--` 选项。 它分隔从传递给子 `dotnet` 进程的参数直接传递到 `dotnet watch` 命令的选项。 如果没有该选项，`--verbose` 选项将适用于 `dotnet watch` 命令，而非 `dotnet build` 命令。
  
   有关详细信息，请参阅[使用 dotnet watch 开发 ASP.NET Core 应用](/aspnet/core/tutorials/dotnet-watch)。

- `dotnet dev-certs` 生成和管理在 ASP.NET Core 应用程序开发期间使用的证书。

- `dotnet user-secrets` 管理 ASP.NET Core 应用程序中用户机密库的机密。

- `dotnet sql-cache` 在 Microsoft SQL Server 数据库中创建表和索引以用于分布式缓存。

- `dotnet ef` 是用于管理 Entity Framework Core 应用程序中数据库、<xref:Microsoft.EntityFrameworkCore.DbContext> 对象和迁移的工具。 有关详细信息，请参阅 [EF Core .NET 命令行工具](/ef/core/miscellaneous/cli/dotnet)。

### <a name="global-tools"></a>全局工具

.NET Core 2.1 支持全局工具，即，可通过命令行在全局范围内使用的自定义工具。 以前版本的 .NET Core 中的扩展性模型只能通过使用 `DotnetCliToolReference` 在每个项目的基础上提供自定义工具。

若要安装全局工具，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令。 例如：

```dotnetcli
dotnet tool install -g dotnetsay
```

完成安装后，可以通过指定工具名称从命令行运行该工具。 有关详细信息，请参阅 [.NET Core 工具概述](../tools/global-tools.md)。

### <a name="tool-management-with-the-dotnet-tool-command"></a>使用 `dotnet tool` 命令管理工具

在 .NET Core 2.1 SDK 中，所有工具操作都使用 `dotnet tool` 命令。 可用选项如下：

- [`dotnet tool install`](../tools/dotnet-tool-install.md) 安装工具。

- [`dotnet tool update`](../tools/dotnet-tool-update.md) 卸载并重新安装工具，它将高效地对其进行更新。

- [`dotnet tool list`](../tools/dotnet-tool-list.md) 列出当前安装的工具。

- [`dotnet tool uninstall`](../tools/dotnet-tool-uninstall.md) 卸载当前安装的工具。

## <a name="roll-forward"></a>前滚

从 .NET Core 2.0 开始，所有 .NET Core 应用程序都将自动前滚到系统上安装的最新次要版本。

从 .NET Core 2.0 开始，如果在其中构建应用程序的 .NET Core 版本在运行时不存在，应用程序将针对最新安装的次要版本的 .NET Core 自动运行。 换而言之，如果应用程序在 .NET Core 2.0 中生成，而主机系统未安装 .NET Core 2.0 但安装了 .NET Core 2.1，则应用程序将通过 .NET Core 2.1 运行。

> [!IMPORTANT]
> 此前滚行为不适用于预览版本。 默认情况下，它也不适用于主要版本，但可以通过以下设置进行更改。

可以通过在没有候选共享框架的情况下更改前滚设置来修改此行为。 可用设置如下：

- `0` - 禁用次要版本前滚行为。 使用此设置，为 .NET Core 2.0.0 构建的应用程序将前滚到 .NET Core 2.0.1，但不会前滚到 .NET Core 2.2.0 或 .NET Core 3.0.0。
- `1` - 启用次要版本前滚行为。 这是设置的默认值。 使用此设置，为 .NET Core 2.0.0 构建的应用程序将前滚到 .NET Core 2.0.1 或 .NET Core 2.2.0，具体取决于安装的版本，但它不会前滚到 .NET Core3.0.0。
- `2` - 启用次要和主要版本前滚行为。 即使考虑不同的主要版本，如果这样设置，为 .NET Core 2.0.0 构建的应用程序将前滚到 .NET Core 3.0.0。

可以通过以下三种方式之一修改此设置：

- 将 `DOTNET_ROLL_FORWARD_ON_NO_CANDIDATE_FX` 环境变量设置为所需的值。

- 使用所需的值将下列行添加到 .runtimeconfig.json 文件：

   ```json
   "rollForwardOnNoCandidateFx" : 0
   ```

- 使用 [.NET Core CLI](../tools/index.md) 时，请使用所需的值将下列选项添加到 .NET Core 命令，例如 `run`：

   ```dotnetcli
   dotnet run --rollForwardOnNoCandidateFx=0
   ```

修补程序版本前滚与此设置无关，并且在应用任何潜在的次要或主要版本前滚之后完成。

## <a name="deployment"></a>部署

### <a name="self-contained-application-servicing"></a>自包含应用程序服务

`dotnet publish` 现发布服务运行时版本的自包含应用程序。 当你使用 .NET Core 2.1 SDK (v 2.1.300) 发布自包含应用程序时，你的应用程序将包括此 SDK 已知的最新服务运行时版本。 升级到最新的 SDK 时，你将发布最新的 .NET Core 运行时版本。 这适用于 .NET Core 1.0 运行时以及更高版本。

自包含发布依赖于 NuGet.org 上的运行时版本。计算机上不需要有服务运行时。

使用 .NET Core 2.0 SDK，自包含应用程序将通过 .NET Core 2.0.0 运行时发布，除非通过 `RuntimeFrameworkVersion` 属性指定不同版本。 借助此新行为，你将不再需要设置此属性便可为自包含的应用程序选择更高版本的运行时。 最简单的方法是始终通过 .NET Core 2.1 SDK (v 2.1.300) 发布。

有关详细信息，请参阅[独立部署运行时前滚](../deploying/runtime-patch-selection.md)。

## <a name="windows-compatibility-pack"></a>Windows 兼容包

当你将现有代码从 .NET Framework 转移到 .NET Core 时，可以使用 [Windows 兼容包](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)。 除了 .NET Core 中提供的 API，它使你还能够访问额外的 20,000 多个 API。 这些 API 包括 <xref:System.Drawing?displayProperty=nameWithType> 命名空间中的类型、<xref:System.Diagnostics.EventLog> 类、WMI、性能计数器、Windows 服务以及 Windows 注册表类型和成员。

## <a name="jit-compiler-improvements"></a>JIT 编译器改进

.NET Core 包含新的 JIT 编译器技术，称为“分层编译”（也称为“自适应优化”），可以显著提高性能。 分层编译是一个可选设置。

由 JIT 编译器执行的重要任务之一是优化代码执行。 然而，对于很少使用的代码路径，相比运行未优化代码所花费的运行时，编译器可能需要更多的时间来优化代码。 分层编译介绍了 JIT 编译中的两个阶段：

- 第一层，将尽可能快地生成代码。

- 第二层，将为那些频繁执行的方法生成优化代码。 为了增强性能，第二层编译并行执行。

可以通过这两种方法之一选择加入分层编译。

- 若要在所有使用 .NET Core 2.1 SDK 的项目中使用分层编译，请设置以下环境变量：

  ```console
  COMPlus_TieredCompilation="1"
  ```

- 若要在每个项目的基础上使用分层编译，将 `<TieredCompilation>` 属性添加到 MSBuild 项目文件的 `<PropertyGroup>` 部分，如以下示例所示：

   ```xml
   <PropertyGroup>
      <!-- other property definitions -->

      <TieredCompilation>true</TieredCompilation>
   </PropertyGroup>
   ```

## <a name="api-changes"></a>API 更改

### <a name="spant-and-memoryt"></a>`Span<T>` 和 `Memory<T>`

.NET Core 2.1 包括一些新类型，使得在使用数组和其他类型内存方面要高效得多。 新类型包括：

- <xref:System.Span%601?displayProperty=nameWithType> 和 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>。

- <xref:System.Memory%601?displayProperty=nameWithType> 和 <xref:System.ReadOnlyMemory%601?displayProperty=nameWithType>。

如果没有这些类型，那么在作为数组的一部分或内存缓冲区的一部分传递此类项时，必须在将数据的某些部分传递给方法之前复制该数据部分。 这些类型提供了该数据的虚拟视图，无需额外的内存分配和复制操作。

下面的示例使用 <xref:System.Span%601> 和 <xref:System.Memory%601> 实例来提供一个数组 10 个元素的虚拟视图。

[!code-csharp[Span\<T>](./snippets/dotnet-core-2-1/csharp/program.cs)]

[!code-vb[Memory\<T>](./snippets/dotnet-core-2-1/vb/program.vb)]

### <a name="brotli-compression"></a>Brotli 压缩

.NET Core 2.1 添加了对 Brotli 压缩和解压缩的支持。 Brotli 是在 [RFC 7932](https://www.ietf.org/rfc/rfc7932.txt) 中定义的通用无损压缩算法，并且大多数 Web 浏览器和主 Web 服务器都提供支持。 可以使用基于流的 <xref:System.IO.Compression.BrotliStream?displayProperty=nameWithType> 类或基于范围的高性能 <xref:System.IO.Compression.BrotliEncoder?displayProperty=nameWithType> 和 <xref:System.IO.Compression.BrotliDecoder?displayProperty=nameWithType> 类。 下面的示例用 <xref:System.IO.Compression.BrotliStream> 类演示压缩：

[!code-csharp[Brotli compression](./snippets/dotnet-core-2-1/csharp/brotli.cs#1)]

[!code-vb[Brotli compression](./snippets/dotnet-core-2-1/vb/brotli.vb#1)]

<xref:System.IO.Compression.BrotliStream> 行为等同于 <xref:System.IO.Compression.DeflateStream> 和 <xref:System.IO.Compression.GZipStream>，这样就可以轻松地将调用这些 API 的代码转换为 <xref:System.IO.Compression.BrotliStream>。

### <a name="new-cryptography-apis-and-cryptography-improvements"></a>新加密 API 和加密改进

.NET Core 2.1 包括加密 API 的许多增强功能：

- <xref:System.Security.Cryptography.Pkcs.SignedCms?displayProperty=nameWithType> 在 System.Security.Cryptography.Pkcs 包中提供。 其实现与 .NET Framework 中的 <xref:System.Security.Cryptography.Pkcs.SignedCms> 类相同。

- <xref:System.Security.Cryptography.X509Certificates.X509Certificate.GetCertHash%2A?displayProperty=nameWithType> 和 <xref:System.Security.Cryptography.X509Certificates.X509Certificate.GetCertHashString%2A?displayProperty=nameWithType> 方法的新重载接受一个哈希算法标识符，使调用方能够使用除 SHA-1 以外的算法获得证书指纹值。

- 新的基于 <xref:System.Span%601> 的加密 API 可用于哈希、HMAC、加密随机数生成、非对称签名生成、非对称签名处理和 RSA 加密。

- 通过使用基于 <xref:System.Span%601> 的实现，<xref:System.Security.Cryptography.Rfc2898DeriveBytes?displayProperty=nameWithType> 的性能提高了大约 15%。

- 新 <xref:System.Security.Cryptography.CryptographicOperations?displayProperty=nameWithType> 类包括两个新方法：

  - <xref:System.Security.Cryptography.CryptographicOperations.FixedTimeEquals%2A> 需要固定时间来返回任意两个长度相同的输入，这使得它适用于加密验证，从而避免提供计时旁道信息。

  - <xref:System.Security.Cryptography.CryptographicOperations.ZeroMemory%2A> 是不能进行优化的内存清理例程。

- 静态 <xref:System.Security.Cryptography.RandomNumberGenerator.Fill%2A?displayProperty=nameWithType> 方法用随机值填充 <xref:System.Span%601>。

- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms?displayProperty=nameWithType> 现在在 Linux 和 macOS 上受支持。

- <xref:System.Security.Cryptography.ECDiffieHellman?displayProperty=nameWithType> 类系列现提供椭圆曲线 Diffie-Hellman (ECDH)。 外围应用与 .NET Framework 中相同。

- <xref:System.Security.Cryptography.RSA.Create%2A?displayProperty=nameWithType> 返回的实例可以使用 SHA-2 摘要对 OAEP 进行加密或解密，并使用 RSA-PSS 生成或验证签名。

### <a name="sockets-improvements"></a>套接字改进

.NET Core 包括一个新类型 <xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType> 和重写的 <xref:System.Net.Http.HttpMessageHandler?displayProperty=nameWithType>，两者构成了更高级别网络 API 的基础。  例如，<xref:System.Net.Http.SocketsHttpHandler?displayProperty=nameWithType> 是 <xref:System.Net.Http.HttpClient> 实现的基础。 在以前版本的 .NET Core 中，更高级别的 API 基于本机网络实现。

.NET Core 2.1 中引入的套接字实现具有很多优点：

- 对照以前的实现，可以看到显著的性能改进。

- 消除平台依赖项，从而简化部署和维护。

- 在所有 .NET Core 平台之间保持行为一致。

<xref:System.Net.Http.SocketsHttpHandler> 是 .NET Core 2.1 中的默认实现。 但是，你可以通过调用 <xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType> 方法配置应用程序以使用较旧的 <xref:System.Net.Http.HttpClientHandler> 类：

```csharp
AppContext.SetSwitch("System.Net.Http.UseSocketsHttpHandler", false);
```

```vb
AppContext.SetSwitch("System.Net.Http.UseSocketsHttpHandler", False)
```

还可以使用环境变量选择退出使用基于 <xref:System.Net.Http.SocketsHttpHandler> 的套接字实现。 为此，需要将 `DOTNET_SYSTEM_NET_HTTP_USESOCKETSHTTPHANDLER` 设置为 `false` 或 0。

在 Windows 上，还可以选择使用 <xref:System.Net.Http.WinHttpHandler?displayProperty=nameWithType>后者依赖于本机实现），或者通过将类实例传递到 <xref:System.Net.Http.HttpClient> 构造函数来使用 <xref:System.Net.Http.SocketsHttpHandler> 类。

在 Linux 和 macOS 上，可以在每个进程的基础上仅配置 <xref:System.Net.Http.HttpClient>。 在 Linux 上，如果想要使用旧的 <xref:System.Net.Http.HttpClient> 实现，则需要部署 [libcurl](https://curl.haxx.se/libcurl/)。 （它随 .NET Core 2.0 一起安装。）

### <a name="breaking-changes"></a>重大更改

有关中断性变更的信息，请参阅[从版本 2.0 迁移到 2.1 的中断性变更](../compatibility/2.1.md)。

## <a name="see-also"></a>请参阅

- [.NET Core 3.1 的新增功能](dotnet-core-3-1.md)
- [EF Core 2.1 中的新增功能](/ef/core/what-is-new/ef-core-2.1)
- [ASP.NET Core 2.1 的新增功能](/aspnet/core/aspnetcore-2.1)
