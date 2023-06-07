---
title: .NET Framework 的配置文件架构
ms.date: 05/01/2017
helpviewer_keywords:
- .NET Framework application configuration, configuration schema
- machine configuration files
- application configuration files, schema
- app.config files, schema
- schema configuration settings
- schemas, configuration settings
- enterprisesec.config files
- well-formed XML
- enterprisesec configuration files
- security.config files
- security [.NET Framework], configuration files
- application development [.NET Framework], schema
- XML tags
- container tags
- machine.config files
- configuration schema [.NET Framework]
- configuration settings [.NET Framework], applications
- configuration file reference [.NET Framework]
ms.assetid: 69003d39-dc8a-460c-a6be-e6d93e690b38
ms.openlocfilehash: ab6f12be01899f5b7e54a7ec2d9675d502d88bc3
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555128"
---
# <a name="configuration-file-schema-for-the-net-framework"></a>.NET Framework 的配置文件架构

配置文件是可用来更改应用设置并为其设置策略的标准 XML 文件。 .NET Framework 配置架构由一些可在配置文件中用来控制应用行为的元素组成。 本节的目录反映了架构的层次结构：启动、运行时、网络和其他配置设置类型。

有关配置文件类型、格式和位置的信息，请参阅 [配置应用](../index.md)。 若要直接编辑配置文件，需要自行熟悉 XML。

> [!IMPORTANT]
> 配置文件中的 XML 标记和特性区分大小写。

## <a name="in-this-section"></a>在本节中

[**\<configuration>** Element](configuration-element.md)\
所有配置文件的顶级元素。

[**\<assemblyBinding>** Element](assemblybinding-element-for-configuration.md)\
指定配置级的程序集绑定策略。

[**\<linkedConfiguration>** Element](linkedconfiguration-element.md)\
指定要包含的配置文件。

[启动设置架构](./startup/index.md)\
指定要使用的公共语言运行时版本的元素。

[运行时设置架构](./runtime/index.md)\
配置程序集绑定和运行时行为的元素。

[网络设置架构](./network/index.md)\
指定 .NET Framework 如何连接到 internet 的元素。

[加密设置架构](./cryptography/index.md)\
将友好算法名称映射到实现密码算法的类的元素。

[配置节架构](configuration-sections-schema.md)\
用于创建和使用自定义设置的配置节的元素。

[跟踪和调试设置架构](./trace-debug/index.md)\
用于指定跟踪开关和侦听器的元素。

[编译器和语言提供程序设置架构](./compiler/index.md)\
指定可用语言提供程序的编译器配置的元素。

[应用程序设置架构](application-settings-schema.md)\
使 Windows 窗体或 ASP.NET 应用程序可以存储和检索应用程序范围的设置和用户范围的设置的元素。

[应用设置架构](./appsettings/index.md)\
包含自定义应用程序设置，如文件路径、XML Web service URL 或应用程序的任何其他自定义配置信息。

[Web 设置架构](./web/index.md)\
用于配置 ASP.NET 如何与主机应用程序（如 IIS）一起工作的元素。 在 Aspnet.config** 文件中使用。

[Windows 窗体配置架构](winforms/index.md)\
Windows 窗体应用程序配置部分中的所有元素，包括多监视器和高 DPI 支持等自定义。

[WCF 配置架构](./wcf/index.md)\
使你能够配置 WCF 服务和客户端应用程序的所有元素。

[WCF 指令语法](./wcf-directive/index.md)\
介绍 `@ServiceHost` 指令，该指令定义 .svc 编译器使用的特定于页的属性。

[WIF 配置架构](windows-identity-foundation/index.md)\
Windows Identity Foundation (WIF) 配置架构中的所有元素。

## <a name="related-sections"></a>相关章节

[远程处理设置架构](/previous-versions/dotnet/netframework-4.0/z415cf9a(v=vs.100))\
描述对实现远程处理的客户端和服务器应用程序进行配置的元素。

[ASP.NET 设置架构](/previous-versions/dotnet/netframework-4.0/b5ysx397(v=vs.100))\
描述控制 ASP.NET Web 应用程序的行为的元素。

[Web 服务设置架构](/previous-versions/dotnet/netframework-4.0/cctwteet(v=vs.100))\
描述控制 ASP.NET Web 服务以及它们的客户端的行为的元素。

[配置 .NET Framework 应用](/previous-versions/dotnet/netframework-4.0/kza1yk3a(v=vs.100))\
描述如何在 .NET Framework 中配置安全性、程序集绑定和远程处理。
