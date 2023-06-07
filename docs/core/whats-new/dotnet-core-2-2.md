---
title: .NET Core 2.2 的新增功能
description: 了解 .NET Core 2.2 的新增功能。
dev_langs:
- csharp
- vb
ms.date: 12/04/2018
ms.openlocfilehash: 656ef9aa2745c935c37b69ae5a54b8d126700e55
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608298"
---
# <a name="whats-new-in-net-core-22"></a>.NET Core 2.2 的新增功能

.NET Core 2.2 包括在应用程序部署、运行时服务的事件处理、Azure SQL 数据库的身份验证、JIT 编译器性能，以及执行 `Main` 方法之前的代码注入方面的增强功能。

## <a name="new-deployment-mode"></a>新部署模式

从 .NET Core 2.2 开始，可以部署[依赖于框架的可执行文件](../deploying/index.md#publish-framework-dependent)，这是“.exe”  文件而不是“.dll”  文件。 与依赖框架的部署在功能上类似，依赖框架的可执行文件 (FDE) 仍然依赖于存在的 .NET Core 的共享系统级版本来运行。 应用程序只包含代码和任何第三方依赖项。 与依赖框架的部署不同，FDE 特定于平台。

这种新的部署模式在构建可执行文件（而不是库）方面具有独特优势，这意味着你可以直接运行应用程序，而无需首先调用 `dotnet`。

## <a name="core"></a>核心

**在运行时服务中处理事件**

你可能经常希望监视应用程序的运行时服务（如 GC、JIT 和 ThreadPool）的使用情况，以了解它们如何影响应用程序。 在 Windows 系统上，这通常通过监视当前进程的 ETW 事件来完成。 虽然这仍然可以很好地工作，但是如果你在低特权环境中或者在 Linux 或 macOS 上运行，那么并不总是能够使用 ETW。

从 .NET Core 2.2 开始，现在可以使用 <xref:System.Diagnostics.Tracing.EventListener?displayProperty=nameWithType> 类来使用 CoreCLR 事件。 这些事件描述了诸如 GC、JIT、ThreadPool 和 interop 等运行时服务的行为。 这些事件与作为 CoreCLR ETW 提供程序的一部分公开的事件相同。  这允许应用程序使用这些事件或使用传输机制将它们发送到遥测聚合服务。 可以在以下代码示例中看到如何订阅事件：

```csharp
internal sealed class SimpleEventListener : EventListener
{
    // Called whenever an EventSource is created.
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        // Watch for the .NET runtime EventSource and enable all of its events.
        if (eventSource.Name.Equals("Microsoft-Windows-DotNETRuntime"))
        {
            EnableEvents(eventSource, EventLevel.Verbose, (EventKeywords)(-1));
        }
    }

    // Called whenever an event is written.
    protected override void OnEventWritten(EventWrittenEventArgs eventData)
    {
        // Write the contents of the event to the console.
        Console.WriteLine($"ThreadID = {eventData.OSThreadId} ID = {eventData.EventId} Name = {eventData.EventName}");
        for (int i = 0; i < eventData.Payload.Count; i++)
        {
            string payloadString = eventData.Payload[i]?.ToString() ?? string.Empty;
            Console.WriteLine($"\tName = \"{eventData.PayloadNames[i]}\" Value = \"{payloadString}\"");
        }
        Console.WriteLine("\n");
    }
}
```

此外，.NET Core 2.2 向 <xref:System.Diagnostics.Tracing.EventWrittenEventArgs> 类添加了以下两个属性来提供有关 ETW 事件的其他信息：

- <xref:System.Diagnostics.Tracing.EventWrittenEventArgs.OSThreadId?displayProperty=nameWithType>

- <xref:System.Diagnostics.Tracing.EventWrittenEventArgs.TimeStamp?displayProperty=nameWithType>

## <a name="data"></a>数据

**使用 SqlConnection.AccessToken 属性对 Azure SQL 数据库进行 AAD 身份验证**

从 .NET Core 2.2 开始，由 Azure Active Directory 颁发的访问令牌可用于对 Azure SQL 数据库进行身份验证。 若要支持访问令牌，必须将 <xref:System.Data.SqlClient.SqlConnection.AccessToken> 属性添加到 <xref:System.Data.SqlClient.SqlConnection> 类。 若要利用 AAD 身份验证，请下载 System.Data.SqlClient NuGet 包的版本 4.6。 若要使用功能，可以使用包含在 [`Microsoft.IdentityModel.Clients.ActiveDirectory`](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet 包中的[适用于 .NET 的 Active Directory 身份验证库](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)获取访问令牌值。

## <a name="jit-compiler-improvements"></a>JIT 编译器改进

**分层编译仍然是一项可选功能**

在 .NET Core 2.1，JIT 编译器实现了一项新的编译器技术，即“分层编译”  ，作为可选功能。 分层编译旨在提高性能。 由 JIT 编译器执行的重要任务之一是优化代码执行。 然而，对于很少使用的代码路径，相比执行未优化代码所花费的运行时，编译器可能需要更多的时间来优化代码。 分层编译介绍了 JIT 编译中的两个阶段：

- 第一层  ，将尽可能快地生成代码。

- 第二层  ，将为那些频繁执行的方法生成优化代码。 为了增强性能，第二层编译并行执行。

有关分层编译可能带来的性能改进的信息，请参阅[宣布发布 .NET Core 2.2 预览版 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-2-2-preview-2/)。

在 .NET Core 2.2 预览版 2 中，默认情况下已启用分层编译。 但是，我们仍然没有准备好在默认情况下启用分层编译。 因此在 .NET Core 2.2 中，分层编译仍是一项可选功能。 有关选择加入分层编译的信息，请参阅 [.NET Core 2.1 中的新增功能](dotnet-core-2-1.md)中的 [Jit 编译器改进](dotnet-core-2-1.md#jit-compiler-improvements)。

## <a name="runtime"></a>运行时

**在执行 Main 方法之前注入代码**

从 .NET Core 2.2 开始，可以使用启动挂钩注入代码，然后再运行应用程序的 Main 方法。 启动挂钩使主机可以在部署应用程序之后自定义其行为，而不需要重新编译或更改应用程序。

我们希望托管提供商定义自定义配置和策略，包括可能会影响主入口点加载行为的设置，如 <xref:System.Runtime.Loader.AssemblyLoadContext?displayProperty=nameWithType> 行为。 挂钩可用于设置跟踪或遥测注入，以设置回调进行处理，或定义其他环境相关的行为。 挂钩独立于入口点，因此不需要修改用户代码。

有关详细信息，请参阅[主机启动挂钩](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/host-startup-hook.md)。

## <a name="see-also"></a>请参阅

- [.NET Core 3.1 的新增功能](dotnet-core-3-1.md)
- [ASP.NET Core 2.2 的新增功能](/aspnet/core/release-notes/aspnetcore-2.2)
- [EF Core 2.2 中的新增功能](/ef/core/what-is-new/ef-core-2.2)
