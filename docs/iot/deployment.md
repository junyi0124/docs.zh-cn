---
title: 将 .NET 应用部署到 Raspberry Pi
description: 了解如何将 .NET 应用部署到 Raspberry Pi。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: how-to
ms.prod: dotnet
ms.openlocfilehash: 4da558e2cdfc283d21f2a5497cb71dc746109614
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96591085"
---
# <a name="deploy-net-apps-to-raspberry-pi"></a>将 .NET 应用部署到 Raspberry Pi

将 .NET 应用程序部署到 Raspberry Pi 与任何其他平台相同。 您的应用程序可以作为 *独立* 或 *依赖于框架的* 部署模式运行。 每个策略都有一些优点。 有关详细信息，请参阅 [.net 应用程序发布概述](../core/deploying/index.md)。

## <a name="deploying-a-framework-dependent-app"></a>部署依赖框架的应用

若要将应用部署为依赖框架的应用，请完成以下步骤：

1. [!INCLUDE [ensure-ssh](includes/ensure-ssh.md)]

1. 使用 [dotnet 脚本](../core/tools/dotnet-install-script.md)在 Raspberry Pi 上安装 .net。 在 Raspberry Pi 上的 Bash 提示符 (本地或 SSH) 完成以下步骤：
    1. 运行以下命令以安装 .NET：

        ```bash
        curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin
        ```

        > [!NOTE]
        > 这会安装最新版本。 如果需要特定版本，请将添加 `--version <VERSION>` 到末尾，其中 `<VERSION>` 是特定的生成版本。

    1. 若要简化路径解析，请添加 `DOTNET_ROOT` 环境变量，并使用以下命令将 *dotnet* 目录添加到 `$PATH` ：

        ```bash
        echo 'export DOTNET_ROOT=$HOME/.dotnet' >> ~/.bashrc
        echo 'export PATH=$PATH:$HOME/.dotnet' >> ~/.bashrc
        source ~/.bashrc
        ```

    1. 用以下命令验证 .NET 安装：

        ```dotnetcli
        dotnet --version
        ```

        验证显示的版本是否与安装的版本匹配。

1. 按照以下步骤，在开发计算机上发布应用程序，具体取决于开发环境。
    - 如果使用的是 **Visual Studio**，请将 [应用程序部署到本地文件夹](/visualstudio/deployment/quickstart-deploy-to-local-folder?view=vs-2019)。 在发布之前，在 "发布配置文件摘要" 中选择 " **编辑** "，然后选择 " **设置** " 选项卡。确保将 **部署模式** 设置为 " *依赖于框架* "，并将 " **目标运行时** " 设置为 " *可移植*"。
    - 如果使用 **.NET CLI**，请使用 [dotnet publish](../core/tools/dotnet-publish.md) 命令。 不需要其他参数。

1. [!INCLUDE [sftp-client](includes/sftp-client.md)]

1. 在 Raspberry Pi 上的 Bash 提示符 (本地或 SSH) 上，运行应用。 为此，请将部署文件夹设置为当前目录，并执行以下命令 (其中 *HelloWorld.dll* 是应用) 的入口点：

    ```dotnetcli
    dotnet HelloWorld.dll
    ```

## <a name="deploying-a-self-contained-app"></a>部署自包含应用

若要将应用部署为自包含应用，请完成以下步骤：

1. [!INCLUDE [ensure-ssh](includes/ensure-ssh.md)]

1. 按照以下步骤，在开发计算机上发布应用程序，具体取决于开发环境。
    - 如果使用的是 **Visual Studio**，请将 [应用程序部署到本地文件夹](/visualstudio/deployment/quickstart-deploy-to-local-folder?view=vs-2019)。 在发布之前，在 "发布配置文件摘要" 中选择 " **编辑** "，然后选择 " **设置** " 选项卡。确保将 " **部署模式** " 设置为 " *自包含* "，并将 " **目标运行时** " 设置为 " *linux-arm*"。
    - 如果使用的是 **.NET CLI**，请将 [dotnet publish](../core/tools/dotnet-publish.md) 命令与 `-r linux-arm` 参数一起使用：

        ```dotnetcli
        dotnet publish -r linux-arm
        ```

1. [!INCLUDE [sftp-client](includes/sftp-client.md)]

1. 在 Raspberry Pi 上的 Bash 提示符 (本地或 SSH) 上，运行应用。 为此，请将当前目录设置为部署位置并完成以下步骤：
    1. 为 *可执行* 文件 (指定) 的可执行 `HelloWorld` 文件的名称。

        ```bash
        chmod +x HelloWorld
        ```

    1. 运行可执行文件。

        ```bash
        ./HelloWorld
        ```
