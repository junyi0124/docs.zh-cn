---
title: WCF 服务主机 (WcfSvcHost.exe)
description: 使用 WCF 服务主机承载和测试已实现的服务。 您可以使用 WCF 测试客户端或您自己的客户端来测试服务。
ms.date: 03/30/2017
ms.assetid: 8643a63d-a357-4c39-bd6c-cdfdf71e370e
ms.openlocfilehash: 2ac1d6318d8a82a82c08f38305ee6f92ad3f52a2
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90544591"
---
# <a name="wcf-service-host-wcfsvchostexe"></a>WCF 服务主机 (WcfSvcHost.exe)

Windows Communication Foundation (WCF) 服务主机 ( # A0) 允许启动 Visual Studio 调试器 (F5) 来自动承载和测试已实现的服务。 然后，你可以使用 WCF 测试客户端 ( # A0) 或你自己的客户端来测试服务，以查找并解决任何潜在错误。

## <a name="wcf-service-host"></a>WCF 服务主机

WCF 服务主机枚举 WCF 服务项目中的服务，加载项目的配置，并为它找到的每个服务实例化主机。 该工具通过 WCF 服务模板集成到 Visual Studio 中，并在你开始调试项目时进行调用。

通过使用 WCF 服务主机，你可以在 WCF 服务库项目中承载 WCF 服务 () 而无需在开发期间编写额外代码或提交到特定的主机。

> [!NOTE]
> WCF 服务主机不支持部分信任。 如果要在部分信任环境中使用 WCF 服务，请不要使用 Visual Studio 中的 WCF 服务库项目模板生成服务。 而是在 Visual Studio 中创建一个新网站，方法是选择 "WCF 服务网站" 模板，该模板可以在支持 WCF 部分信任的 Web 服务器中承载服务。

## <a name="project-types-hosted-by-wcf-service-host"></a>WCF 服务主机承载的项目类型

WCF 服务主机可以承载以下 WCF 服务库项目类型： WCF 服务库、顺序工作流服务库、状态机工作流服务库和联合服务库。 WCF 服务主机还可以托管这些可使用 " **添加项** " 功能添加到服务库项目中的服务。 这包括 WCF 服务、WF 状态机服务、WF 顺序服务、XAML WF 状态机服务和 XAML WF 顺序服务。

但是，应该注意到该工具将不会帮助您配置主机。 对于此项任务，必须手动编辑 App.config 文件。 另外，此工具也不会验证用户定义的配置文件。

> [!CAUTION]
> 不应在生产环境中使用 WCF 服务主机来托管服务，因为它不是为此目的而设计的。  WCF 服务主机不支持此类环境的可靠性、安全性和可管理性要求。 请改用 IIS，因为它可以提供卓越的可靠性和监视功能，并且是承载服务的首选解决方案。 完成服务开发后，应将服务从 WCF 服务主机迁移到 IIS。

## <a name="scenarios-for-using-wcf-service-host-inside-visual-studio"></a>在 Visual Studio 中使用 WCF 服务主机的方案

下表列出了 " **命令行参数** " 对话框中的所有参数，在 Visual Studio 的 " **解决方案资源管理器** " 中右键单击项目，选择 " **属性**"，然后选择 " **调试** " 选项卡，然后单击 " **启动项目**"，可以找到该对话框。 这些参数在配置 WCF 服务主机时非常有用。

|参数|含义|
|---------------|-------------|
|`/client`|一个可选参数，用于指定要在承载服务后运行的可执行文件的路径。 这会在托管后启动 WCF 测试客户端。|
|`/clientArg`|将字符串指定为传递给自定义客户端应用程序的参数。|
|`/?`|显示帮助文本。|

#### <a name="using-wcf-test-client"></a>使用 WCF 测试客户端

创建新的 WCF 服务项目并按 F5 启动调试器后，WCF 服务主机将开始承载它在项目中找到的所有服务。 WCF 测试客户端将自动打开并显示在配置文件中定义的服务终结点的列表。 可以从主窗口中测试参数并调用服务。

若要确保使用 WCF 测试客户端，请在 Visual Studio 的 **解决方案资源管理器** 中右键单击项目，选择 " **属性**"，然后选择 " **调试** " 选项卡。单击 " **启动项目** "，并确保下面出现在 " **命令行参数** " 对话框中。

`/client:WcfTestClient.exe`

#### <a name="using-a-custom-client"></a>使用自定义客户端

若要使用自定义客户端，请在 Visual Studio 的 **解决方案资源管理器** 中右键单击项目，选择 " **属性**"，然后选择 " **调试** " 选项卡。单击 " **启动项目** "，然后 `/client` 在 " **命令行参数** " 对话框中编辑参数以指向自定义客户端，如下面的示例中所示。

`/client:"path/CustomClient.exe"`

当您按 F5 再次启动服务时，WCF 服务主机将在您启动调试器时自动启动您的自定义客户端。

也可以按照以下示例中的指示使用 `/clientArg:` 形参将字符串指定为传递给自定义客户端应用程序的实参。

`/client:"path/CustomClient.exe" /clientArg:"arguments that are passed to Client"`

例如，如果使用的是联合服务库模板，则可以使用以下命令行自变量：

`/client:iexplore.exe /clientArgs:http://localhost:8731/Design_Time_Addresses/Feed1/`

#### <a name="specifying-no-client"></a>指定无客户端

若要指定在 WCF 服务托管后不使用任何客户端，请在 Visual Studio 的 **解决方案资源管理器** 中右键单击项目，选择 " **属性**"，然后选择 " **调试** " 选项卡。单击 " **启动项目** "，并将 " **命令行参数** " 对话框留空。

#### <a name="using-a-custom-host"></a>使用自定义主机

若要使用自定义主机，请在 Visual Studio 的 **解决方案资源管理器** 中右键单击项目，选择 " **属性**"，然后选择 " **调试** " 选项卡。单击 " **启动外部程序** "，并输入自定义主机的完整路径。 还可以使用 " **命令行参数** " 对话框指定要传递给主机的参数。

## <a name="wcf-service-host-user-interface"></a>WCF 服务主机用户界面

最初在 Visual Studio) 中按 F5 调用 WCF 服务主机 (时，" **Wcf 服务主机** " 窗口将自动打开。 当 WCF 服务主机正在运行时，该程序的图标将出现在通知区域中。 双击该图标以打开 " **WCF 服务主机** " 窗口

当服务承载期间发生错误时，"WCF 服务主机" 对话框将打开以显示相关信息。

" **WCF 服务主机** " 主窗口包含以下两个菜单：

- **File**：包含 **Close** 和 **Exit** 命令。 单击 " **关闭**" 时，" **WCF 服务主机** " 对话框将关闭，但会继续托管服务。 单击 " **退出**" 时，WCF 服务主机也会关闭。 这还将停止所有承载的服务。

- **Help**：包含包含版本信息的 **About** 命令。 它还包含可打开帮助文件的 **帮助** 命令。

" **WCF 服务主机** " 主窗口包含以下两个区域：

- 第一个区域为 " **服务**"。 该区域包含一个显示所有服务基本信息的列表。 这些信息包括：

  - **服务**：列出所有服务。

  - **状态**：列出服务的状态。 有效值为 "已启动"、"已停止" 和 "错误"。

  - **元数据地址**：显示服务的元数据地址。

- 第二个区域是 **附加信息**。 当在 **服务** 区域中选择特定的服务行时，它会显示服务状态的详细说明。 如果状态为“错误”，则可以在屏幕上查看完整的错误消息。

## <a name="stopping-wcf-service-host"></a>停止 WCF 服务主机

可以通过以下四种方法关闭 WCF 服务主机：

- 在 Visual Studio 中停止调试会话。

- 从 " **WCF 服务主机**" 窗口中的 "**文件**" 菜单中选择 "**退出**"。

- 从 "系统通知" 区域中的 "WCF 服务主机任务栏" 图标的上下文菜单中选择 " **退出** "。

- 如果正在使用 WCF 测试客户端，则退出它。

## <a name="using-service-host-without-administrator-privilege"></a>在无管理员权限的情况下使用服务主机

为了使无管理员权限的用户能够开发 WCF 服务，在 http://+:8731/Design_Time_Addresses 安装 Visual Studio 的过程中，将为命名空间 "" 创建一个 ACL (访问控制列表) 。 该 ACL 被设置为“(UI)”，这将包括登录到此计算机的所有交互用户。 管理员可以在此 ACL 中添加或删除用户，或者打开其他端口。此 ACL 使用户可以使用 WCF 服务自动主机 ( # A0) ，而无需授予其管理员权限。

你可以使用提升的管理员帐户在 Windows Vista 中使用 netsh.exe 工具来修改访问权限。 下面是使用 netsh.exe 的示例。

```console
netsh http add urlacl url=http://+:8001/MyService user=<domain>\<user>
```

有关 netsh.exe 的详细信息，请参阅 "[如何使用 Netsh.exe 工具和命令行开关](/previous-versions/tn-archive/bb490939(v=technet.10))"。

## <a name="see-also"></a>请参阅

- [WCF 测试客户端 (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)
