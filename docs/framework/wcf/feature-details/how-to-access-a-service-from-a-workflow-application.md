---
title: 如何：从工作流应用程序访问服务
ms.date: 03/30/2017
ms.assetid: 925ef8ea-5550-4c9d-bb7b-209e20c280ad
ms.openlocfilehash: 13fae7dec3026e96e3c196467da29fe768a3655f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257927"
---
# <a name="how-to-access-a-service-from-a-workflow-application"></a>如何：从工作流应用程序访问服务

本主题说明如何从工作流控制台应用程序调用工作流服务。 具体取决于 [如何：使用消息传递活动创建工作流服务](how-to-create-a-workflow-service-with-messaging-activities.md) 主题。 尽管本主题介绍如何从工作流应用程序调用工作流服务，但也可以使用相同的方法从工作流应用程序调用任何 Windows Communication Foundation (WCF) 服务。

### <a name="create-a-workflow-console-application-project"></a>创建工作流控制台应用程序项目

1. 启动 Visual Studio 2012。

2. 加载在 [如何：创建包含消息传递活动的工作流服务](how-to-create-a-workflow-service-with-messaging-activities.md) 主题中创建的 MyWFService 项目。

3. 在 **解决方案资源管理器** 中右键单击 " **MyWFService** " 解决方案，然后选择 "**添加**"、"**新建项目**"。 从项目类型列表中的 "**已安装的模板** 和 **工作流" 控制台应用程序** 中选择 "**工作流**"。 将项目命名为 MyWFClient 并保存在默认位置，如以下插图所示。

     ![“添加新项目”对话框](./media/how-to-access-a-service-from-a-workflow-application/add-new-project-dialog.jpg)

     单击 **"确定"** 按钮以关闭 " **添加新项目" 对话框**。

4. 创建项目以后，将在设计器中打开 Workflow1.xaml 文件。 单击 " **工具箱** " 选项卡以打开 "工具箱" （如果尚未打开），然后单击图钉使 "工具箱" 窗口保持打开状态。

5. 按 **Ctrl** + **F5** 生成并启动该服务。 与以前一样，将启动 ASP.NET Development Server，并且 Internet Explorer 将显示 WCF 帮助页。 请注意该页面的 URI，因为在下一步中，您必须使用它。

     ![显示 WCF 帮助页和 URI 的 IE](./media/how-to-access-a-service-from-a-workflow-application/ie-wcf-help-page-uri.jpg)

6. 右键单击 **解决方案资源管理器** 中的 **MyWFClient** 项目，然后选择 "**添加**  >  **服务引用**"。 单击 " **发现** " 按钮，在当前解决方案中搜索任何服务。 在“服务”列表中，单击 Service1.xamlx 旁边的三角形。 单击 Service1 旁边的三角形列出由 Service1 服务实现的约定。 展开 "**服务**" 列表中的 **Service1** 节点。 Echo 操作显示在 " **操作** " 列表中，如下图所示。

     ![“添加服务引用”对话框](./media/how-to-access-a-service-from-a-workflow-application/add-service-reference.jpg)

     保留默认命名空间，然后单击 **"确定"** 以关闭 **添加服务引用** 对话框。 将显示以下对话框。

     ![添加服务引用通知对话框](./media/how-to-access-a-service-from-a-workflow-application/add-service-reference-dialog.jpg)

     单击 **"确定"** 以关闭对话框。 接下来，按 Ctrl+Shift+B 生成解决方案。 请注意，在工具箱中添加了一个名为 **MyWFClient** 的新部分。 展开此部分，请注意，此时已添加“Echo”活动，如以下插图所示。

     ![工具箱中的回显活动](./media/how-to-access-a-service-from-a-workflow-application/echo-activity-toolbox.jpg)

7. 将 <xref:System.Activities.Statements.Sequence> 活动拖放到设计器图面。 它位于 "工具箱" 的 " **控制流** " 部分下。

8. <xref:System.Activities.Statements.Sequence>在焦点为的活动中，单击 "**变量**" 链接，并添加一个名为的字符串变量 `inString` 。 为变量指定默认值以及名为的 `"Hello, world"` 字符串变量，如下 `outString` 图所示。

     ![添加 inString 变量](./media/how-to-access-a-service-from-a-workflow-application/add-instring-variable.jpg)

9. 将 **回显** 活动拖放到中 <xref:System.Activities.Statements.Sequence> 。 在 "属性" 窗口中，将参数和变量的参数绑定 `inMsg` 到 `inString` 变量，如下 `outMsg` `outString` 图所示。 这样会将 `inString` 变量的值传入操作中，然后获取返回值，并将返回值放到 `outString` 变量中。

     ![将自变量绑定到变量](./media/how-to-access-a-service-from-a-workflow-application/bind-arguments-variables.jpg)

10. 将 " **WriteLine** " 活动拖放到 **回响** 活动下面以显示服务调用返回的字符串。 " **WriteLine** " 活动位于 "工具箱" 的 " **基元** " 节点中。 **Text** **WriteLine** `outString` 通过 `outString` 在 **writeline** 活动的文本框中键入内容，将 WriteLine 活动的 text 参数绑定到变量。 现在，此工作流应该如以下插图所示。

     ![已完成的客户端工作流](./media/how-to-access-a-service-from-a-workflow-application/complete-client-workflow.jpg)

11. 右键单击 MyWFService 解决方案，然后选择 "**设置启动项目 ...**"。选择 "**多启动项目**" 单选按钮，然后在 "**操作**" 列中为每个项目选择 "**启动**"，如下图所示。

     ![启动项目选项](./media/how-to-access-a-service-from-a-workflow-application/startup-project-options.jpg)

12. 按 Ctrl+F5 启动服务和客户端。 ASP.NET 开发服务器承载服务，Internet Explorer 将显示 WCF 帮助页，客户端工作流应用程序在控制台窗口中启动，并显示从服务返回的字符串 "Hello，world" )  (。

## <a name="see-also"></a>另请参阅

- [工作流服务](workflow-services.md)
- [如何：使用消息传递活动创建工作流服务](how-to-create-a-workflow-service-with-messaging-activities.md)
- [从 Web 项目的工作流中使用 WCF 服务](/archive/blogs/endpoint/how-to-consume-a-wcf-service-from-a-wf4-workflow)
