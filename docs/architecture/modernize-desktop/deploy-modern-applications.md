---
title: 部署新式桌面应用程序
description: 部署新式桌面应用程序时需要了解的所有内容。
ms.date: 05/12/2020
ms.openlocfilehash: 4a4d93caf1c2f8f58d48ee3199bbe6983fa939f4
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "97866422"
---
# <a name="deploying-modern-desktop-applications"></a>部署新式桌面应用程序

开发桌面应用程序时，要考虑的一个问题是如何将应用程序打包并部署到用户的计算机。 打包、部署和安装的问题在于，它通常由 IT 专业人员负责，他们关心的不同方面与开发人员有关。

如今，我们都熟悉了 DevOps 概念，开发人员和 IT 专业人员在其中密切合作，将应用程序移动到其生产环境中。 但是，如果你已在桌面工作中经历了10年以上，你可能会看到以下情景。 开发人员团队共同工作，以满足项目截止时间。 业务人员紧张，因为他们需要系统在多个用户的计算机上工作才能运行公司。 在 "D 天" 上，项目经理会对照每个开发人员进行检查，使其代码正常工作，并确保一切正常，使他们能够发货。 然后，包团队将生成应用程序的安装程序，将其分发给每个用户计算机和一组测试用户运行该应用程序。 嗯，它们会尝试，因为在显示任何 UI 之前，应用程序会引发一个异常，其中显示 "方法 ~ 对象 ~ 失败"。 紧急情况从空中开始流动，并进行短暂的调查，重点介绍引入了第三方控件的一个年轻人开发人员，这无疑是 "在开发计算机上工作"。

出于两个主要原因，安装桌面应用程序通常是一种痛苦：

- 开发团队和 IT 团队之间缺少密切的协作文化。
- 缺乏可靠的打包和部署技术，我们可以进行构建。

事实上，我们一直在生活，这是因为有时你后悔安装了应用，因为：

- 它最终会在您的计算机上产生一些意外的副作用。
- 以前安装的某些应用程序将停止工作。

此外，不能通过卸载应用程序将系统还原到其原始状态。 我们在这种情况下，我们已经梦寐以求了 "DLL Hell" 或 "Winrot" 等术语。

在本章中，我们将讨论 .MSIX。 .MSIX 是 Microsoft 的全新技术，它尝试充分利用以前的技术，为未来的打包技术提供坚实的基础。

打包技术与现代化有什么关系？ 事实证明，打包是企业 IT 的基本知识，其中投入了大量资金。 现代化并非仅与使用最新技术相关。 在公司将功能交付给客户端之前，它还与缩短业务需求的上市时间有关。

## <a name="the-modern-application-lifecycle"></a>新式应用程序生命周期

如今，开发人员编写并生成应用程序的代码，然后将生成的资产传递给 IT 专业人员。 然后，IT 专业人员重新配置应用程序并将其重新打包，通常是在 MSI 中，或在最近的版本中，采用 app-v 打包格式。 然后，应用程序通过不同的通道和工具进行部署。 此方法的主要问题之一通常称为 "打包 paralysis"。 问题在于，每次更新应用更新或操作系统时，此周期都将重复。

您可以看到如下图所示的过程：

![显示新式 IT 生命周期的示意图](./media/deploy-modern-applications/modern-it-application-lifecycle.png)

公司需要通过一种方式将此打包周期分解为三个独立的周期：

- OS 更新
- 应用程序更新
- 自定义

![显示新式 IT 良性周期的图示](./media/deploy-modern-applications/modern-it-virtuous-cycle.png)

前面的关系图显示你可以：

- 更新基础 OS，无需重新打包应用。
- 启用自定义，无需重新打包原始开发人员包。

这项根式变化会使我们成为新的和新式 IT 生命周期，如下图所示：

![使用 Microsoft 工具显示应用程序生命周期的图示](./media/deploy-modern-applications/microsoft-it-tools.png)

开发人员创建应用程序并生成 .MSIX 包，IT 专业人员可以使用和配置该包，而无需重新打包。 除了 .MSIX 技术，Microsoft 还创建了一些工具，可用于自定义和配置包，而无需重新打包。

## <a name="msix-the-next-generation-of-deployment"></a>.MSIX：下一代部署

在 .MSIX 之前，可以使用多种打包技术，如安装向导、MSI、ClickOnce、App-v 和脚本撰写。 其中每种技术都有其各自的优势，Microsoft 决定最大程度地选择生成 .MSIX。 .MSIX 是在这些现有技术的基础上建立的，它们都是最佳选择：

- App-v = \> 容器化
- ClickOnce = \> 自动更新
- MSI = \> 易于分发

借助 .MSIX，你将获得一项安装程序技术，其中包含所有这些功能。

![显示对生成 .MSIX 有影响的各种技术的图示](./media/deploy-modern-applications/msix.png)

