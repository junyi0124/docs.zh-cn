---
title: 教程：承载和运行基本 Windows Communication Foundation 服务
description: 了解如何在控制台应用程序中托管 WCF 服务，这是一系列文章中的一部分，可帮助你开始创建 WCF 应用程序。
ms.date: 03/19/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF services [WCF]
- WCF services [WCF], running
ms.assetid: 31774d36-923b-4e2d-812e-aa190127266f
ms.openlocfilehash: 5318991087e71430523681d601d3b38c4513027b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246124"
---
# <a name="tutorial-host-and-run-a-basic-windows-communication-foundation-service"></a>教程：承载和运行基本 Windows Communication Foundation 服务

本教程介绍创建基本 Windows Communication Foundation （WCF）应用程序所需的五个任务中的第三个。 有关教程的概述，请参阅[教程： Windows Communication Foundation 应用程序入门](getting-started-tutorial.md)。

用于创建 WCF 应用程序的下一个任务是在控制台应用程序中托管 WCF 服务。 WCF 服务公开一个或多个*终结点*，每个终结点都公开一个或多个服务操作。 服务终结点指定下列信息：

- 可在其中找到服务的地址。
- 一个包含描述客户端如何与服务进行通信的信息的绑定。
- 定义服务向其客户端提供的功能的协定。

在本教程中，你将了解如何执行以下操作：
> [!div class="checklist"]
>
> - 创建和配置用于承载 WCF 服务的控制台应用程序项目。
> - 添加代码以承载 WCF 服务。
> - 更新配置文件。
> - 启动 WCF 服务并验证它是否正在运行。

## <a name="create-and-configure-a-console-app-project-for-hosting-the-service"></a>创建和配置用于托管服务的控制台应用程序项目

1. 在 Visual Studio 中创建控制台应用项目：

    1. 从 "**文件**" 菜单中，选择 "**打开**  >  **项目/解决方案**"，并浏览到前面创建的**GettingStarted**解决方案（*GettingStarted*）。 选择“打开”。

    2. 从 "**视图**" 菜单中选择 "**解决方案资源管理器**"。

    3. 在 "**解决方案资源管理器**" 窗口中，选择 " **GettingStarted** " 解决方案（顶级节点），然后从快捷菜单中选择 "**添加**  >  **新项目**"。

    4. 在 "**添加新项目**" 窗口的左侧，选择 " **Visual c #** " 或 " **Visual Basic**下的" **Windows 桌面**"类别。

    5. 选择 "**控制台应用（.NET Framework）** " 模板，然后输入 " *GettingStartedHost* " 作为 "**名称**"。 选择“确定”。

2. 将**GettingStartedHost**项目中的引用添加到**GettingStartedLib**项目：

    1. 在 "**解决方案资源管理器**" 窗口中，选择 " **GettingStartedHost** " 项目下的 "**引用**" 文件夹，然后从快捷菜单中选择 "**添加引用**"。

    2. 在 "**添加引用**" 对话框中窗口左侧的 "**项目**" 下，选择 "**解决方案**"。

    3. 在窗口的中间部分中选择 " **GettingStartedLib** "，然后选择 **"确定"**。

       此操作可使**GettingStartedLib**项目中定义的类型可用于**GettingStartedHost**项目。

3. 将**GettingStartedHost**项目中的引用添加到 <xref:System.ServiceModel> 程序集：

    1. 在 "**解决方案资源管理器**" 窗口中，选择 " **GettingStartedHost** " 项目下的 "**引用**" 文件夹，然后从快捷菜单中选择 "**添加引用**"。

    2. 在 "**添加引用**" 窗口中窗口左侧的 "**程序集**" 下，选择 "**框架**"。

    3. 选择 " **system.servicemodel**"，然后选择 **"确定"**。

    4. 通过选择 "**文件**" "  >  **全部保存**" 保存解决方案。

## <a name="add-code-to-host-the-service"></a>添加用于托管服务的代码

若要承载服务，请添加代码以执行以下步骤：

1. 创建基址的 URI。
2. 创建用于承载服务的类实例。
3. 创建服务终结点。
4. 启用元数据交换。
5. 打开服务主机以侦听传入消息。
  
对代码进行以下更改：

