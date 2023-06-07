---
title: 教程：创建 Windows Communication Foundation 客户端
description: 了解如何通过从 WCF 服务中检索元数据来创建客户端，这是一系列文章中的一部分，可帮助你开始创建 WCF 应用程序。
ms.date: 03/19/2019
helpviewer_keywords:
- clients [WCF], running
- WCF clients [WCF], running
ms.assetid: a67884cc-1c4b-416b-8c96-5c954099f19f
ms.openlocfilehash: d5a2a2b5175c9e34eaf1dbaff20ac57f34117c4e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246319"
---
# <a name="tutorial-create-a-windows-communication-foundation-client"></a>教程：创建 Windows Communication Foundation 客户端

本教程介绍创建基本 Windows Communication Foundation （WCF）应用程序所需的五个任务中的第四个。 有关教程的概述，请参阅[教程： Windows Communication Foundation 应用程序入门](getting-started-tutorial.md)。

用于创建 WCF 应用程序的下一个任务是通过从 WCF 服务中检索元数据来创建客户端。 使用 Visual Studio 添加服务引用，后者从服务的 MEX 终结点中获取元数据。 然后，Visual Studio 将使用你选择的语言生成客户端代理的托管源代码文件。 它还会创建一个客户端配置文件（*App.config*）。 此文件使客户端应用程序能够连接到终结点上的服务。

> [!NOTE]
> 如果从 Visual Studio 中的类库项目调用 WCF 服务，请使用**添加服务引用**功能自动生成代理和关联的配置文件。 但是，由于类库项目不使用此配置文件，因此需要将生成的配置文件中的设置添加到调用类库的可执行文件的*App.config*文件中。

> [!NOTE]
> 作为替代方法，请使用 "配置[元数据" 实用工具](#servicemodel-metadata-utility-tool)（而不是 Visual Studio）生成代理类和配置文件。

客户端应用程序使用生成的代理类与服务通信。 [教程：使用客户端](how-to-use-a-wcf-client.md)中介绍了此过程。

在本教程中，你将了解如何执行以下操作：
> [!div class="checklist"]
>
> - 创建并配置 WCF 客户端的控制台应用程序项目。
> - 将服务引用添加到 WCF 服务，以生成代理类和配置文件。

## <a name="create-a-windows-communication-foundation-client"></a>创建 Windows Communication Foundation 客户端

1. 在 Visual Studio 中创建控制台应用项目：

    1. 从 "**文件**" 菜单中，选择 "**打开**  >  **项目/解决方案**"，并浏览到前面创建的**GettingStarted**解决方案（*GettingStarted*）。 选择“打开”。

    2. 从 "**视图**" 菜单中选择 "**解决方案资源管理器**"。

    3. 在 "**解决方案资源管理器**" 窗口中，选择 " **GettingStarted** " 解决方案（顶级节点），然后从快捷菜单中选择 "**添加**  >  **新项目**"。

    4. 在 "**添加新项目**" 窗口的左侧，选择 " **Visual c #** " 或 " **Visual Basic**下的" **Windows 桌面**"类别。

    5. 选择 "**控制台应用（.NET Framework）** " 模板，然后输入 " *GettingStartedClient* " 作为 "**名称**"。 选择“确定”。

2. 将**GettingStartedClient**项目中的引用添加到 <xref:System.ServiceModel> 程序集：

    1. 在 "**解决方案资源管理器**" 窗口中，选择 " **GettingStartedClient** " 项目下的 "**引用**" 文件夹，然后从快捷菜单中选择 "**添加引用**"。

    2. 在 "**添加引用**" 窗口中窗口左侧的 "**程序集**" 下，选择 "**框架**"。

    3. 找到并选择 " **system.servicemodel**"，然后选择 **"确定"**。

    4. 通过选择 "**文件**" "  >  **全部保存**" 保存解决方案。

3. 将服务引用添加到计算器服务：

   1. 在 "**解决方案资源管理器**" 窗口中，选择 " **GettingStartedClient** " 项目下的 "**引用**" 文件夹，然后从快捷菜单中选择 "**添加服务引用**"。

   2. 在 "**添加服务引用**" 窗口中，选择 "**发现**"。

      CalculatorService 服务启动，Visual Studio 会将其显示在 "**服务**" 框中。

   3. 选择 " **CalculatorService** " 以将其展开并显示服务实现的服务协定。 保留默认**命名空间**，然后选择 **"确定"**。

      Visual Studio 将在**GettingStartedClient**项目中的**连接的服务**文件夹下添加一个新项。

### <a name="servicemodel-metadata-utility-tool"></a>System.servicemodel 元数据实用工具

下面的示例演示如何选择性地使用带[元数据实用工具（Svcutil.exe）](servicemodel-metadata-utility-tool-svcutil-exe.md)生成代理类文件。 此工具将生成代理类文件和*App.config*文件。 下面的示例演示如何在 c # 中生成代理并分别 Visual Basic：

```shell
svcutil.exe /language:cs /out:generatedProxy.cs /config:app.config http://localhost:8000/GettingStarted/CalculatorService
```

```shell
svcutil.exe /language:vb /out:generatedProxy.vb /config:app.config http://localhost:8000/GettingStarted/CalculatorService
```

### <a name="client-configuration-file"></a>客户端配置文件

创建客户端后，Visual Studio 将在**GettingStartedClient**项目中创建**App.config**配置文件，该文件应类似于以下示例：

```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <!-- specifies the version of WCF to use-->
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1" />
        </startup>
        <system.serviceModel>
            <bindings>
                <!-- Uses wsHttpBinding-->
                <wsHttpBinding>
                    <binding name="WSHttpBinding_ICalculator" />
                </wsHttpBinding>
            </bindings>
            <client>
                <!-- specifies the endpoint to use when calling the service -->
                <endpoint address="http://localhost:8000/GettingStarted/CalculatorService"
                    binding="wsHttpBinding" bindingConfiguration="WSHttpBinding_ICalculator"
                    contract="ServiceReference1.ICalculator" name="WSHttpBinding_ICalculator">
                    <identity>
                        <dns value="localhost" />
                    </identity>
                </endpoint>
            </client>
        </system.serviceModel>
    </configuration>
```

在 [\<system.serviceModel>](../configure-apps/file-schema/wcf/system-servicemodel.md) 部分下，请注意 [\<endpoint>](../configure-apps/file-schema/wcf/endpoint-element.md) 元素。 ** &lt; 终结点 &gt; **元素定义客户端用于访问服务的终结点，如下所示：

- 地址： `http://localhost:8000/GettingStarted/CalculatorService` 。 终结点的地址。
- 服务协定： `ServiceReference1.ICalculator` 。 服务协定处理 WCF 客户端和服务之间的通信。 使用其**添加服务引用**函数时，Visual Studio 会生成此约定。 它实质上是在 GettingStartedLib 项目中定义的协定副本。
- 绑定： <xref:System.ServiceModel.WSHttpBinding> 。 绑定将 HTTP 指定为传输、可互操作的安全性和其他配置详细信息。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 创建并配置 WCF 客户端的控制台应用程序项目。
> - 将服务引用添加到 WCF 服务，以生成客户端应用程序的代理类和配置文件。

转到下一教程，了解如何使用生成的客户端。

> [!div class="nextstepaction"]
> [教程：使用 WCF 客户端](how-to-use-a-wcf-client.md)
