---
title: .NET Framework 技术在 .NET Core 和 .NET 5+ 上不可用
titleSuffix: ''
description: 了解在 .NET Core 和 .NET 5.0 及更高版本上不可用的 .NET Framework 技术。
author: cartermp
ms.date: 10/13/2020
ms.openlocfilehash: 492aace9db3dc3acef18e995f10b7b5fbe251558
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161031"
---
# <a name="net-framework-technologies-unavailable-on-net-core-and-net-5"></a>.NET Framework 技术在 .NET Core 和 .NET 5+ 上不可用

一些适用于 .NET Framework 库的技术不可用于 .NET Core 和 .NET 5.0 及更高版本，例如应用域、远程处理、代码访问安全性 (CAS)、安全透明度和 <xref:System.EnterpriseServices?displayProperty=fullName>。 如果库依赖于这些技术中的一个或多个，请考虑使用下面所述的替代方法。 有关 API 兼容性的详细信息，请参阅 [.NET 中断性变更](../compatibility/breaking-changes.md)。

> [!TIP]
> 当前未实现某个 API 或技术并不因此意味着有意不对其提供支持。 搜索 .NET GitHub 存储库，查看所遇到的特定问题是否是特意设计的。 如果找不到此类指示器，请在 [dotnet/runtime 存储库](https://github.com/dotnet/runtime/issues)中提出问题，请求提供特定 API 和技术。

## <a name="application-domains"></a>应用程序域

应用程序域 (AppDomain) 可将应用相互隔离。 AppDomain 需要运行时支持并且通常价格昂贵。 不支持创建其他应用域，也尚未计划在将来添加此功能。 对于代码隔离，将流程或容器用作备用。 若要动态加载程序集，请使用 <xref:System.Runtime.Loader.AssemblyLoadContext> 类。

.NET 5+ 公开了一些 <xref:System.AppDomain> API 曲面，以便可以更轻松地从 .NET Framework 进行代码迁移。 一些 API 可正常工作（例如 <xref:System.AppDomain.UnhandledException?displayProperty=nameWithType>），一些成员不会执行任何操作（例如 <xref:System.AppDomain.SetCachePath%2A>），也有一些会引发 <xref:System.PlatformNotSupportedException>（例如 <xref:System.AppDomain.CreateDomain%2A>）。 对照 [dotnet/runtime GitHub 存储库](https://github.com/dotnet/runtime)中的 [`System.AppDomain` 引用源](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Private.CoreLib/src/System/AppDomain.cs)检查所使用的类型。 确保选择与已实现的版本相匹配的分支。

## <a name="remoting"></a>远程处理

.NET 远程处理被认为是存在问题的体系结构。 它用于在不再受支持的应用程序域之间进行通信。 同样，远程处理也需要运行时支持，进行维护的成本较高。 出于这些原因，.NET Core 和 .NET 5+ 不支持 .NET 远程处理，并且我们不计划在将来添加对它的支持。

对于跨进程通信，可将进程间通信 (IPC) 机制视为远程处理的备用方案，如 <xref:System.IO.Pipes> 类或 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> 类。

对于跨计算机的通信，可将基于网络的解决方案用作备用方案。 最好使用低开销纯文本协议，例如 HTTP。 此处，ASP.NET Core 使用的 Web 服务器 [Kestrel Web 服务器](/aspnet/core/fundamentals/servers/kestrel)是一个选择。 也可考虑将 <xref:System.Net.Sockets> 用于基于网络的跨计算机的方案。 有关更多选项，请参阅 [.NET 开放源代码开发人员项目：消息传送](https://github.com/Microsoft/dotnet/blob/master/dotnet-developer-projects.md#messaging)。

## <a name="code-access-security-cas"></a>代码访问安全性 (CAS)

沙盒依赖为托管应用程序或库使用或运行提供资源的运行时或框架进行限制，其[在 .NET Framework 上不受支持](../../framework/misc/code-access-security.md)，因此在 .NET Core 和 .NET 5+ 上也不受支持。 .NET Framework 和运行时中存在太多发生特权提升以继续将 CAS 视为安全边界的情况。 此外，CAS 使实现更加复杂，通常会对无意使用它的应用程序造成正确性-性能影响。

可使用操作系统提供的安全边界，例如虚拟化、容器或具有最少特权集的用于运行进程的用户帐户。

## <a name="security-transparency"></a>安全透明度

与 CAS 相似，借助安全透明度可以通过声明性方式将沙盒代码与安全关键代码隔离，但是[不再支持将它作为安全边界](../../framework/misc/security-transparent-code.md)。 Silverlight 大规模使用了此功能。

可使用操作系统提供的安全边界，例如虚拟化、容器或用于运行进程的用户帐户具有最少的一组特权。

## <a name="systementerpriseservices"></a>System.EnterpriseServices

.NET Core 和 .NET 5+ 不支持 <xref:System.EnterpriseServices?displayProperty=fullName> (COM+)。

## <a name="see-also"></a>请参阅

- [有关从 .NET Framework 移植到 .NET Core 的概述](index.md)