### <a name="benefits-of-msix"></a>.MSIX 的优点

#### <a name="never-regret-installing-an-app"></a>从不后悔安装应用

.MSIX 提供可预测、可靠和安全的部署。 包清单中包含的声明性方法使 OS 可以跟踪应用程序所需的每个资产。 它还提供真正的干净卸载，不会产生负面影响。

#### <a name="disk-space-optimization"></a>磁盘空间优化

.MSIX 经过优化，可降低应用程序对用户计算机磁盘空间的占用空间。 它创建文件的单实例存储。 也就是说，如果有两个不同于同一个 DLL 的包，则不会两次安装 DLL。 平台会处理该问题，因为它知道某个特定应用程序的所有文件都是根据其声明性性质而安装的。 它还允许并行工作的不同版本的 DLL。

使用资源包时，可以轻松创建多语言应用，操作系统会安装所使用的应用。

#### <a name="network-optimization"></a>网络优化

.MSIX 检测到字节块级别上的文件之间的差异，启用称为差异更新的功能。 这意味着在应用程序更新上只下载更新的字节块。

![显示 .MSIX 如何管理差异更新的关系图](./media/deploy-modern-applications/msix-managing-updates.png)

使用流式安装，用户可以快速开始使用应用程序，同时在后台下载应用程序的其他部分。 此功能可为用户提供丰富的体验。

利用可选包功能，你可以在应用部署上实现组件化，因此你可以在需要时下载这些包。

#### <a name="simple-packaging-and-deployment"></a>简单打包和部署

AppManifest 声明了版本控制、设备目标，并为每个应用程序确定了标准方式。 它还提供了一种方法来对资产进行签名，从而提供可靠的安全基础。

#### <a name="os-managed"></a>操作系统管理

操作系统将处理用于安装、更新和删除应用程序的所有过程。 应用程序是按用户安装的，但仅下载一次，从而最大程度地减少磁盘占用。 Microsoft 正在努力同时提供 Windows 7 上的 .MSIX 体验。

#### <a name="windows-provides-integrity-for-the-app"></a>Windows 提供应用的完整性

使用数字签名，可以确保不会从不受信任的源安装应用程序。 .MSIX 还会阻止篡改，原因如下：

- 它保留文件哈希记录。
- 它检测是否在安装后修改了文件。

#### <a name="works-for-the-entire-app-catalog"></a>适用于整个应用程序目录

.MSIX 的最酷之一是，它适用于整个应用程序目录、Windows 窗体、WPF、MFC/ATL、Delphi，即使你想要执行 xCopy 部署、ClickOnce 或转到应用商店，你也可以使用相同的 .MSIX 包。

### <a name="tools"></a>工具

#### <a name="windows-application-packaging-project"></a>Windows 应用程序打包项目

您可以使用 Visual Studio 中的 " **Windows 应用程序打包项目**"   项目为桌面应用程序生成包。 然后，你可以将该包发布到 Microsoft Store 或将其旁加载到一个或多个电脑上。

#### <a name="msix-packaging-tool"></a>MSIX 打包工具

使用 MSIX 打包工具可将现有的 Win32 应用程序重新打包为 MSIX 格式。 它同时提供交互式 UI 和用于转换的命令行，并使你能够在不使用源代码的情况下转换应用程序。

![MSIX 打包工具](./media/deploy-modern-applications/msix-packaging-tool.png)

#### <a name="package-support-framework"></a>包支持框架

包支持框架是一个开源工具包，可帮助你在无法访问源代码的情况下将修补程序应用于现有的 Win32 应用程序，使其能够在 .MSIX 容器中运行。 包支持框架可帮助应用程序遵循新式运行时环境的最佳做法。

#### <a name="app-installer"></a>应用安装程序

应用安装程序允许通过双击应用程序包来安装 Windows 10 应用。 这意味着用户无需使用 PowerShell 或其他开发人员工具来部署 Windows 10 应用。 应用安装程序还可以从 Web、可选包和相关的集中安装应用。

## <a name="how-to-create-an-msix-package-from-an-existing-win32-desktop-application"></a>如何从现有 Win32 桌面应用程序创建 .MSIX 包

让我们完成从现有 Win32 应用程序创建 .MSIX 包的过程。 在此示例中，我们将使用 Windows 窗体应用程序。

若要开始，请将新项目添加到解决方案中，选择 Windows 应用程序打包项目，并为其指定名称。

![添加新的 Windows 应用程序打包项目](./media/deploy-modern-applications/add-packaging-project.png)

你将看到打包项目的结构，并记下名为 " *应用程序*" 的特殊文件夹。 在此文件夹中，可以指定要包含在包中的应用程序。 它可以有多个。

![打包项目的结构](./media/deploy-modern-applications/packaging-project.png)

右键单击 " *应用程序* " 文件夹，然后选择要从 Visual Studio 解决方案中打包的 Windows 窗体项目。

