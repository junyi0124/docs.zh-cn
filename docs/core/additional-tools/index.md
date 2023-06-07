---
title: 其他工具
description: 概述了可安装的支持和扩展 .NET Core 功能的其他工具。
author: mlacouture
ms.date: 02/13/2020
ms.custom: mvc
ms.openlocfilehash: 6aa8b62f02c4325664ffeccc0c0d4a0635a96f2d
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599214"
---
# <a name="net-core-additional-tools-overview"></a>.NET Core 附加工具概述

本节除了 .NET Core CLI 外，还编译了可支持和扩展 .NET Core 功能的工具列表。

## <a name="net-core-uninstall-tool"></a>.NET Core 卸载工具

使用 [.NET Core 卸载工具](https://github.com/dotnet/cli-lab/releases) (`dotnet-core-uninstall`)，可清理系统上的 .NET Core SDK 和运行时，以便仅保留指定的版本。 可使用选项集合来指定要卸载的版本。

## <a name="net-core-diagnostic-tools"></a>.NET Core 诊断工具

[dotnet-counters](../diagnostics/dotnet-counters.md) 是一个性能监视工具，用于初级运行状况监视和性能调查。

通过 [dotnet-dump](../diagnostics/dotnet-dump.md)，可在不使用本机调试器的情况下收集和分析 Windows 和 Linux 核心转储。

[dotnet-gcdump](../diagnostics/dotnet-gcdump.md) 提供为活动 .NET 进程收集 GC（垃圾回收器）转储的方式。

[dotnet-trace](../diagnostics/dotnet-trace.md) 会从你的应用收集分析数据，这些数据可帮助你了解应用运行速度缓慢的原因。

## <a name="net-install-tool-for-extension-authors"></a>适用于扩展创建者的 .NET 安装工具

[适用于扩展创建者的 .NET 安装工具](https://github.com/dotnet/vscode-dotnet-runtime)是一种专门供 VS Code 扩展创建者获取 .NET Core 运行时的 Visual Studio Code 扩展。 此工具专门供采用 .NET 编写并且需要 .NET 启动其各个部分的扩展（例如语言服务器）使用。 此扩展并非直接供用户用来安装用于开发的 .NET。

## <a name="wcf-web-service-reference-tool"></a>WCF Web Service Reference 工具

WCF (Windows Communication Foundation) [Web ervice Reference 工具](wcf-web-service-reference-guide.md)是一个 Visual Studio 连接服务提供程序，首次推出是在 [Visual Studio 2017 版本 15.5](/visualstudio/releasenotes/vs2017-relnotes-v15.5#WCFTools) 中。 此工具可从网络位置上当前解决方案的 Web 服务中，或从 WSDL 文件中检索元数据。 还可生成与 .NET Core 兼容的源文件并使用可用于访问 Web 服务操作的方法定义 WCF 代理类。

## <a name="wcf-dotnet-svcutil-tool"></a>WCF dotnet-svcutil 工具

WCF [dotnet-svcutil 工具](dotnet-svcutil-guide.md)是一个 .NET 工具，可从网络位置上的 Web 服务中或从 WSDL 文件中检索元数据。 还可生成与 .NET Core 兼容的源文件并使用可用于访问 Web 服务操作的方法定义 WCF 代理类。

dotnet-svcutil 工具是 [WCF Web Service Reference](wcf-web-service-reference-guide.md) Visual Studio 连接服务提供程序（随 Visual Studio 2017 版本 15.5 首次推出）的替代产品。 dotnet-svcutil 工具作为一种 .NET 工具，可用于 Linux、macOS 和 Windows。

## <a name="wcf-dotnet-svcutilxmlserializer-tool"></a>WCF dotnet-svcutil.xmlserializer 工具

在 .NET Framework 中，可以使用 svcutil 工具预生成序列化程序集。 WCF [dotnet-svcutil.xmlserializer 工具](dotnet-svcutil.xmlserializer-guide.md)在 .NET Core 上提供类似的功能。 它为客户端应用程序中 WCF 服务协定使用且可由 <xref:System.Xml.Serialization.XmlSerializer> 序列化的类型预生成 C# 序列化代码。 当序列化或反序列化这些类型的对象时，这会提高 XML 序列化的启动性能。

## <a name="xml-serializer-generator"></a>XML 序列化程序生成器

正如 [XML 序列化程序生成器工具 (Sgen.exe)](../../standard/serialization/xml-serializer-generator-tool-sgen-exe.md) 适用于 .NET Framework，[Microsoft.XmlSerializer.Generator NuGet 包](https://www.nuget.org/packages/Microsoft.XmlSerializer.Generator) 是适用于 .NET Core 和 .NET 标准库的解决方案。 它为程序集中包含的类型创建 XML 序列化程序集，从而提高使用 <xref:System.Xml.Serialization.XmlSerializer> 序列化或反序列化这些类型对象时，XML 序列化的启动性能。

## <a name="generating-self-signed-certificates"></a>生成自签名证书

可以使用 [dotnet dev-certs](self-signed-certificates-guide.md) 创建用于开发和测试方案的自签名证书。
