---
title: Windows 10 迁移
description: Windows 10 功能（如打包和 XAML 孤岛）深入探讨。
ms.date: 09/16/2019
ms.openlocfilehash: cd17088b086a32fd3bb37e617d3a1047acedde0e
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "97866416"
---
# <a name="windows-10-migration"></a>Windows 10 迁移

请考虑以下情况：你有在 Windows 7 天内开发的工作桌面应用程序。 它目前使用 WPF 技术，在 Windows 10 上运行时，它具有过时的 UI 和行为。 就像观看现在俨然电影（如 Matrix）时，可以使用 Nokia 8110 设备查看 Neo。 此电影在20年后工作良好，但它可以从设备现代化中获益。

随着 Windows 10 的发布，Microsoft 引入了许多创新来支持诸如平板电脑和触摸设备之类的方案，并为 Microsoft 操作系统的用户提供最佳体验。 例如，你能够：

- 使用 Windows Hello 通过你的面部登录。
- 使用笔绘制或手写自动识别和 digitalized 的文本。
- 使用 WinML 运行在云上构建的本地自定义 AI 模型。

通过 Windows 运行时 (WinRT) 库为 Windows 开发人员启用所有这些功能。 可以在现有的桌面应用程序中利用这些功能，因为库也同时公开给 .NET Framework 和 .NET Core。 你甚至可以使用 XAML 孤岛来现代化 UI，并根据时间改进应用的视觉对象和行为。

此处需要注意的一个重要事项是，无需放弃 .NET Framework 技术来遵循此现代化路径。 您可以安全地停留在那里，并拥有 Windows 10 的所有优点，而无需迁移到 .NET Core。 因此，您可以获得强大的功能和灵活性来选择您的现代化路径。

## <a name="winrt-apis"></a>WinRT Api

WinRT Api 是面向对象的、结构良好的应用程序编程接口 (Api) 为 Windows 10 开发人员提供对操作系统必须提供的所有内容的访问权限。 通过 WinRT Api，你可以将功能（如推送通知、设备 Api、Microsoft Ink 和 WinML）与桌面应用中的其他功能集成。

通常，可以从经典桌面应用程序调用 WinRT Api。 但是，有两个主要方面是此规则的例外情况：

* 需要包标识的 Api。
* 需要可视化的 Api，如 XAML 或组合。

### <a name="universal-windows-platform-uwp-packages"></a>通用 Windows 平台 (UWP) 包

#### <a name="application-package-identity"></a>应用程序包标识

UWP 应用有一个部署系统，其中操作系统管理应用程序的安装和卸载。 这要求安装是声明性的，这意味着在安装期间不执行任何用户代码。 相反，应用程序要与系统集成的所有内容（如协议、文件类型和扩展）都是在应用程序清单中声明的。 部署管道在部署时配置这些集成点。 操作系统管理所有此功能并对其进行跟踪的唯一方式是，每个 "包" 具有标识，这是应用程序的唯一标识符。

某些 WinRT Api 要求此包标识按预期方式工作。 但是，经典桌面应用（如本机 c + + 或 .NET 应用）使用不需要包标识的不同部署系统。 如果要在桌面应用程序中使用这些 WinRT Api，则需要为其提供包标识。

继续操作的一种方法是构建附加的打包项目。 在打包项目中，指向原始源代码项目，并指定要提供的标识信息。如果安装包并运行已安装的应用程序，它会自动获取标识，使代码可以调用需要标识的所有 WinRT Api。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">
    <Identity Name="YOUR-APP-GUID "
              Publisher="CN=YOUR COMPANY"
              Version="1.x.x.x" />
