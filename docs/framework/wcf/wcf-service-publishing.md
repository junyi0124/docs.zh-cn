---
title: WCF 服务发布
description: WCF 服务发布可帮助你将应用程序部署到生产环境，以便进行测试。
ms.date: 03/30/2017
ms.assetid: c806b253-cd47-4b96-b831-e73cbf08808f
ms.openlocfilehash: ac817dac15deaf35fdb8e078094dd4b9dca97d06
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293477"
---
# <a name="wcf-service-publishing"></a>WCF 服务发布

Windows Communication Foundation (WCF) 服务发布可帮助你从 WCF 服务主机和 WCF 测试客户端提供的早期开发环境中进行开发，以便将应用程序实际部署到生产环境中以进行测试。 在提交到最终部署计划之前，可以使用 Windows Communication Foundation (WCF) Service 发布验证 WCF 服务是否正确执行并且已准备好发布。 还可以选择将 WCF 服务库部署到各种目标位置进行测试。

## <a name="supported-services-and-target-locations"></a>支持的服务和目标位置

WCF 服务发布支持发布从一组 WCF 服务库模板创建的 WCF 服务及其对应的项模板，其中包括：

- 带有项模板的 WCF 服务库模板。

- 联合服务库。

可以通过选择 "**文件**" "  >  **新建项目**" > [**Visual Basic** 或 **Visual c #**] > **WCF** 来查找这些服务模板。 对于此位置中的其他 WCF 模板 (包括 WCF 工作流服务应用程序和 WCF 服务应用程序) ，你可以使用 [web 应用程序的一键式发布](/previous-versions/aspnet/dd465337(v=vs.110))来发布。

可以将服务发布到以下目标位置。

- 本地 IIS。

- 文件系统。

- FTP 站点。

## <a name="using-wcf-service-publishing"></a>使用 WCF 服务发布

若要部署服务实现，请执行以下步骤：

1. 以提升的权限打开 Visual Studio (右键单击该可执行文件，然后选择 "以 **管理员身份运行** " 以将其打开) 。  如果使用的是 IIS 7.0 或更高版本，请确保已使用控制面板中的 "打开或关闭 Windows 功能" 安装了 "IIS 元数据库和 IIS6 配置兼容性" 组件。

2. 打开服务项目，从主菜单中选择 "**生成**  >  **发布 \<Project Name>** "，或者右键单击 "**解决方案资源管理器** 中的项目，然后单击"**发布**"。

3. 此时将显示 " **发布** " 窗口。 单击 " **..."。** 按钮指定服务应部署到的目标位置。 你可以选择将应用程序部署到本地 IIS、文件系统或 FTP 站点。 如果将应用程序部署到本地 IIS，则可选择网站并在其下创建 web 应用程序，方法是单击右上角的 " **新建 Web 应用程序** " 图标。

4. 在主窗口中单击 " **发布** " 后，Visual Studio 会将应用程序部署到指定的目标位置，并将 Web.config、.svc 和程序集文件复制到目标目录。 . .Svc 的名称为 "项目名称. ServiceName. .svc 成功发布服务后，可以在 Visual Studio 的 "输出" 窗口中找到 hotlink，这类似于 "连接到 `http://localhost/WebApplicationFolderName...` "。 可在按住 Ctrl 键的同时单击该链接来打开 Visual Studio 中的浏览器页面，以便查看服务目录结构。

     如果您无法浏览到网站，这可能是因为 IIS 中未启用目录浏览器。 请按照 "可以尝试的操作" 一节中的提示进行操作。 或者，你可以直接键入 `http://localhost/WebApplicationFolderName/ProjectName.ServiceName.svc` 以查看服务页面。

您可以使用 " **发布** " 来指定是否要将项目中定义的所有服务的程序集、配置和 .svc 文件复制到目标位置，并覆盖目标位置中的现有文件。

如果选择将应用程序部署到本地 IIS，则可能会遇到与 IIS 设置有关的错误。 请确保已正确安装 IIS。 可以 `http://localhost` 在浏览器的地址栏中输入，检查 IIS 默认页面是否显示。 在某些情况下，这些问题也可能是由于不正确地在 IIS 中注册 ASP.NET 或 WCF 引起的。 可以打开 Visual Studio 的开发人员命令提示，并运行命令 `aspnet_regiis.exe -ir` 来修复 ASP.NET 注册问题，或运行命令 `ServiceModelReg.exe –ia` 来修复 WCF 注册问题。

## <a name="files-generated-for-publishing"></a>生成的待发布文件

 在承载 WCF 服务库之前，该工具会生成以下文件：程序集文件、Web.config 文件和 .svc 文件。 这些文件全部复制到目标位置。 然后发布该服务。

### <a name="assembly-files"></a>程序集文件

 使用此工具发布 WCF 服务时，将首先自动生成服务，并在生成后在服务项目中生成程序集文件。

### <a name="svc-file"></a>.SVC 文件

 如果文件存在，则发布操作将为每个 WCF 服务生成 * .svc 文件，以确保版本有效性。 有两种不同类型的 svc 文件：一个用于 WCF 服务库和联合服务库，另一个用于顺序和状态机工作流服务库。 生成的 \* .svc 文件将复制到目标位置的根文件夹中。

### <a name="webconfig-file"></a>Web.config 文件

 每次将服务项目发布到特定目标位置时，都会创建一个 Web.config 文件。

 生成的 Web.config 文件包括可用于 Web 托管的 Web 部分，以及包含以下更改的 WCF 服务库 App.config 的内容：

- 排除了基址。

- 排除了 `<diagnostics>` 元素中的设置以保留目标平台的跟踪设置。

## <a name="publishing-wcf-services-with-non-http-bindings-to-iis"></a>将具有非 HTTP 绑定的 WCF 服务发布到 IIS

 如果使用的是 IIS 7.0 或更高版本，则可以将具有非 HTTP 绑定的 WCF 服务发布到 IIS。 需要进行一些预配置。 有关详细信息，请参阅  [Windows 进程激活服务中的托管](./feature-details/hosting-in-windows-process-activation-service.md)主题。

## <a name="security"></a>安全性

 发布到本地 IIS 需要具有管理员权限，原因是 IIS 要求以 Administrator 帐户身份运行。 如果没有管理员权限的用户打开 WCF 服务发布，则 IIS 不可用作目标位置。 在没有管理员权限的情况下发布到文件系统或 FTP 站点工作。

## <a name="see-also"></a>另请参阅

- [WCF Visual Studio 模板](wcf-vs-templates.md)
- [WCF 服务主机 (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
- [WCF 测试客户端 (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)
