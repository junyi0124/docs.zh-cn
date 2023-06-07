---
title: 迁移新式桌面应用程序
description: 对于新式桌面应用程序的迁移过程，需要了解的所有内容。
ms.date: 05/12/2020
ms.openlocfilehash: f7862d6379eeeb737c386b5ffeaab938d258b046
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "97866472"
---
# <a name="migrating-modern-desktop-applications"></a>迁移新式桌面应用程序

在本章中，我们将探讨在将现有应用程序从 .NET Framework 迁移到 .NET Core 时可能会遇到的最常见问题。

复杂的桌面应用程序不能隔离，并且需要与可能驻留在本地计算机或远程服务器上的子系统进行某种类型的交互。 可能需要某种类型的数据库以本地或远程方式连接为永久性存储。 随着 Internet 和面向服务的体系结构的增加，通常将应用程序连接到驻留在远程服务器或云中的某种服务。 可能需要访问计算机文件系统才能实现某些功能。 或者，可能使用的是位于应用程序外的 COM 对象内的一项功能，例如，如果要在应用程序中集成 Office 程序集，这是一个常见方案。

此外，.NET Framework 和 .NET Core 公开的 API 图面中存在差异，并且在 .NET Framework 上可用的某些功能在 .NET Core 中不可用。 因此，在规划迁移时，必须了解并考虑这些问题。

## <a name="configuration-files"></a>配置文件

配置文件可用于存储在运行时读取的属性集，并影响我们的应用程序的行为，例如在何处查找数据库或执行循环的次数。 此方法的优点是，您可以修改应用程序的某些方面，而无需重新编码并重新编译。 例如，当同一应用程序代码在具有一组特定配置值的开发环境中运行时，以及在生产环境中使用不同的配置值运行时，这会很方便。

### <a name="configuration-on-net-framework"></a>.NET Framework 上的配置

如果使用的是工作 .NET Framework 桌面应用程序，则可能是通过 <xref:System.Configuration.AppSettingsSection> 命名空间中的类访问了app.config文件 `System.Configuration` 。

在 .NET Framework 基础结构中，有一个从其父项继承属性的配置文件的层次结构。 您可以找到一个 *machine.config* 文件，该文件定义了许多可在任何子代配置文件中使用或重写的属性和配置节。

### <a name="configuration-on-net-core"></a>.NET Core 上的配置

在 .NET Core 世界中，没有 *machine.config* 的文件。 即使您可以继续使用旧的传统 <xref:System.Configuration> 命名空间，您也可以考虑切换到新式 <xref:Microsoft.Extensions.Configuration> ，这提供了很多增强功能。

配置 API 支持配置提供程序的概念，该概念定义用于加载配置的数据源。 内置提供程序有不同种类，例如：

- 内存中的 .NET 对象
- INI 文件
- JSON 文件
- XML 文件
- 命令行参数
- 环境变量
- 加密的用户存储

 您也可以构建自己的。

新配置允许可分组为多层层次结构的名称-值对的列表。 任何存储的值都映射到一个字符串，并且提供内置的绑定支持，使你能够将设置反序列化为自定义的普通旧 CLR 对象 (POCO) 对象。

<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>对象允许你添加应用程序可能需要的多个配置提供程序，并使用优先规则解析首选项。 因此，在代码中添加的最后一个提供程序将覆盖其他提供程序。 这是一项很好的功能，可用于管理不同的执行环境，因为开发、测试和生产环境可以定义不同的配置，并在代码中的单个函数上管理它们。

### <a name="migrating-configuration-files"></a>正在迁移配置文件

你可以继续使用现有的 app.config XML 文件。 不过，你可以通过这种机会迁移配置，使其受益于 .NET Core 上的几项增强功能。

若要从旧样式的 *app.config* 迁移到新的配置文件，应选择 XML 格式和 JSON 格式。

如果选择 XML，则转换会很简单。 由于内容相同，因此，只需将 *app.config* 文件重命名为具有 XML 扩展名的文件即可。 然后，将引用 AppSettings 的代码更改为使用 `ConfigurationBuilder` 类。 此更改应该很简单。

