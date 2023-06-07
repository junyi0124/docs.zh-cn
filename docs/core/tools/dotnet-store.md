---
title: dotnet store 命令
description: “dotnet store”命令可将指定的程序集存储到运行时包存储区。
ms.date: 02/14/2020
ms.openlocfilehash: 8efb11c6bf648bc7787d5627e02b180abb8a0afd
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634332"
---
# <a name="dotnet-store"></a>dotnet store

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>名称

`dotnet store` - 将指定的程序集存储到[运行时包存储区](../deploying/runtime-store.md)。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet store -m|--manifest <PATH_TO_MANIFEST_FILE>
    -f|--framework <FRAMEWORK_VERSION> -r|--runtime <RUNTIME_IDENTIFIER>
    [--framework-version <FRAMEWORK_VERSION>] [--output <OUTPUT_DIRECTORY>]
    [--skip-optimization] [--skip-symbols] [-v|--verbosity <LEVEL>]
    [--working-dir <WORKING_DIRECTORY>]

dotnet store -h|--help
```

## <a name="description"></a>说明

`dotnet store` 将指定的程序集存储到[运行时包存储区](../deploying/runtime-store.md)。 默认情况下，程序集更适用于目标运行时和框架。 有关详细信息，请参阅[运行时包存储区](../deploying/runtime-store.md)主题。

## <a name="required-options"></a>必需选项

- **`-f|--framework <FRAMEWORK>`**

  指定[目标框架](../../standard/frameworks.md)。 目标框架必须在项目文件中进行指定。

- **`-m|--manifest <PATH_TO_MANIFEST_FILE>`**

  包存储区清单文件  是包含要存储的包列表的 XML 文件。 清单文件的格式与 SDK 样式项目格式兼容。 因此，引用所需的包的项目文件能够与 `-m|--manifest` 选项结合使用，以便于将程序集存储到运行时包存储区。 若要指定多个清单文件，请为各个文件重复指定选项和路径。 例如：`--manifest packages1.csproj --manifest packages2.csproj`。

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  目标[运行时标识符](../rid-catalog.md)。

## <a name="optional-options"></a>可选选项

- **`--framework-version <FRAMEWORK_VERSION>`**

  指定 .NET SDK 版本。 使用此选项，可以选择特定的框架版本，不再局限于 `-f|--framework` 选项指定的框架。

- **`-h|--help`**

  显示帮助信息。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  指定运行时包存储区的路径。 如果未指定，默认路径为用户配置文件 .NET 安装目录的 store 子目录。

- **`--skip-optimization`**

  跳过优化阶段。

- **`--skip-symbols`**

  跳过符号生成。 目前，只能在 Windows 和 Linux 上生成符号。

- **`-v|--verbosity <LEVEL>`**

  设置命令的详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。

- **`-w|--working-dir <WORKING_DIRECTORY>`**

  此命令使用的工作目录。 如果未指定，使用当前目录的 obj  子目录。

## <a name="examples"></a>示例

- 存储 packages.csproj  项目文件中为 .NET Core 2.0.0 指定的包：

  ```dotnetcli
  dotnet store --manifest packages.csproj --framework-version 2.0.0
  ```

- 存储 packages.csproj  中指定的包，但不进行优化：

  ```dotnetcli
  dotnet store --manifest packages.csproj --skip-optimization
  ```

## <a name="see-also"></a>另请参阅

- [运行时包存储](../deploying/runtime-store.md)
