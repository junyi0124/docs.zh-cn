---
title: 如何：安装和卸载 Windows 服务
description: 了解如何安装和卸载 Windows 服务。 如果要使用 .NET 开发 Windows 服务，可以使用 InstallUtil.exe 或 PowerShell。
ms.date: 02/05/2019
helpviewer_keywords:
- Windows Service applications, deploying
- services, uninstalling
- services, installing
- installing Windows Services
- uninstalling applications, apps, Windows services
- installation, Windows services
- uninstalling Windows services
- installutil.exe tool
ms.assetid: c89c5169-f567-4305-9d62-db31a1de5481
ms.openlocfilehash: 6b7cfd8b241df4fe01c9c2a08888c88a1c749d13
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "91609676"
---
# <a name="how-to-install-and-uninstall-windows-services"></a>如何：安装和卸载 Windows 服务

如果正使用 .NET Framework 开发 Windows 服务，可以使用 [InstallUtil.exe](../tools/installutil-exe-installer-tool.md) 的命令行实用工具或 [PowerShell](/powershell/scripting/overview) 快速安装服务应用。 如果开发人员希望发布用户可安装和卸载的 Windows 服务，则可以使用免费的 [WiX 工具集](https://wixtoolset.org/)或[高级安装程序](https://www.advancedinstaller.com/)、[InstallShield](https://www.revenera.com/install/products/installshield.html) 等其他商业工具。 有关详细信息，请参阅[创建安装程序包（Windows 桌面）](/visualstudio/deployment/deploying-applications-services-and-components#create-an-installer-package-windows-desktop)。

> [!WARNING]
> 如果你想要从你的计算机卸载服务，不要遵循本文中的步骤。 而是找出安装了该服务的程序或软件包，然后在“设置”中选择“应用”来卸载该程序。 请注意，许多服务是 Windows 不可或缺的部分；如果你删除它们，可能导致系统不稳定。

要使用本文中的步骤，首先需要将服务安装程序添加到 Windows 服务。 有关详细信息，请参见[演练：创建 Windows 服务应用](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)。

无法通过按 F5 从 Visual Studio 开发环境直接运行 Windows 服务项目。 必须先在项目中安装服务，然后才能运行该项目。

> [!TIP]
> 可以使用“服务器资源管理器”验证是否已安装或卸载服务。 有关详细信息，请参阅[如何在 Visual Studio 中使用服务器资源管理器](https://support.microsoft.com/help/316649/how-to-use-the-server-explorer-in-visual-studio-net-and-visual-studio)。

### <a name="install-your-service-manually-using-installutilexe-utility"></a>使用 InstallUtil.exe 实用工具手动安装服务

1. 从“开始”菜单中选择“Visual Studio \<*version*>”目录，然后选择“VS \<*version*> 开发人员命令提示”  。

     出现“Visual Studio 开发人员命令提示”。

2. 访问你的项目的已编译可执行文件所在的目录。

3. 将项目的可执行文件作为参数，通过命令提示运行 InstallUtil.exe：

    ```console
    installutil <yourproject>.exe
    ```

     如果使用的是 Visual Studio 开发人员命令提示，InstallUtil.exe 应该在系统路径上。 如果不在，可以将其添加到该路径，或使用完全限定的路径来调用它。 此工具随 .NET Framework 安装在 *%WINDIR%\Microsoft.NET\Framework[64]\\<framework_version\>* 中。

     例如：
     - 对于 32 位版的 .NET Framework 4 或 4.5 以及更高版本，如果 Windows 安装目录是 C: \ Windows，则默认路径是 C:\Windows\ Microsoft.NET \ Framework \ v4.0.30319 \ InstallUtil.exe 。
     - 对于 64 位版的 .NET Framework 4 或 4.5 以及更高版本，默认路径是 C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe。

### <a name="uninstall-your-service-manually-using-installutilexe-utility"></a>使用 InstallUtil.exe 实用工具手动卸载服务

1. 从“开始”菜单中选择“Visual Studio \<*version*>”目录，然后选择“VS \<*version*> 开发人员命令提示”  。

     出现“Visual Studio 开发人员命令提示”。

2. 将项目的输出作为参数，通过命令提示运行 InstallUtil.exe：

    ```console
    installutil /u <yourproject>.exe
    ```

3. 删除服务的可执行文件后，该服务可能仍然会出现在注册表中。 如果发生这种情况下，请使用命令 [sc delete](/windows-server/administration/windows-commands/sc-delete) 从注册表中删除服务的条目。

### <a name="install-your-service-manually-using-powershell"></a>使用 PowerShell 手动安装服务

1. 从“开始”菜单中，选择“Windows PowerShell”目录，然后选择“Windows PowerShell”。

2. 访问你的项目的已编译可执行文件所在的目录。

3. 运行 [New-Service](/powershell/module/microsoft.powershell.management/new-service) cmdlet，并将项目的输出和服务名称作为参数：

    ```powershell
    New-Service -Name "YourServiceName" -BinaryPathName <yourproject>.exe
    ```

### <a name="uninstall-your-service-manually-using-powershell"></a>使用 PowerShell 手动卸载服务

1. 从“开始”菜单中，选择“Windows PowerShell”目录，然后选择“Windows PowerShell”。

2. 运行 [Remove-Service](/powershell/module/microsoft.powershell.management/remove-service) cmdlet，并将服务名称作为参数：

    ```powershell
    Remove-Service -Name "YourServiceName"
    ```

3. 删除服务的可执行文件后，该服务可能仍然会出现在注册表中。 如果发生这种情况下，请使用命令 [sc delete](/windows-server/administration/windows-commands/sc-delete) 从注册表中删除服务的条目。

    ```powershell
    sc.exe delete "YourServiceName"
    ```

## <a name="see-also"></a>请参阅

- [Windows 服务应用程序简介](introduction-to-windows-service-applications.md)
- [如何：创建 Windows 服务](how-to-create-windows-services.md)
- [如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)
- [Installutil.exe（安装程序工具）](../tools/installutil-exe-installer-tool.md)