如果要使用 JSON 格式，而不想手动迁移，则可以在 .NET Core 中使用一个名为 [dotnet-config2json](https://www.nuget.org/packages/dotnet-config2json/) 的工具，该工具可以将 *app.config* 文件转换为 JSON 配置文件。

使用 *machine.config* 文件中定义的配置节时，还可能会遇到一些问题。 例如，请考虑以下配置：

```xml
<configuration>
    <system.diagnostics>
        <switches>
            <add name="General" value="4" />
        </switches>
        <trace autoflush="true" indentsize="2">
            <listeners>
                <add name="myListener"
                     type="System.Diagnostics.TextWriterTraceListener,
                           System, Version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
                     initializeData="MyListener.log"
                     traceOutputOptions="ProcessId, LogicalOperationStack, Timestamp, ThreadId, Callstack, DateTime" />
            </listeners>
        </trace>
    </system.diagnostics>
</configuration>
```

如果将此配置用于 .NET Core，则会出现异常：

无法识别的配置节系统。诊断

发生此异常的原因是，该部分和负责处理该节的程序集已在 *machine.config* 文件中定义，该文件现在不存在。

若要轻松地解决此问题，可以将部分定义从旧的 *machine.config* 复制到新的配置文件中：

```xml
<configSections>
    <section name="system.diagnostics"
             type="System.Diagnostics.SystemDiagnosticsSection,
                   System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
</configSections>
```

## <a name="accessing-databases"></a>访问数据库

几乎每个桌面应用程序都需要某种类型的数据库。 对于桌面版，通常可以使用桌面应用程序和数据库引擎之间的直接连接来查找客户端-服务器体系结构。 这些数据库可以是本地的，也可以是远程的，具体取决于需要在不同用户之间共享信息。

从代码的角度来看，有许多技术和框架，使开发人员能够连接、查询和更新数据库。

当您谈论 Windows 桌面应用程序时，您可以找到的最常见数据库示例为 Microsoft Access 和 Microsoft SQL Server。 如果你的桌面编程经验超过20多年，ODBC、OLEDB、RDO、ADO、ADO.NET、LINQ 和实体框架等名称将非常熟悉。

### <a name="odbc"></a>ODBC

由于 Microsoft 提供 `System.Data.Odbc` 与 .NET Standard 2.0 兼容的库，因此你可以继续在 .Net Core 上使用 ODBC。

### <a name="ole-db"></a>OLE DB

[OLE DB](/previous-versions/windows/desktop/ms722784(v=vs.85))  是一种以统一方式访问各种数据源的好方法。 但它是基于 COM 的，后者是仅限 Windows 的技术，因此不适合用于 .NET Core 等跨平台技术。 在 SQL Server 版本2014及更高版本中也不受支持。 由于这些原因，.NET Core 不支持 OLE DB。

### <a name="adonet"></a>ADO.NET

你仍可以通过 .NET Core 上的现有桌面代码使用 ADO.NET。 只需更新某些 NuGet 包。

### <a name="ef-core-vs-ef6"></a>EF Core 与 EF6

当前支持的两个版本实体框架 (EF) ，实体框架 6 (EF6) 和 EF Core。

作为 .NET Framework 世界中发布的最新技术实体框架，6.4 是最新版本。 通过启动 .NET Core，Microsoft 还发布了基于实体框架的新数据访问堆栈，并将其称为 Entity Framework Core。

可以同时使用 .NET Framework 和 .NET Core 中的 EF 6.4 和 EF Core。 那么，有哪些决策驱动因素可帮助确定这两者之间的关系呢？

EF 6.3 是 EF6 的第一个版本，可在 .NET Core 上运行并跨平台工作。 事实上，此版本的主要目的是为了更轻松地将使用 EF6 的现有应用程序迁移到 .NET Core。

EF Core 旨在提供类似于 EF6 的开发人员体验。 大多数顶级 API 保持不变，因此，用过 EF6 的开发人员都会对 EF Core 感到很熟悉。

尽管兼容，但在做出决定之前，应检查实现情况。
有关详细信息，请参阅 [EF6 & 比较 EF Core](/ef/efcore-and-ef6/)。

建议在以下情况中使用 EF Core：

* 应用需要.NET Core 的功能。
* EF Core 支持应用需要的所有功能。

如果以下两个条件都成立，请考虑使用 EF6：

* 该应用程序将在 Windows 上运行，并 .NET Framework 4.0 或更高版本。
* EF6 支持应用需要的所有功能。

### <a name="relational-databases"></a>关系数据库

#### <a name="sql-server"></a>SQL Server

如果你在几年前就为桌面开发，SQL Server 是所选的一个数据库。 使用 <xref:System.Data.SqlClient> .NET Framework 中的，你可以访问 SQL Server 的版本，它封装了特定于数据库的协议。

在 .NET Core 中，可以找到 `SqlClient` 与 .NET Framework 中现有的类完全兼容的新类，但位于 <xref:Microsoft.Data.SqlClient> 库中。 只需添加对 [SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) NuGet 包的引用，并对命名空间进行一些重命名，一切都应按预期方式工作。

#### <a name="microsoft-access"></a>Microsoft Access

Microsoft Access 已用于多年，当不需要复杂的、可缩放的 SQL Server 时。 你仍可以使用库连接到 Microsoft Access <xref:System.Data.Odbc> 。

## <a name="consuming-services"></a>使用服务

随着面向服务的体系结构的引发，桌面应用程序开始从客户端-服务器模型发展为三层方法。 在客户端-服务器方法中，将建立直接的数据库连接，该客户端通常在单个 EXE 文件中包含业务逻辑。 另一方面，三层方法建立了一个实现业务逻辑和数据库访问的中间服务层，从而实现更好的安全性、可伸缩性和可重用性。 该层方法不是直接使用数据的数据集，而是依赖于一组实现协定和类型对象的服务来实现数据传输。

如果你有使用 WCF 服务的桌面应用程序，并且想要将其迁移到 .NET Core，则需要考虑一些事项。

第一件事是如何解析配置以访问服务。 因为 .NET Core 上的配置不同，所以需要在配置文件中进行一些更新。

其次，你需要通过 Visual Studio 2019 上提供的新工具重新生成服务客户端。 在此步骤中，您必须考虑激活同步操作的生成，以使客户端与现有代码兼容。

迁移后，如果你发现所需的库不存在于 .NET Core 上，则可以添加对 [Microsoft. 兼容性](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet 包的引用，并查看是否存在缺少的函数。

如果要使用 <xref:System.Net.WebRequest> 类来执行 web 服务调用，则可能会发现 .Net Core 有一些不同之处。 建议改为使用 HttpClient 的。

## <a name="consuming-a-com-object"></a>使用 COM 对象

目前，无法从 Visual Studio 2019 添加对 COM 对象的引用以与 .NET Core 一起使用。 因此，你必须手动修改项目文件。

`COMReference`在项目文件中插入结构，如以下示例中所示：

```xml
<ItemGroup>
    <COMReference Include="MSHTML">
        <Guid>{3050F1C5-98B5-11CF-BB82-00AA00BDCE0B}\</Guid>
        <VersionMajor>4</VersionMajor>
        <VersionMinor>0</VersionMinor>
        <Lcid>0</Lcid>
        <WrapperTool>primary</WrapperTool>
        <Isolated>false</Isolated>
    </COMReference>
</ItemGroup>
```

## <a name="more-things-to-consider"></a>更多注意事项

可用于 .NET Framework 库的多种技术不可用于 .NET Core。 如果你的代码依赖于其中一些技术，请考虑此部分中所述的替代方法。

[Windows 兼容性包](../../core/porting/windows-compat-pack.md)提供对以前仅对 .NET Framework 可用的 api 的访问权限。 它可用于 .NET Core 和 .NET Standard 项目。

有关 API 兼容性的详细信息，可以在中找到有关重大更改和弃用的 Api 的文档 <https://docs.microsoft.com/dotnet/core/compatibility/fx-core> 。

### <a name="appdomains"></a>AppDomain

应用程序域 (AppDomain) 可将应用相互隔离。 Appdomain 需要运行时支持，而且成本高昂。 不支持创建其他应用程序域。 对于代码隔离，建议将流程分隔开来或将容器用作一种替代方法。 对于程序集的动态加载，建议新  <xref:System.Runtime.Loader.AssemblyLoadContext> 类。

为了使代码迁移 .NET Framework 更简单，.NET Core 公开了某些 AppDomain API 图面。 某些 Api 函数通常 (例如  <xref:System.AppDomain.UnhandledException?displayProperty=nameWithType>) ，某些成员不执行任何操作 (例如  <xref:System.AppDomain.SetCachePath%2A>) ，其中一些成员引发 <xref:System.PlatformNotSupportedException> (例如  <xref:System.AppDomain.CreateDomain%2A>) 。

### <a name="remoting"></a>远程处理

.NET 远程处理用于跨 AppDomain 通信，这不再受支持。 同样，远程处理也需要运行时支持，进行维护的成本较高。 由于这些原因，.net Core 不支持 .NET 远程处理。

对于跨进程的通信，应考虑将进程间通信 (IPC) 机制，作为远程处理（如或类）的替代方法 <xref:System.IO.Pipes?displayProperty=nameWithType> <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> 。

对于跨计算机的通信，可将基于网络的解决方案用作备用方案。 最好使用低开销的明文协议，如 HTTP。 此处，ASP.NET Core 使用的 Web 服务器 Kestrel Web 服务器是一个选择。

### <a name="code-access-security-cas"></a>代码访问安全性 (CAS)

沙盒，它依赖于运行时或框架来约束托管应用程序或库使用或运行的资源，但在 .NET Core 中不受支持。

使用由操作系统提供的安全边界，如虚拟化、容器或用户帐户，用于以最小特权集运行进程。

### <a name="security-transparency"></a>安全透明度

与 CAS 相似，借助安全透明度可以以声明性方式将沙盒代码与安全关键代码隔离，但是不再支持将它作为安全边界。

使用由操作系统提供的安全边界，如虚拟化、容器或用户帐户，以便使用最少的权限集来运行进程。

>[!div class="step-by-step"]
>[上一页](whats-new-dotnet-core.md )
>[下一页](windows-migration.md)