</Package>
```

通过检查包含 API 的类型是否使用 [DualApiPartition](xref:Windows.Foundation.Metadata.DualApiPartitionAttribute) 属性进行标记，可以检查哪些 api 需要打包的应用程序标识。如果是，则可以从未打包的传统桌面应用程序调用。 否则，必须使用打包项目的帮助将经典桌面应用转换为 UWP。

<https://docs.microsoft.com/windows/desktop/apiindex/uwp-apis-callable-from-a-classic-desktop-app>

#### <a name="benefits-of-packaging"></a>打包的优点

除了允许您访问这些 Api 之外，您还可以通过为桌面应用程序创建 Windows 应用包来获得一些额外的好处，包括：

* **简化的部署**。 应用具有良好的部署体验，确保用户可以放心地安装应用程序并对其进行更新。 如果用户选择卸载应用程序，则会完全删除该应用程序，而不留下任何跟踪来阻止 Windows rot 问题。

* **自动更新和许可**。 你的应用程序可以参与 Microsoft Store 的内置许可和自动更新功能。 自动更新是高度可靠和高效的机制，因为仅下载文件的已更改部分。

* **扩大了覆盖范围，并简化了盈利**。 您可能不是您的情况，但如果您选择通过 Microsoft Store 将您的应用程序分发给数百万个 Windows 10 用户。

* **添加 UWP 功能**。 您可以按自己的节奏将 UWP 功能添加到应用程序包。

#### <a name="prepare-for-packaging"></a>准备打包

在开始打包桌面应用程序之前，必须先解决一些要点，然后才能开始此过程。 应用程序必须遵守 Microsoft Store 的规则和策略，并在 UWP 应用程序模型中运行。 例如，必须在 .NET Framework 4.6.2 或更高版本上运行，并写入 `HKEY_CURRENT_USER` 注册表配置单元，并将 AppData 文件夹虚拟化到特定于用户的本地位置。

打包的设计目标是将应用程序状态与系统状态分开，同时保持与其他应用的兼容性。 Windows 10 通过将应用程序置于 UWP 包内来实现此目标。 它在运行时检测并重定向对文件系统和注册表的一些更改，以满足打包提供的应用程序的受信任的干净安装和卸载行为。

你为桌面应用程序创建的包是仅限桌面的完全信任应用程序，它们不会进行沙盒处理，不过，在应用中应用了轻型虚拟化以写入到 `HKCU` 和 `AppData` 。 此虚拟化可让他们与经典桌面应用程序相同的方式与其他应用交互。

##### <a name="installation"></a>安装

应用包安装在 *% ProgramFiles% \\ WindowsApps \\ package_name* 下，其中包含名为的可执行文件  `app_name.exe` 。 每个包文件夹都包含一个名为 `AppxManifest.xml`) 的清单 (，其中包含打包应用程序的特殊 XML 命名空间。 在该清单文件中，是一个  `<EntryPoint>`   元素，它引用完全信任的应用程序。 当该应用程序启动时，它不会在应用程序容器内运行，而是以用户通常的方式运行。

部署后，软件包文件由操作系统标记为只读并严格锁定。 如果这些文件遭到篡改，Windows 将阻止应用启动。

##### <a name="file-system"></a>文件系统

对于打包的桌面应用程序，操作系统支持不同级别的文件系统操作，具体取决于文件夹位置。

当尝试访问用户的 *AppData* 文件夹时，系统将创建一个专用的每用户、每个应用在幕后的位置。 这将创建打包后的应用程序在实际修改专用副本时编辑真正 *AppData* 的假象。 通过以这种方式重定向写入，系统可以跟踪应用所作的所有文件修改。 然后，它可以在卸载减少系统 "rot" 时清除所有这些文件，并为用户提供更好的应用程序删除体验。

##### <a name="registry"></a>注册表

应用包包含注册表 .dat 文件，该文件  `HKLM\Software`   在实际注册表中充当的逻辑等效项。 在运行时，此虚拟注册表将此配置单元的内容合并到原生系统配置单元，以提供两者的单一视图。

所有写入都将保留在包升级过程中，仅在卸载应用程序时删除。

##### <a name="uninstallation"></a>卸载

当用户卸载包时，将删除位于下的所有文件和文件夹  `C:\Program Files\WindowsApps\package_name` ，以及对 AppData 或在该过程中捕获的注册表进行的任何重定向写入操作。

有关打包应用程序如何处理安装、文件访问、注册表和卸载的详细信息，请参阅 <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-behind-the-scenes> 。

您可以获取要检查的项目的完整列表 <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-prepare> 。

## <a name="how-to-add-winrt-apis-to-your-desktop-project"></a>如何将 WinRT Api 添加到桌面项目

在本部分中，可以找到有关如何在现有 WPF 应用程序中集成 Toast 通知的演练。 尽管代码的观点很简单，但它有助于阐释整个过程。 通知是可在 .NET 应用中使用的众多可用 WinRT Api 之一。 在这种情况下，API 需要包标识。 如果 Api 不需要包标识，此过程更加简单。

让我们使用一个现有的 WPF 示例应用程序，它读取文件并在屏幕上显示其内容。 目标是在应用程序启动时显示 Toast 通知。

![运行的示例应用程序的屏幕截图](./media/windows-migration/sample-application.png)

首先，应签入以下链接，以确定要使用的 Windows 10 API 是否需要包标识：

<https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-supported-api>

示例将使用 <xref:Windows.UI.Notifications.Notification?displayProperty=nameWithType> 需要打包标识的 API：

![Microsoft 文档中的通知类](./media/windows-migration/notification-class-documentation.png)

若要访问 WinRT API，请添加对 NuGet 包的引用， `Microsoft.Windows.SDK.Contracts`   此包将在幕后执行神奇 (查看) 的详细信息 <https://blogs.windows.com/windowsdeveloper/2019/04/30/calling-windows-10-apis-from-a-desktop-application-just-got-easier/> 。

你现在已准备好开始添加一些代码。

创建 `ShowToastNotification` 将在应用程序启动时调用的方法。 它只是从 XML 模式生成 toast 通知：

```csharp
private void ShowNotification(string title, string content, string image)
{
    string xmlString = $@"<toast><visual><binding template = 'ToastGeneric'><text>{title}</text><text>{content}</text><image src=>'{image}'</image></binding></visual></toast>";
    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);
    ToastNotification toast = new ToastNotification(toastXml);
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

