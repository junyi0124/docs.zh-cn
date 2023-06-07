---
title: 提升的 Dotnet 命令访问权限
description: 了解需要提升访问权限的 dotnet 命令的最佳做法。
author: wli3
ms.date: 06/26/2019
ms.openlocfilehash: b34a4d631ec0e5ef641e1ffbc91e081d25645157
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634046"
---
# <a name="elevated-access-for-dotnet-commands"></a>提升的 Dotnet 命令访问权限

开发人员可根据软件开发最佳做法来编写需要最少权限的软件。 但是，某些软件（如性能监视工具）由于操作系统规则，需要管理员权限。 以下指南介绍使用 .NET Core 编写此类软件的适用方案。

可以运行以下提升的命令：

- `dotnet tool` 命令，如 [dotnet tool install](dotnet-tool-install.md)。
- `dotnet run --no-build`
- `dotnet-core-uninstall`

不建议运行其他提升的命令。 具体而言，不建议为使用 MSBuild（例如，[dotnet restore](dotnet-restore.md)、[dotnet build](dotnet-build.md) 和 [dotnet run](dotnet-run.md)）的命令提升访问权限。 主要问题是用户在发出 dotnet 命令后在根帐户和受限帐户之间来回切换时存在权限管理问题。 受限用户可能会发现自己无法访问根用户构建的文件。 有办法可以解决这种情况，但不一定要使用这些方法。

只要不在根帐户和受限帐户之间来回切换，就能够以根帐户的身份运行命令。 例如，Docker 容器默认以根帐户身份运行，因此它们具有此特性。

## <a name="global-tool-installation"></a>全局工具安装

以下说明展示了执行下述操作的推荐方法：安装、运行和卸载需要提升权限才能执行的 .NET 工具。

<!-- markdownlint-disable MD025 -->

# <a name="windows"></a>[Windows](#tab/windows)

### <a name="install-the-tool"></a>安装工具

如果文件夹 `%ProgramFiles%\dotnet-tools` 已存在，请执行以下操作以检查“用户”组是否有写入或修改该目录的权限：

- 右键单击 `%ProgramFiles%\dotnet-tools` 文件夹并选择“属性”  。 随即打开“常用属性”对话框  。
- 选择“安全性”选项卡。在“组或用户名”下，检查“用户”组是否具有写入或修改目录的权限。
- 如果“用户”组可以写入或修改目录，则在安装工具时使用其他目录名，而不使用 dotnet-tools  。

要安装工具，请在提升的提示符下运行以下命令。 此操作将在安装期间创建 dotnet-tools 文件夹  。

```dotnetcli
dotnet tool install PACKAGEID --tool-path "%ProgramFiles%\dotnet-tools".
```

### <a name="run-the-global-tool"></a>运行全局工具

**选项 1** 在提升的提示符中使用完整路径：

```cmd
"%ProgramFiles%\dotnet-tools\TOOLCOMMAND"
```

**选项 2** 将新创建的文件夹添加到 `%Path%`。 只需执行此操作一次。

```cmd
setx Path "%Path%;%ProgramFiles%\dotnet-tools\"
```

然后使用以下命令运行：

```cmd
TOOLCOMMAND
```

### <a name="uninstall-the-global-tool"></a>卸载全局工具

在提升的提示符处，键入下列命令：

```dotnetcli
dotnet tool uninstall PACKAGEID --tool-path "%ProgramFiles%\dotnet-tools"
```

# <a name="linux"></a>[Linux](#tab/linux)

[!INCLUDE [elevated-access-unix](../../../includes/elevated-access-unix.md)]

# <a name="macos"></a>[macOS](#tab/macos)

[!INCLUDE [elevated-access-unix](../../../includes/elevated-access-unix.md)]

---

## <a name="local-tools"></a>本地工具

本地工具的作用域按用户和个子目录树来限定。 执行特权运行后，本地工具将受限的用户环境共享给提升的环境。 在 Linux 和 macOS 中，这会导致将文件设置为仅限根用户访问。 如果用户切换回受限帐户，则用户无法再访问或写入文件。 因此，不建议将必须提升的工具安装为本地工具。 建议使用 `--tool-path` 选项和上述全局工具指南。

## <a name="elevation-during-development"></a>开发过程中的提升

在开发过程中，可能需要提升访问权限才能测试应用程序。 （例如）IoT 应用就常存在这种情况。 建议在构建应用程序时不要进行提升，而是在运行时使用提升。 有几种模式，如下所示：

- 使用生成的可执行文件（它提供最佳的启动性能）：

   ```dotnetcli
   dotnet build
   sudo ./bin/Debug/netcoreapp3.0/APPLICATIONNAME
   ```

- 使用 [dotnet run](dotnet-run.md) 命令与 `—no-build` 标志，以避免生成新的二进制文件：

   ```dotnetcli
   dotnet build
   sudo dotnet run --no-build
   ```

## <a name="see-also"></a>请参阅

- [.NET 工具概述](global-tools.md)
