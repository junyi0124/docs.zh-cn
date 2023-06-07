---
title: 运行时配置选项
description: 了解如何使用运行时配置设置来配置 .NET Core 应用程序。
ms.date: 01/21/2020
ms.openlocfilehash: 21673a221d0f21202febf4730b955da66132d5f7
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90538193"
---
# <a name="net-core-run-time-configuration-settings"></a>.NET Core 运行时配置设置

.NET Core 支持使用配置文件和环境变量在运行时配置 .NET Core 应用程序的行为。 如果出现以下情况，则运行时配置是一个不错的选择：

- 你不拥有或控制应用程序的源代码，因此无法以编程方式对其进行配置。

- 应用程序的多个实例在单个系统上同时运行，并且你想要将每个实例配置为获得最佳性能。

> [!NOTE]
> 本文档正在编写中。 如果你注意到此处提供的信息不完整或不准确，可以[创建一个问题](https://github.com/dotnet/docs/issues)告知我们，或[提交拉取请求](https://github.com/dotnet/docs/pulls)以解决问题。 要了解如何提交 dotnet/docs 存储库的拉取请求，请参阅[参与者指南](/contribute/dotnet/dotnet-contribute)。

.NET Core 提供了以下机制，它们可用于配置运行时应用程序行为：

- [runtimeconfig.json 文件](#runtimeconfigjson)

- [MSBuild 属性](#msbuild-properties)

- [环境变量](#environment-variables)

> [!TIP]
> 如果使用环境变量配置运行时选项，会将设置应用于所有 .NET Core 应用。 如果在 runtimeconfig.json 或项目文件中配置运行时选择，则仅将设置应用于此应用程序。

某些配置值还可以通过调用 <xref:System.AppContext.SetSwitch%2A?displayProperty=nameWithType> 方法以编程方式进行设置。

文档此部分的文章按类别组织，例如[调试](debugging-profiling.md)和[垃圾回收](garbage-collector.md)。 如果适用，将显示 runtimeconfig.json 文件、MSBuild 属性、环境变量的配置选项；对于 .NET Framework 项目，还会显示 app.config 文件的配置选项以便交叉引用。 

## <a name="runtimeconfigjson"></a>runtimeconfig.json

[构建](../tools/dotnet-build.md)项目时，将在输出目录中生成 *[appname].runtimeconfig.json* 文件。 如果项目文件所在的文件夹中存在 *runtimeconfig.template.json* 文件，它包含的任何配置选项都将合并到 *[appname].runtimeconfig.json* 文件中。 如果自行构建应用，请将所有配置选项放在 *runtimeconfig.template.json* 文件中。 如果只是运行应用，请将其直接插入 [appname].runtimeconfig.template.json 文件中。

> [!NOTE]
> 后续生成中将覆盖 [appname].runtimeconfig.template.json 文件。

在 runtimeconfig.json 文件的 configProperties 部分指定运行时配置选项。 此部分包含窗体：

```json
"configProperties": {
  "config-property-name1": "config-value1",
  "config-property-name2": "config-value2"
}
```

### <a name="example-appnameruntimeconfigjson-file"></a>示例 [appname].runtimeconfig.template.json 文件

如果要将这些选项放在输出 JSON 文件中，请将它们嵌套在 `runtimeOptions` 属性下。

```json
{
  "runtimeOptions": {
    "tfm": "netcoreapp3.1",
    "framework": {
      "name": "Microsoft.NETCore.App",
      "version": "3.1.0"
    },
    "configProperties": {
      "System.GC.Concurrent": false,
      "System.Threading.ThreadPool.MinThreads": 4,
      "System.Threading.ThreadPool.MaxThreads": 25
    }
  }
}
```

### <a name="example-runtimeconfigtemplatejson-file"></a>示例 runtimeconfig.template.json 文件

如果要将这些选项放在模板 JSON 文件中，请省略 `runtimeOptions` 属性。

```json
{
  "configProperties": {
    "System.GC.Concurrent": false,
    "System.Threading.ThreadPool.MinThreads": "4",
    "System.Threading.ThreadPool.MaxThreads": "25"
  }
}
```

## <a name="msbuild-properties"></a>MSBuild 属性

可以使用 SDK 样式 .NET Core 项目的 .csproj 或 .vbproj 文件中的 MSBuild 属性设置某些运行时配置选项。  MSBuild 属性优先于在 *runtimeconfig.template.json* 文件中设置的选项。 它们还会覆盖生成时在 [appname].runtimeconfig.json 文件中设置的任何选项。

下面是一个示例 SDK 样式项目文件，其中包含用于配置运行时行为的 MSBuild 属性：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
    <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
    <ThreadPoolMaxThreads>25</ThreadPoolMaxThreads>
  </PropertyGroup>

</Project>
```

用于配置运行时行为的 MSBuild 属性记录在每个区域各自的文章中，例如[垃圾回收](garbage-collector.md)。 它们还在 SDK 样式项目的 MSBuild 属性参考的[运行时配置](../project-sdk/msbuild-props.md#run-time-configuration-properties)部分中列出。

## <a name="environment-variables"></a>环境变量

环境变量可用于提供一些运行时配置信息。 如果使用环境变量配置运行时选项，会将设置应用于所有 .NET Core 应用。 指定为环境变量的配置旋钮通常有 COMPlus_ 前缀。

可以使用 Windows 控制面板、命令行或通过在 Windows 和 Unix 系统上调用 <xref:System.Environment.SetEnvironmentVariable(System.String,System.String)?displayProperty=nameWithType> 方法以编程方式定义环境变量。

下面的示例演示如何在命令行中设置环境变量：

```shell
# Windows
set COMPlus_GCRetainVM=1

# Powershell
$env:COMPlus_GCRetainVM="1"

# Unix
export COMPlus_GCRetainVM=1
```