尽管项目会生成，但会出现错误，因为通知 API 需要包标识，而你未提供。 将 Windows 打包项目添加到解决方案可以解决此问题：

![Visual Studio 中的 "添加新项目" 对话框的屏幕截图](./media/windows-migration/add-packaging-project.png)

选择要支持的最低 Windows 版本和目标版本。 并非所有的 WinRT Api 在所有 Windows 10 版本中都受支持。 每个 Windows 10 更新会添加仅在此版本中可用的新 Api;下层支持不可用。

![选择最低 Windows 版本](./media/windows-migration/select-versions.png)

下一步是通过添加项目引用将 WPF 应用程序添加到 Windows 打包项目：

![将 WPF 应用程序添加到 Windows 打包项目](./media/windows-migration/add-application.png)

![引用管理器](./media/windows-migration/reference-manager.png)

Windows 打包项目可以打包多个应用，因此应将其设置为入口点：

![设置入口点](./media/windows-migration/set-entry-point.png)

下一步是将 WPF 项目设置为解决方案配置中的启动项目。 可以按 F5 编译并生成并查看结果。

![运行和显示结果的示例应用程序](./media/windows-migration/sample-app-result.png)

让我们生成包，以便可以安装应用程序。 右键单击 "**存储**  >  **创建应用包**"。

!["创建应用包" 对话框](./media/windows-migration/create-app-packages.png)

选择 "旁加载" 选项以从计算机部署应用：

![选择旁加载选项](./media/windows-migration/select-sideloading-option.png)

选择应用的应用程序体系结构：

![选择应用程序体系结构](./media/windows-migration/select-app-architecture.png)

最后，单击 " **创建**" 创建包。

