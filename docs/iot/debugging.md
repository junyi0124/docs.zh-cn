---
title: 在 Raspberry Pi 上调试 .NET 应用
description: 了解如何在 Raspberry Pi 和类似设备上调试 .NET 应用。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: how-to
ms.prod: dotnet
zone_pivot_groups: ide-set-one
ms.openlocfilehash: 7b9872304ee53071452772e3da02081a7def4d80
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "96591048"
---
# <a name="debug-net-apps-on-raspberry-pi"></a>在 Raspberry Pi 上调试 .NET 应用

调试在基于 ARM 的 IoT 设备上运行的 .NET 应用（如 Raspberry Pi）带来了独特的挑战。 可以在 ARM 设备上开发 .NET 应用程序。 但是，在 Visual Studio 外调试 .NET 应用程序所需的 OmniSharp 与 ARM 设备不兼容。 因此，必须从兼容的平台远程调试应用程序。

::: zone pivot="vscode"

## <a name="debug-from-visual-studio-code-cross-platform"></a>从 Visual Studio Code (跨平台) 调试

若要从 Visual Studio Code 调试 Raspberry Pi 上的 .NET，则需要在 Raspberry Pi 上以及项目的 *launch.js文件上* 执行配置步骤。

### <a name="enable-ssh-on-the-raspberry-pi"></a>在 Raspberry Pi 上启用 SSH

远程调试需要 SSH。 若要启用 SSH，请 [参阅 Raspberry Pi 文档中的 *enable ssh*](https://www.raspberrypi.org/documentation/remote-access/ssh/) <span class="docon docon-navigate-external x-hidden-focus"></span> 。

### <a name="install-the-visual-studio-remote-debugger-on-the-raspberry-pi"></a>在 Raspberry Pi 上安装 Visual Studio 远程调试器

在 Raspberry Pi 上的 Bash 控制台 (本地或通过 SSH) ，请完成以下步骤：

1. 执行以下命令以下载并安装 Raspberry Pi 上的 Visual Studio 远程调试器：

    ```bash
    curl -sSL https://aka.ms/getvsdbgsh | /bin/sh /dev/stdin -v latest -l ~/vsdbg
    ```

1. 调试器需要作为运行 `root` 。 默认情况下， `root` 在 Raspberry Pi 上没有密码。 `root`通过执行以下命令并按照提示设置密码。

    ```bash
    sudo passwd root
    ```

1. Visual Studio Code 使用 SSH 协议进行远程调试。 出于安全考虑， `root` 默认情况下不允许通过 SSH 登录。 若要启用 `root` 通过 SSH 登录，请完成以下步骤：

    1. 执行以下命令以在 [nano](https://www.nano-editor.org/docs.php)中打开 */etc/ssh/sshd_config* <span class="docon docon-navigate-external x-hidden-focus"></span> 。

        ```bash
        sudo nano /etc/ssh/sshd_config
        ```

    1. 按 <kbd>Ctrl + W</kbd> 并搜索 **PermitRootLogin**。
    1. 如果需要，则取消注释行的 **PermitRootLogin** 。
    1. 按如下所示将行更改为：

        ```console
        PermitRootLogin yes
        ```

        > [!NOTE]
        > 如果 */etc/ssh/sshd_config* 中没有 **PermitRootLogin** 行，请使用上面所示的值添加新行。

    1. 按 <kbd>Ctrl + X</kbd> 退出并节省机会。 当系统提示 **"保存已修改的缓冲区？**" 时，按 <kbd>Y</kbd> 确认。 按 <kbd>enter</kbd> 确认覆盖原始文件。
    1. 通过执行以下命令重新启动 Raspberry Pi：

        ```bash
        sudo reboot
        ```

### <a name="setup-launchjson-in-visual-studio-code"></a>在 Visual Studio Code 中设置 launch.js

将启动配置添加到项目的 *launch.js*。 如果项目没有 *launch.js的* 文件，请通过以下方式添加一个：切换到 " **运行** " 选项卡，选择 " **在文件上创建 launch.js**"，然后在对话框中选择 " **.net** " 或 " **.net Core** "。

*launch.js上* 的新配置应如下所示：

```json
"configurations": [
    {
        "name": ".NET Core Launch (remote console)",
        "type": "coreclr",
        "request": "launch",
        "preLaunchTask": "build",
        "program": "/home/pi/.dotnet/dotnet",
        "args": ["/home/pi/sample/Sample.dll"],
        "cwd": "/home/pi/sample",
        "stopAtEntry": false,
        "console": "internalConsole",
        "pipeTransport": {
            "pipeCwd": "${workspaceFolder}",
            "pipeProgram": "C:\\Program Files\\PuTTY\\PLINK.EXE",
            "pipeArgs": [
                "-pw",
                "P@ssw0rd",
                "root@raspberrypi"
            ],
            "debuggerPath": "/home/pi/vsdbg/vsdbg"
        }
    },
```

请注意以下内容：

- `program` 是指向 Pi 上的 .NET 运行时的路径。
- `args` 是要在 Pi 上调试的程序集的路径。
- `cwd` 在 Pi 上启动应用时使用的工作目录。
- `pipeProgram` 是指向本地计算机上的 SSH 客户端的路径。
- `pipeArgs` 要传递到 SSH 客户端的参数。 请确保指定 password 参数，以及 `root` 该格式的用户 `<user>@<hostname>` 。

> [!IMPORTANT]
> 上面的示例使用 *plink*，它是 [PuTTY](https://www.ssh.com/ssh/putty/) <span class="docon docon-navigate-external x-hidden-focus"></span> SSH 客户端的一个组件。 [OpenSSH](https://www.openssh.com/)适用 <span class="docon docon-navigate-external x-hidden-focus"></span> 于最新版本的 Windows 和 Linux 的 OpenSSH 可以改为使用。 但是，OpenSSH 不支持以命令行参数的形式发送密码。 若要使用 OpenSSH，请 [为无密码 SSH 访问配置 Raspberry Pi](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md) <span class="docon docon-navigate-external x-hidden-focus"></span> 。

### <a name="deploy-the-app"></a>部署应用

部署应用，如 [将 .net 应用部署到 Raspberry Pi](deployment.md)中所述。 确保部署路径与在配置的launch.js中的参数中指定的路径相同 `cwd` 。 *launch.json*

### <a name="launch-the-debugger"></a>启动调试程序

在 " **运行** " 选项卡上，选择 " **.net Core 启动 (远程控制台)** 配置"，然后选择 " **启动调试**"。 此应用在 Raspberry Pi 上启动。 调试器可用于设置断点、检查局部变量等。

## <a name="references"></a>参考

[使用 ARM 上的 .Net Core 在 Windows 上使用 VS Code 远程调试到 Raspberry Pi](https://www.hanselman.com/blog/remote-debugging-with-vs-code-on-windows-to-a-raspberry-pi-using-net-core-on-arm)<span class="docon docon-navigate-external x-hidden-focus"></span>

::: zone-end

::: zone pivot="visualstudio"

## <a name="debug-from-visual-studio-on-windows"></a>在 Windows 上从 Visual Studio 进行调试

Visual Studio 可以通过 SSH 在远程设备上调试 .NET 应用。 设备上不需要专用配置。 若要详细了解如何使用 Visual Studio 远程调试 .NET，请参阅 [在 Linux 上使用 SSH 远程调试 .net](/visualstudio/debugger/remote-debugging-dotnet-core-linux-with-ssh?view=vs-2019)。

::: zone-end