1. 在**GettingStartedHost**项目中打开**Program.cs**或**Module1**文件，并将其代码替换为以下代码：

    ```csharp
    using System;
    using System.ServiceModel;
    using System.ServiceModel.Description;
    using GettingStartedLib;

    namespace GettingStartedHost
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Step 1: Create a URI to serve as the base address.
                Uri baseAddress = new Uri("http://localhost:8000/GettingStarted/");

                // Step 2: Create a ServiceHost instance.
                ServiceHost selfHost = new ServiceHost(typeof(CalculatorService), baseAddress);

                try
                {
                    // Step 3: Add a service endpoint.
                    selfHost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(), "CalculatorService");

                    // Step 4: Enable metadata exchange.
                    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
                    smb.HttpGetEnabled = true;
                    selfHost.Description.Behaviors.Add(smb);

                    // Step 5: Start the service.
                    selfHost.Open();
                    Console.WriteLine("The service is ready.");

                    // Close the ServiceHost to stop the service.
                    Console.WriteLine("Press <Enter> to terminate the service.");
                    Console.WriteLine();
                    Console.ReadLine();
                    selfHost.Close();
                }
                catch (CommunicationException ce)
                {
                    Console.WriteLine("An exception occurred: {0}", ce.Message);
                    selfHost.Abort();
                }
            }
        }
    }
    ```

    ```vb
    Imports System.ServiceModel
    Imports System.ServiceModel.Description
    Imports GettingStartedLib.GettingStartedLib

    Module Service

        Class Program
            Shared Sub Main()
                ' Step 1: Create a URI to serve as the base address.
                Dim baseAddress As New Uri("http://localhost:8000/GettingStarted/")

                ' Step 2: Create a ServiceHost instance.
                Dim selfHost As New ServiceHost(GetType(CalculatorService), baseAddress)
               Try

                    ' Step 3: Add a service endpoint.
                    selfHost.AddServiceEndpoint( _
                        GetType(ICalculator), _
                        New WSHttpBinding(), _
                        "CalculatorService")

                    ' Step 4: Enable metadata exchange.
                    Dim smb As New ServiceMetadataBehavior()
                    smb.HttpGetEnabled = True
                    selfHost.Description.Behaviors.Add(smb)

                    ' Step 5: Start the service.
                    selfHost.Open()
                    Console.WriteLine("The service is ready.")

                    ' Close the ServiceHost to stop the service.
                    Console.WriteLine("Press <Enter> to terminate the service.")
                    Console.WriteLine()
                    Console.ReadLine()
                    selfHost.Close()

                Catch ce As CommunicationException
                    Console.WriteLine("An exception occurred: {0}", ce.Message)
                    selfHost.Abort()
                End Try
            End Sub
        End Class

    End Module
    ```

    有关此代码的工作原理的信息，请参阅[服务托管程序步骤](#service-hosting-program-steps)。

2. 更新项目属性：

   1. 在 "**解决方案资源管理器**" 窗口中，选择 " **GettingStartedHost** " 文件夹，然后从快捷菜单中选择 "**属性**"。

   2. 在**GettingStartedHost**属性页上，选择 "**应用程序**" 选项卡：

      - 对于 c # 项目，请从 "**启动对象**" 列表中选择 " **GettingStartedHost** "。

      - 对于 Visual Basic 项目，从 "**启动对象**" 列表中选择 "**服务**"。

   3. 从 "**文件**" 菜单中，选择 "**全部保存**"。

## <a name="verify-the-service-is-working"></a>验证服务是否正常运行

1. 生成解决方案，然后从 Visual Studio 内部运行**GettingStartedHost**控制台应用程序。

    必须以管理员权限运行该服务。 由于你以管理员权限打开了 Visual Studio，因此，当你在 Visual Studio 中运行**GettingStartedHost**时，该应用程序也会以管理员权限运行。 作为替代方法，您可以以管理员身份打开新的命令提示符（从快捷菜单中选择 "**更多**以  >  **管理员身份运行**"），并在其中运行**GettingStartedHost.exe** 。

2. 打开 web 浏览器并浏览到中的服务页面 `http://localhost:8000/GettingStarted/CalculatorService` 。

   > [!NOTE]
   > 此类服务需要适当的权限才能在计算机上注册 HTTP 地址以进行侦听。 管理员帐户具有此权限，但对于非管理员帐户来说，必须授予对 HTTP 命名空间的权限。 有关如何配置命名空间预留的详细信息，请参阅[配置 HTTP 和 HTTPS](feature-details/configuring-http-and-https.md)。

## <a name="service-hosting-program-steps"></a>服务托管计划步骤

你添加用于承载服务的代码中的步骤如下所述：

- **步骤 1**：创建类的实例 `Uri` 以承载服务的基址。 包含基址的 URL 具有标识服务的可选 URI。 基址的格式如下所示： `<transport>://<machine-name or domain><:optional port #>/<optional URI segment>` 。 计算器服务的基址使用 HTTP 传输、localhost、端口8000和 URI 段 GettingStarted。

- **步骤 2**：创建用于 <xref:System.ServiceModel.ServiceHost> 承载服务的类的实例。 构造函数采用两个参数：实现服务协定的类的类型和服务的基址。

- **步骤 3**：创建 <xref:System.ServiceModel.Description.ServiceEndpoint> 实例。 服务终结点由地址、绑定和服务协定组成。 <xref:System.ServiceModel.Description.ServiceEndpoint>构造函数由服务协定接口类型、绑定和地址组成。 服务协定是在服务类型中定义和实现的 `ICalculator`。 此示例的绑定为 <xref:System.ServiceModel.WSHttpBinding> ，它是内置绑定并连接到符合 WS-* 规范的终结点。 有关 WCF 绑定的详细信息，请参阅[wcf 绑定概述](bindings-overview.md)。 将地址追加到基址以标识终结点。 该代码将地址指定为 CalculatorService，将终结点的完全限定地址指定为 `http://localhost:8000/GettingStarted/CalculatorService` 。

    > [!IMPORTANT]
    > 对于 .NET Framework 版本4及更高版本，添加服务终结点是可选的。 对于这些版本，如果不添加代码或配置，WCF 将为服务实现的每个基址和协定组合添加一个默认终结点。 有关默认终结点的详细信息，请参阅[指定终结点地址](specifying-an-endpoint-address.md)。 有关默认终结点、绑定和行为的详细信息，请参阅[WCF 服务的](samples/simplified-configuration-for-wcf-services.md)[简化配置](simplified-configuration.md)和简化配置。

- **步骤 4**：启用元数据交换。 客户端使用元数据交换生成代理来调用服务操作。 若要启用元数据交换，请创建一个 <xref:System.ServiceModel.Description.ServiceMetadataBehavior> 实例，将其 <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled> 属性设置为 `true` ，然后将该 `ServiceMetadataBehavior` 对象添加到实例的集合中 <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> <xref:System.ServiceModel.ServiceHost> 。

- **步骤 5**：打开 <xref:System.ServiceModel.ServiceHost> 以侦听传入的消息。 应用程序等待你按**enter**。 在应用程序实例化后 <xref:System.ServiceModel.ServiceHost> ，它将执行 try/catch 块。 有关安全捕获引发的异常的详细信息 <xref:System.ServiceModel.ServiceHost> ，请参阅[使用 Close 和 ABORT 释放 WCF 客户端资源](samples/use-close-abort-release-wcf-client-resources.md)。

> [!IMPORTANT]
> 添加 WCF 服务库时，如果你通过启动服务主机进行调试，则 Visual Studio 会为你托管它。 若要避免冲突，可以阻止 Visual Studio 承载 WCF 服务库。
>
> 1. 在**解决方案资源管理器**中选择 " **GettingStartedLib** " 项目，然后从快捷菜单中选择 "**属性**"。
> 2. 选择 " **Wcf 选项**" 并取消选中 **"在调试同一解决方案中的另一个项目时启动 WCF 服务主机"**。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 创建和配置用于承载 WCF 服务的控制台应用程序项目。
> - 添加代码以承载 WCF 服务。
> - 更新配置文件。
> - 启动 WCF 服务并验证它是否正在运行。

转到下一教程，了解如何创建 WCF 客户端。

> [!div class="nextstepaction"]
> [教程：创建 WCF 客户端](how-to-create-a-wcf-client.md)