## <a name="xaml-islands"></a>XAML 岛

XAML 孤岛是一组组件，使 Windows 桌面开发人员能够在其现有 Win32 应用程序（包括 Windows 窗体和 WPF）上使用 UWP XAML 控件。

![XAML 孤岛的结构](./media/windows-migration/xaml-islands.png)

你可以使用标准控件以及从现代世界中包含控件的 UWP UI 的 "孤岛" 来图像 Win32 应用。 此概念类似于在网页中具有显示来自 `different page.`

除了从 Windows 10 Api 添加功能外，还可以使用 XAML 孤岛在应用内添加 UWP XAML 片段。

Windows 10 1903 更新引入了一组 Api，可用于在 Win32 窗口中承载 UWP XAML 内容。 只有在 Windows 10 1903 上运行的应用才能使用 XAML 孤岛。

### <a name="the-road-to-xaml-islands"></a>XAML 孤岛的道路

当 Microsoft 引入了 WinRT Api 作为框架，以便在)  (Windows 窗体、WPF 和本机 Win32 应用程序中实现 Win32 应用的现代化时，从2012开始开始 XAML 孤岛。 但是，WinRT 中的新 UI 控件可用于新应用程序，但不能用于现有应用程序。

在2015和 Windows 10 中，UWP 是出生的。 UWP 允许您创建可跨 Windows 设备（如 XBox、移动和桌面）运行的应用程序。 一年后，Microsoft 宣布桌面桥 (以前称为 Project Centennial) 。 桌面桥是一组工具，使开发人员能够将现有的 Win32 应用程序引入 Microsoft Store。 2017中添加了更多功能，使开发人员能够利用一些新的 Windows 10 Api （例如操作中心的活动磁贴和通知）增强其 Win32 应用程序。 但仍不存在新的 UI 控件。

在版本2018中，Microsoft 宣布了一种方法，使开发人员能够将新的 Windows 10 XAML 控件使用到当前的 Win32 应用程序中，而无需将其应用完全迁移到 UWP。 它被标记为 UWP XAML 孤岛。

### <a name="how-it-works"></a>工作原理

Windows 10 1903 更新引入了几个 XAML 宿主 Api。 其中两个是 `WindowsXamlManager`   和  `DesktopWindowXamlSource` 。

 `WindowsXamlManager`   类处理 UWP XAML 框架。 它的 `InitializeForCurrentThread` 方法将 UWP XAML 框架加载到 Win32 应用程序的当前线程中。

 `DesktopWindowXamlSource`   是 XAML 岛内容的实例。 它具有 `Content` 用于实例化和设置的属性。 `DesktopWindowXamlSource`   呈现并从 HWND 获取其输入。 它需要知道将 XAML 岛附加到哪些其他 HWND，并负责调整和定位父级的 HWND。

WPF 或 Windows 窗体开发人员通常不会处理其代码中的 HWND，因此，很难理解并处理 HWND 指针和基础布线材料来与 Win32 和 UWP 的世界通信。

#### <a name="the-xaml-islands-net-wrappers"></a>XAML 孤岛 .NET 包装

Windows 社区工具包为 WPF 或 Windows 窗体设置了 XAML 孤岛 .NET 包装，使其更易于使用。 这些包装管理 Hwnd、焦点管理和其他功能。 Windows 窗体和 WPF 开发人员应使用这些包装。

Windows 社区工具包提供了两种类型的控件：包装控件和宿主控件。

##### <a name="wrapped-controls"></a>包装控件

这些包装控件将某些 UWP 控件包装到 Windows 窗体或 WPF 控件中，为这些开发人员隐藏 UWP 概念。 这些控件包括：

* Web 视图和 WebViewCompatible
* InkCanvas 和 InkToolbar
* MediaPlayerElement
* MapControl

将 `Microsoft.Toolkit.Wpf.UI.Controls` 包添加到项目中，包括对命名空间的引用，并开始使用。

```xml
<Window
        ...
        xmlns:uwpControls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls">
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="\*"/>
    </Grid.RowDefinitions>
    <uwpControls:InkToolbar TargetInkCanvas="{x:Reference Name=inkCanvas}"/>
    <uwpControls:InkCanvas Grid.Row="1" x:Name="inkCanvas" />
</Grid>
```

##### <a name="hosting-controls"></a>宿主控件

XAML 孤岛的强大功能延伸到了多数第一方控件、第三方控件以及为 UWP 开发的自定义控件，这些控件可作为 "孤岛" 集成到 Windows 窗体和 WPF，并具有完整功能的 UI。 `WindowsXamlHost`WPF 和 Windows 窗体的控件允许执行此操作。

例如，若要 `WindowsXamlHost` 在 WPF 中使用控件，请添加对 `Microsoft.Toolkit.Wpf.UI.XamlHost` Windows 社区工具包提供的包的引用。

将放 `WindowsXamlHost` 入 UI 代码后，指定要加载的 UWP 类型。 您可以选择使用被包装的控件，如 `Button` 或由多个不同控件（自定义 UWP 控件）组成的更复杂的控件。

下面的示例演示如何添加 UWP `Button` ：

```xml
<Window
        ...
        xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost">

<xamlhost:WindowsXamlHost x:Name="myUwpButton"
                          InitialTypeName="Windows.UI.Xaml.Controls.Button" />
```

对于如何实现此操作，有一个清晰的建议，更好的是，有一个包含自定义复合控件的单个和更大的 XAML 岛，而不是每个都有一个控件。

XAML 的核心功能之一是绑定，它在 Win32 代码和岛之间有效。 因此，您可以绑定 `Textbox` 具有 UWP 的 Win32 `Textbox` 。 需要考虑的一个重要事项是，这些绑定是从 UWP 到 Win32 的单向绑定，因此，如果在 `Textbox` XAML 岛内部更新，Win32 文本框将会更新，但不会被更新。

若要查看有关如何使用 XAML 孤岛的演练，请参阅：

<https://docs.microsoft.com/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands>

#### <a name="adding-uwp-xaml-custom-controls"></a>添加 UWP XAML 自定义控件

XAML 自定义控件是由你或第三方) 创建的控件 (或用户控件， (包括 WinUI 1.x 控件) 。 若要在 Windows 窗体或 WPF 应用程序中托管自定义 UWP 控件，你将需要：

- `WindowsXamlHost`在 .Net Core 2.x 应用中使用 UWP 控件。
- 若要创建定义对象的 UWP 应用项目，则为 `XamlApplication` 。

WPF 或 Windows 窗体项目必须有权访问 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` Windows 社区工具包提供的类的实例。 此对象用作根元数据提供程序，用于为应用程序的当前目录中的程序集中的自定义 UWP XAML 类型加载元数据。 执行此操作的建议方法是将 (通用 Windows) 项目的空白应用添加到与 WPF 或 Windows 窗体项目相同的解决方案，然后修改此项目中的默认应用类。

自定义 UWP XAML 控件可包含在此 UWP 应用或独立 UWP 类库项目中，您可以在与 WPF 或 Windows 窗体项目相同的解决方案中引用该项目。

可以在以下位置查看详细的分步过程说明：

<https://docs.microsoft.com/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands>

#### <a name="the-windows-ui-library-winui-2"></a>Windows UI 库 (WinUI 2) 

除了操作系统附带的收件箱 Windows 10 控件外，同一 UWP XAML 团队还在 Windows UI 库中提供了其他控件， (**WinUI 2**) 。 WinUI 2 为 Windows UWP 应用提供官方的本机 Microsoft UI 控件和功能，这些控件可以在 XAML 孤岛内使用。

WinUI 2 是开源的，你可以在中找到信息 <https://github.com/microsoft/microsoft-ui-xaml> 。

以下文章演示了如何从 WinUI 2 库承载 UWP XAML 控件： <https://docs.microsoft.com/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands>

### <a name="do-you-need-xaml-islands"></a>是否需要 XAML 孤岛

XAML 孤岛适用于那些希望通过利用新的 UWP 控件和行为而无需完全重写应用程序来改善用户体验的现有 Win32 应用程序。 你可以使用 [Windows 10 api](/windows/uwp/porting/desktop-to-uwp-enhance)，但在 XAML 孤岛之前，只需要使用非 UI 相关的 api。

如果要开发新的 Windows 应用程序， [UWP 应用程序](/windows/uwp/get-started/universal-application-platform-guide)   可能是正确的方法。

### <a name="the-road-ahead-xaml-islands-winui-30"></a>更快的 XAML 孤岛： WinUI 3。0

自 Windows 8 起，Windows UI 平台（包括 XAML UI 框架、视觉组合层和输入处理）已作为 Windows 的一个有机组成部分提供。 这意味着，若要从对 UI 技术的最新改进中受益，你必须升级到最新版本的 UI，并在开发应用时降低创新速度。 为了使这两个发展周期和促进创新，Microsoft 正在积极处理 WinUI 项目。

从2018中的 WinUI 2 开始，Microsoft 已开始将一些新的 XAML UI 控件和功能作为构建于 UWP SDK 顶层的单独 NuGet 包交付。

![WinUI 2.0 的结构](./media/windows-migration/winui2.png)

WinUI 3 处于积极开发阶段，将极大地扩展 WinUI 的作用域，以包括完全与 UWP SDK 完全分离的完整 UI 平台：

![WinUI 3.0 的结构](./media/windows-migration/winui3.png)

XAML 框架现在将在 GitHub 上开发，并以 [NuGet](/nuget/what-is-nuget)包的形式提供   。

作为 OS 的一部分发布的现有 UWP XAML API 将不会再收到新的功能更新。 它们仍将根据 Windows 10 支持生命周期收到安全更新和关键修复程序。

通用 Windows 平台包含的不仅仅是 XAML 框架 (例如，应用程序和安全模型、媒体管道、Xbox 和 Windows 10 shell 集成、广泛的设备支持) 并将继续发展。 所有新的 XAML 功能都将作为 WinUI 的一部分进行开发和交付。

#### <a name="winui-3-in-desktop-app-and-winui-xaml-islands"></a>桌面应用中的 WinUI 3 和 WinUI XAML 孤岛

正如您所看到的，WinUI 3 是 UWP XAML 的演变，它在 UWP 应用模型中自然运行，并且其所有要求都 (.MSIX 打包 ID、沙箱、CoreWindow 等。 若要仅在 Win32 应用模型中使用 WinUI 3，WinUI 内容应由另一个 UI 框架承载 (Windows 窗体、WPF 等) 使用 **WINUI XAML 孤岛**。 如果要改进应用和混合技术，这是正确的路径。 但是，如果要替换 WinUI 的整个旧 UI，应用程序不应加载 UI 框架，只需托管 WinUI 即可。

WinUI 3 将解决 **在桌面应用中添加 WinUI 的** 关键反馈。 这将允许 Win32 应用程序使用 WinUI 3 作为独立 UI 框架;无需加载 Windows 窗体或 WPF。

在此聚合内，WinUI 3 将让开发人员轻松地混合和匹配的正确组合：

* 应用模型： UWP，Win32
* 平台： .NET Core 或本机
* 语言： .NET (C \# ，Visual Basic) ，标准 c + +
* 打包： .MSIX，AppX 用于 Microsoft Store，未打包
* 互操作：使用 WinUI 3，使用 WinUI XAML 孤岛扩展现有 WPF、WinForms 和 MFC 应用。

如果希望了解更多详细信息，Microsoft 将在中共享此路线图 <https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md> 。

>[!div class="step-by-step"]
>[上一页](migrate-modern-applications.md)
>[下一页](example-migration-core.md)
