---
title: dotnet clean 命令
description: dotnet clean 命令可清除当前目录。
ms.date: 02/14/2020
ms.openlocfilehash: a59922feba75e940a5cee2dfeb500f4f86372870
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463705"
---
# <a name="dotnet-clean"></a>dotnet clean

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>名称

`dotnet clean` - 清除项目输出。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet clean [<PROJECT>|<SOLUTION>] [-c|--configuration <CONFIGURATION>]
    [-f|--framework <FRAMEWORK>] [--interactive]
    [--nologo] [-o|--output <OUTPUT_DIRECTORY>]
    [-r|--runtime <RUNTIME_IDENTIFIER>] [-v|--verbosity <LEVEL>]

dotnet clean -h|--help
```

## <a name="description"></a>说明

`dotnet clean` 命令可清除上一个生成的输出。 它以 [MSBuild 目标](/visualstudio/msbuild/msbuild-targets) 的形式实现，以便在运行命令时对项目进行评估。 只会清除在生成过程中创建的输出。 中间 (*obj*) 和最终输出 (*bin*) 文件夹都会被清除。

## <a name="arguments"></a>参数

`PROJECT | SOLUTION`

要清理的 MSBuild 项目或解决方案。 如果未指定项目或解决方案文件，MSBuild 会在当前工作目录中搜索文件扩展名以 *proj* 或 *sln* 结尾的文件并使用该文件。

## <a name="options"></a>选项

* **`-c|--configuration <CONFIGURATION>`**

  定义生成配置。 大多数项目的默认配置为 `Debug`，但你可以覆盖项目中的生成配置设置。 只有在生成期间指定了此选项，才必须在清除时使用此选项。

* **`-f|--framework <FRAMEWORK>`**

  在生成时指定的[框架](../../standard/frameworks.md)。 必须在[项目文件](csproj.md)中定义该框架。 如果在生成时指定了框架，则必须在清除时指定框架。

* **`-h|--help`**

  打印出有关命令的简短帮助。

* **`--interactive`**

  允许命令停止并等待用户输入或操作。 例如，完成身份验证。 自 .NET Core 3.0 SDK 起可用。

* **`--nologo`**

  不显示启动版权标志或版权消息。 自 .NET Core 3.0 SDK 起可用。

* **`-o|--output <OUTPUT_DIRECTORY>`**

  包含要清理的生成项目的目录。 如果在生成项目时指定了框架，则使用输出目录开关指定 `-f|--framework <FRAMEWORK>` 开关。

* **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  清除指定运行时的输出文件夹。 在创建[独立部署 (SCD)](../deploying/index.md#publish-self-contained) 时使用此选项。

* **`-v|--verbosity <LEVEL>`**

  设置 MSBuild 详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。

## <a name="examples"></a>示例

* 清除项目的默认生成：

  ```dotnetcli
  dotnet clean
  ```

* 清除使用版本配置生成的项目：

  ```dotnetcli
  dotnet clean --configuration Release
  ```