![向打包项目添加 Windows 窗体项目](./media/deploy-modern-applications/add-winforms-project.png)

此时，可以编译和生成包，但我们要检查几个问题。 为了获得更好的用户体验，Visual Studio 可以自动生成所有视觉对象，新式应用程序需要处理磁贴栏和 "开始" 菜单的图标和磁贴资产。 打开 *appxmanifest.xml* 文件以访问清单设计器。 然后，你可以通过单击 " **创建**"，从项目中的给定映像生成所有视觉资产。

![Visual Studio 中的清单设计器屏幕截图](./media/deploy-modern-applications/manifest-designer.png)

如果你打开 *appxmanifest.xml* 文件的代码，你会看到一些有趣的事情。

在 `<Package>` 中，有一个 `<Identity>` 节点。 你的打包应用程序将通过它来获取其标识，该标识将由操作系统进行管理。

![标识节点](./media/deploy-modern-applications/identity-node.png)

在 `<Capabilities>` 节点中，你可以找到应用程序所需的所有需求，特别注意 `<rescap:Capability Name="runFullTrust" \>` ，这会告诉 OS 以完全信任模式运行应用，因为它是 Win32 应用程序。

![功能节点](./media/deploy-modern-applications/capabilities-node.png)

将打包项目设置为解决方案的启动项目，然后选择 " *运行*"。 这将：

- 编译 Windows 窗体应用程序。
- 从生成结果中创建 .MSIX 包。
- 部署包。
- 在开发计算机上本地安装它。
- 启动应用。

![已安装的应用程序](./media/deploy-modern-applications/our-installed-application.png)

为此，你具有 .MSIX 提供完全集成到 Windows 10 的全新安装和卸载体验。

最后一阶段介绍如何将 .MSIX 包部署到其他计算机。

右键单击打包项目，选择 " **应用商店** " 菜单，然后选择 " **创建应用包** " 选项。

然后，可以选择是创建要上传到存储的包，还是创建用于旁加载的包。 在大多数现代化情况下，你将选择 " **我想要创建旁加载包**"。

![配置包](./media/deploy-modern-applications/configuring-packages.png)

你可以选择要作为目标的不同体系结构，因为你可以在同一个 .MSIX 包中包含所需数量。

最后一步是声明要在何处部署最终安装资产。

![配置更新设置](./media/deploy-modern-applications/configure-update-settings.png)

你可以选择在企业文件服务器上使用共享 UNC 路径的 web 服务器。 请注意设置以指定你希望如何更新应用程序。 下一节将介绍应用程序更新。

有关详细的分步指南，请参阅 [使用 Visual Studio 从源代码打包桌面应用](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。

## <a name="auto-updates-in-msix"></a>.MSIX 中的自动更新

Windows 应用商店使用 Windows 更新具有很好的更新机制。 在大多数企业方案中，你不会使用应用商店分发桌面应用。 因此，你需要一种类似的方式来为应用程序配置更新，并将它们拉取给用户。

结合使用 Windows 10 功能和 .MSIX 包，你可以为用户提供出色的更新体验。 事实上，用户根本不需要技术，但仍可从无缝应用程序更新体验中获益。

可以通过两种不同的方式配置更新以与用户交互：

- 用户提示更新：操作系统会显示一些自动生成的理想 UI，通知用户有关应用程序即将安装的信息。 它基于你在安装文件中指定的属性生成此 UI。

- 后台无提示更新。 如果选择此选项，则用户无需知道更新。

你还可以配置当应用程序启动或定期执行更新的时间。 凭借边载功能，您甚至可以在应用程序运行时获取这些更新。

当你使用此类型的部署时，将创建一个名为 *appinstaller* 的特殊文件。 此简单文件包含以下部分：

- *Appinstaller* 文件的位置
- 应用程序的主 .MSIX 包属性
- 更新行为

![appinstaller 文件](./media/deploy-modern-applications/appinstaller-file.png)

与此文件结合使用，Microsoft 设计了一个特殊的 URL 协议，以从链接启动安装过程：

```xml
<a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/MyApp.msix">Install app package </a>
```

此协议适用于所有浏览器，并在 Windows 10 上通过出色的用户体验启动安装过程。 由于 OS 管理安装过程，因此它知道此应用程序的安装位置，并跟踪该进程影响的所有文件。

.MSIX 创建用于安装的用户界面，该用户界面会自动显示包的某些属性。 这允许为每个应用提供常见的安装体验。

生成新的 .MSIX 包并将其移到部署服务器后，只需编辑 *appinstaller* 文件来反映这些更改，主要是版本和新 .msix 文件的路径。 用户下一次启动应用程序时，系统将检测更改，并在后台下载新版本的文件。 完成此操作后，将以透明方式对用户执行安装。

>[!div class="step-by-step"]
>[上一页](example-migration-core.md)
