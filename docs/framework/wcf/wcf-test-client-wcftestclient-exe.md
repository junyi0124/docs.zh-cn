---
title: WCF 测试客户端 (WcfTestClient.exe)
description: 了解 WCF 测试客户端，它提供与 WCF 服务主机组合时的无缝服务测试。 提交客户端测试值并查看服务响应。
ms.date: 03/30/2017
ms.assetid: d4302855-677f-4640-aa90-c5d785d72fb7
ms.openlocfilehash: f583d20edf7eeea87ae1dbf63a3cadef05912833
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264122"
---
# <a name="wcf-test-client-wcftestclientexe"></a>WCF 测试客户端 (WcfTestClient.exe)

Windows Communication Foundation (WCF) 测试客户端 ( # A0) 是一个 GUI 工具，用户使用该工具可以输入测试参数、将该输入提交给服务并查看服务发回的响应。 与 WCF 服务主机结合使用时，可以提供无缝的服务测试体验。

通常，你可以在以下位置找到 WCF 测试客户端 ( # A0) ： `C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE` -社区可以是 "企业"、"专业" 或 "社区" 中的一个，具体取决于安装的 Visual Studio 级别。

## <a name="scenarios-for-using-test-client"></a>使用测试客户端的方案

以下各节讨论最常见的方案，在这些方案中，你可以使用 WCF 测试客户端来简化开发过程。

### <a name="inside-visual-studio"></a>Visual Studio 内部

#### <a name="wcf-service-host-starts-wcf-test-client-with-a-single-service"></a>WCF 服务主机启动 WCF 测试客户端和一项服务

创建新的 WCF 服务项目并按 F5 启动调试器后，WCF 服务主机会开始在项目中承载该服务。 然后，WCF 测试客户端打开并显示在配置文件中定义的服务终结点的列表。 可以测试参数和调用服务，并可以重复此过程以继续测试和验证服务。

#### <a name="wcf-service-host-starts-wcf-test-client-with-multiple-services"></a>WCF 服务主机启动 WCF 测试客户端和多项服务

你还可以使用 WCF 测试客户端来帮助调试包含多个服务的服务项目。 在 WCF 测试客户端打开时，它会自动在项目中循环访问服务列表，并将其打开以供测试。

### <a name="outside-visual-studio"></a>Visual Studio 外部

你还可以在 Visual Studio 外部调用 WCF 测试客户端 ( # A0) 来测试 Internet 上的任意服务。 若要找到此工具，请转到以下位置：

`C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE` (其中，社区可以是 "企业"、"专业" 或 "社区" 中的一个，具体取决于计算机上安装的 Visual Studio 级别) 

若要使用该工具，可以双击此文件名从该位置打开它，也可以从命令行启动它。

WCF 测试客户端使用任意数目的 Uri 作为命令行参数。  这些 URI 是可以测试的服务的 URI。

`wcfTestClient.exe URI1 URI2 …`

在 "WCF 测试客户端" 窗口打开后，单击 "**文件**" -> "**添加服务**"，然后输入要打开的服务的终结点地址。

## <a name="wcf-test-client-user-interface"></a>WCF 测试客户端用户界面

可以将 WCF 测试客户端用于单个服务或多个服务。

### <a name="service-operations"></a>服务操作

WCF 测试客户端主窗口的左窗格列出了所有可用的服务及其各自的终结点和操作。

双击某个操作后，可以在具有此操作名称的新选项卡内的右窗格中查看其内容。

左窗格还列出了客户端配置文件。 双击任何项可以在新选项卡式窗口内的右窗格中显示文件的内容。

### <a name="entering-test-parameters"></a>输入测试参数

若要查看测试参数，请双击某个操作以在右侧窗格中打开它。 默认情况下，在 **格式化** 视图中显示参数，您可以为参数输入任意值以测试服务。

若要查看消息的 XML，请单击 " **xml**"。 若要将其发送到服务，请单击 " **调用**"。

对于数据集参数，请单击 **...** 按钮 "**编辑 ...** " 在显示数据网格的新窗口中进行编辑。 请注意 " **复制数据集** " 和 " **粘贴数据集** " 按钮的外观。 如果第一次编辑时 DataSet 对象的架构未知，则 DataGrid 为空。 您必须将具有相同架构的 DataSet 对象粘贴到 DataGrid 中的当前对象。  (请注意，你需要在粘贴操作之前从其他位置复制架构。 ) 你还可以通过单击 " **复制数据集** " 按钮，为将来的使用复制数据集对象。

服务响应将出现在测试参数下面。

> [!NOTE]
> 如果预期的返回值是字符串，则结果将显示为带引号的字符串，即使提供的输入未包含在引号中，也是如此。

如果在创建服务约定时将特定操作指定为单向操作，则不会显示服务响应。 消息一旦处于排队等待发送状态，则会弹出一个对话框，通知您已成功发送消息。

### <a name="session-support"></a>会话支持

使用服务操作的选项卡中的 " **启动新的代理** " 复选框可以切换会话支持。 默认情况下清除此框。

如果为特定操作输入测试参数 (或同一服务终结点中的另一个操作) 并单击 "多次 **调用** " 复选框，则这些操作将共享一个代理，服务状态在多个操作中保持不变。

如果选中了 " **启动新的代理** " 复选框，则将为每个 **调用** 启动一个新的代理，上一个会话方案将结束，并且服务状态将重置。

### <a name="editing-client-configuration"></a>编辑客户端配置

WCF 测试客户端主窗口的左窗格列出了客户端配置文件。 双击任何项可以在右窗格中显示文件的内容。

#### <a name="edit-with-service-configuration-editor"></a>使用服务配置编辑器进行编辑

右键单击左窗格中的 " **配置文件** "，然后选择上下文菜单 " **编辑并 svcconfigeditor.exe**"。 服务配置编辑器启动并显示客户端配置内容。 可以编辑配置，然后将其保存在此工具中。

在服务配置编辑器中保存此文件后，WCF 测试客户端将显示一条警告消息，告知你该文件已在外部修改，并询问你是否要重新加载它。

如果选择 " **是**"，则 "Client.dll.config" 选项卡中的配置内容将反映在编辑器中所做的更改。

如果选择 " **否**"，则 "Client.dll.config" 选项卡中的配置内容将保持不变，并且已修改的内容会自动保存到源文件中。

#### <a name="restore-to-default-configuration"></a>还原到默认配置

如果要取消所有更改并还原到默认的客户端配置，请右键单击左窗格中的 " **配置文件** "，然后选择上下文菜单 " **还原到默认配置**"。将加载默认配置值，并还原 "Client.dll.config" 选项卡中的内容。

#### <a name="validate-changes"></a>验证更改

在 WCF 测试客户端中加载保存的更改时，将根据 WCF 架构检查配置是否有效。 如果发现错误，将显示一个对话框来给出错误详细信息。

在代理生成、二进制编译或服务调用期间，支持编辑的菜单项 (即 "编辑 ..."、"还原 ..." 等) 已禁用。 在将更新的配置加载到 WCF 测试客户端时，也会禁用服务调用。

#### <a name="persist-client-configuration"></a>使客户端配置保持不变

"**工具** -> **选项** -> **客户端配置**" 选项卡包含 "**启动服务时始终重新生成配置**" 选项（默认情况下启用）。 此选项指定每次 WCF 测试客户端加载服务时，它会根据最新的服务协定和服务 App.config 文件重新生成配置文件。

如果已编辑 WCF 服务的客户端配置并希望始终使用此更新的文件来调试服务，则可以取消选中 " **重新生成** " 选项。 这样，即使在更新服务并重新打开 WCF 测试客户端时，Client.dll.config 文件也是您以前更新的文件，而不是根据更新后的服务重新生成的文件。

但是，您可能需要编辑此配置文件以使其与重新生成的代理一致。 如果由于更新了服务而导致重新生成的代理与配置文件不匹配，则调用该服务时将出错。

> [!CAUTION]
> 如果您已修改了客户端配置文件并选择以后再使用它，则可以在以下位置找到此文件：
>
> \Documents 和 Settings \\ [用户帐户] \My Documents\Test 客户端项目。
>
> 任何存储到客户端配置文件的已更新的凭据信息都是由此文件夹的访问控制列表 (ACL) 保护的。

### <a name="adding-removing-and-refreshing-services"></a>添加、删除和刷新服务

#### <a name="add-service"></a>添加服务

单击 "**文件**" " -> **添加服务**" 将服务添加到 WCF 测试客户端。 然后您需要键入要添加的服务的 URI（终结点地址）。 该服务的地址可以是 mex 地址，也可以是 WSDL 地址。

你还可以在 " **最近使用的服务** " 子菜单中找到10个最近添加的服务终结点的列表。 如果选择其中一个，则会将指定的服务添加到 WCF 测试客户端。

您还可以右键单击服务树的根服务 **项目**，然后选择 " **添加服务** " 以获得相同的结果。

在代理生成、二进制编译或服务调用期间，支持添加服务的菜单项被禁用。 服务调用也被禁用。

#### <a name="remove-service"></a>移除服务

右键单击要删除的服务的根服务，然后选择 " **删除服务** " 以从 WCF 测试客户端中删除服务。

在代理生成、二进制编译或服务调用期间，支持删除服务的菜单项被禁用。 服务调用也被禁用。

#### <a name="refresh-service"></a>刷新服务

如果在 WCF 测试客户端运行期间对服务进行了更改，并且想要确保该服务的 WCF 测试客户端实现是最新的，请右键单击该服务的根服务，然后选择 " **刷新服务**"。 请注意，刷新后服务状态将重置。

在代理生成、二进制编译或服务调用期间，支持刷新服务的菜单项被禁用。 服务调用也被禁用。

## <a name="location-of-files-generated-by-the-test-client"></a>测试客户端生成的文件的位置

默认情况下，WCF 测试客户端将生成的客户端代码和配置文件存储在 "%Appdata%\local\temp\test client projects 客户端项目" 文件夹中。 在 WCF 测试客户端退出后删除此文件夹。 如果在 WCF 测试客户端中修改了某个配置文件，并且禁用了 " **启动服务时始终重新生成配置** " 选项，则会将修改后的文件复制到 "我的 Documents\Test 客户端项目" 下的 "CachedConfig" 文件夹中，并 (元数据地址到文件名) XML 文件作为索引。

你还可以在命令行中启动 WCF 测试客户端，使用 `/ProjectPath` 开关指定新的所需路径来存储生成的文件，或者使用 `/RestoreProjectPath` 开关还原默认位置。 语法如下：

`wcfTestClient.exe /ProjectPath [desired location]`

运行此命令不会打开 WCF 测试客户端。 而只会更改文件夹位置。 您可以运行此命令，而不管 WCF 测试客户端是否正在运行。 重新启动 WCF 测试客户端时，将应用新位置。 位置信息可以保存在注册表中，也可以保存在 "%Appdata%\local\temp\test client projects 客户端项目" 文件夹中的 WcfTestClient.exe。

## <a name="features-supported-by-wcf-test-client"></a>WCF 测试客户端支持的功能

以下是 WCF 测试客户端支持的功能列表：

- 服务调用：请求/响应和单向消息。

- 绑定：Svcutil.exe 支持的所有绑定。

- 控制会话。

- 消息协定。

- XML 序列化。

以下是 WCF 测试客户端不支持的功能的列表：

- 类型：<xref:System.IO.Stream>、<xref:System.ServiceModel.Channels.Message>、<xref:System.Xml.XmlElement>、<xref:System.Xml.XmlAttribute>、<xref:System.Xml.XmlNode>、实现 <xref:System.Xml.Serialization.IXmlSerializable> 接口的类型（包括相关 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 特性）、<xref:System.Xml.Linq.XDocument> 和 <xref:System.Xml.Linq.XElement> 类型以及 ADO.NET <xref:System.Data.DataTable> 类型。

- 双工协定。

- 事务。

- 安全性： CardSpace、证书和用户名/密码。

- 绑定：WSFederationbinding、任何上下文绑定和 Https 绑定、WebHttpbinding（Json 响应消息支持）。

## <a name="closing-wcf-test-client"></a>关闭 WCF 测试客户端

可以通过以下方式关闭 WCF 测试客户端：

- 在 **“文件”** 菜单中，单击 **“退出”** 。 或者，在 WCF 测试客户端主窗口中，单击 " **关闭**"。 这两个操作还会关闭 WCF 服务自动主机，并在 Visual Studio 启动 WCF 测试客户端时停止 Visual Studio 调试过程。

- 右键单击通知区域中的 " **WCF 服务主机** " 图标，然后单击 " **退出"。** 这会关闭 WCF 服务自动主机和 WCF 测试客户端，并停止 Visual Studio 调试过程。

## <a name="see-also"></a>另请参阅

- [WCF 服务主机 (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
