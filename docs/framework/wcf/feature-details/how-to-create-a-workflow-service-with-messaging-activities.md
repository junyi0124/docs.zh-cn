---
title: 如何：使用消息传递活动创建工作流服务
ms.date: 03/30/2017
ms.assetid: 53d094e2-6901-4aa1-88b8-024b27ccf78b
ms.openlocfilehash: 21d08d9c3c78cc8774d038018703ffb0c7ceb1fe
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286313"
---
# <a name="how-to-create-a-workflow-service-with-messaging-activities"></a>如何：使用消息传递活动创建工作流服务

此主题说明如何使用消息传递活动创建简单工作流服务， 本主题着重介绍工作流服务的创建机制，其中的服务仅包含消息传递活动。 在实际服务中，工作流包含其他许多活动。 该服务实现一个名为 Echo 的操作，来获取一个字符串并将该字符串返回到调用方。 本主题是一系列主题（包含两个主题）中的第一个。 下一主题 [如何：从工作流应用程序访问服务](how-to-access-a-service-from-a-workflow-application.md) 讨论如何创建可以调用在本主题中创建的服务的工作流应用程序。  
  
### <a name="to-create-a-workflow-service-project"></a>创建工作流服务项目  
  
1. 启动 Visual Studio 2012。  
  
2. 单击 " **文件** " 菜单，选择 " **新建**"，然后单击 " **项目** " 以显示 " **新建项目" 对话框**。 从项目类型列表中，选择已安装模板列表中的 " **工作流** " 和 " **WCF 工作流服务应用程序** "。 将项目命名 `MyWFService` 为，并使用默认位置，如下图所示。  
  
     单击 **"确定"** 按钮以关闭 " **新建项目" 对话框**。  
  
3. 创建项目之后，将在设计器中打开 Service1.xamlx 文件，如以下插图所示。  
  
     ![屏幕截图显示设计器中的 open Service1. .xamlx 文件。](./media/how-to-create-a-workflow-service-with-messaging-activities/default-workflow-service.jpg)  
  
     右键单击标记为 " **顺序服务** " 的活动，然后选择 " **删除**"。  
  
### <a name="to-implement-the-workflow-service"></a>实现工作流服务  
  
1. 选择屏幕左侧的 " **工具箱** " 选项卡以显示 "工具箱"，然后单击图钉使窗口保持打开状态。 展开 "工具箱" 的 " **消息传送** " 部分以显示消息传递活动和消息传递活动模板，如下图所示。  
  
     ![显示已展开消息传递部分的工具箱的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/toolbox-messaging-section.jpg)  
  
2. 将 **ReceiveAndSendReply** 模板拖放到工作流设计器中。 这会创建一个 <xref:System.Activities.Statements.Sequence> 活动，其中包含一个 **接收** 活动，后跟一个 <xref:System.ServiceModel.Activities.SendReply> 活动，如下图所示。  
  
     ![显示 ReceiveAndSendReply 模板的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/receiveandsendreply-template.jpg)  
  
     请注意，已将 <xref:System.ServiceModel.Activities.SendReply> 活动的<xref:System.ServiceModel.Activities.SendReply.Request%2A> 属性设置为 `Receive`，即<xref:System.ServiceModel.Activities.Receive> 活动要答复的<xref:System.ServiceModel.Activities.SendReply> 活动的名称。  
  
3. 在 " <xref:System.ServiceModel.Activities.Receive> 活动类型" 文本框中， `Echo` 将其标记为 **OperationName**。 此步骤定义服务所实现操作的名称。  
  
     ![显示要指定操作名称的位置的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/define-operation-name.jpg)  
  
4. <xref:System.ServiceModel.Activities.Receive>选定活动后，单击 "**视图**" 菜单并选择 "**属性窗口**"，打开 "属性" 窗口（如果尚未打开）。 在 " **属性" 窗口** 中向下滚动，直到看到 " **CanCreateInstance** "，然后单击相应的复选框，如下图所示。 此设置使工作流服务主机可以在接收消息时创建服务的新实例（如果需要）。  
  
     ![显示 CanCreateInstance 属性的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/cancreateinstance-property.jpg)  
  
5. 选择 <xref:System.Activities.Statements.Sequence> 活动，然后单击设计器左下角的 " **变量** " 按钮。 这样将显示变量编辑器。 单击 " **创建变量** " 链接，添加用于存储发送到操作的字符串的变量。 将变量命名为 `msg` ，并将其 **变量** 类型设置为字符串，如下图所示。  
  
     ![显示如何添加变量的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/add-variable-msg-string.jpg)  
  
     再次单击 " **变量** " 按钮以关闭 "变量编辑器"。  
  
6. 单击 " **定义"。** 链接 **，以** <xref:System.ServiceModel.Activities.Receive> 显示 " **内容定义** " 对话框。 选择 "**参数**" 单选按钮，单击 "**添加新参数**" 链接， `inMsg` 在 "**名称**" 文本框中键入，在 "**类型**" 下拉列表框中选择 "**字符串**"，然后 `msg` 在 "**分配到**" 文本框中键入，如下图所示。  
  
     ![显示添加参数内容的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/adding-parameters-content.jpg)  
  
     此步骤指定由 Receive 活动接收字符串参数，并且将该数据绑定到 `msg` 变量。 单击 **"确定"** 以关闭 " **内容定义** " 对话框。  
  
7. 单击活动的 "**内容**" 框中的 "**定义 ...** " 链接 <xref:System.ServiceModel.Activities.SendReply> ，以显示 "**内容定义**" 对话框。 选择 "**参数**" 单选按钮，单击 "**添加新参数**" 链接，在 " `outMsg` **名称**" 文本框中键入，在 "**类型**" 下拉列表框中选择 "**字符串**"，然后在 " `msg` **值**" 文本框中显示，如下图所示。  
  
     ![显示如何添加 outMsg 参数的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/outmsg-parameters-content.jpg)  
  
     此步骤指定由 <xref:System.ServiceModel.Activities.SendReply> 活动发送消息或消息协定类型，并且将该数据绑定到 `msg` 变量。 由于此活动是 <xref:System.ServiceModel.Activities.SendReply> 活动，因此这意味着 `msg` 中的数据用于填充活动发送回客户端的消息。 单击 **"确定"** 以关闭 " **内容定义** " 对话框。  
  
8. 单击 " **生成** " 菜单并选择 " **生成解决方案**"，保存并生成解决方案。  
  
## <a name="configure-the-workflow-service-project"></a>配置工作流服务项目  

 现在工作流服务是完整的。 本节介绍如何配置工作流服务解决方案以易于承载和运行。 此解决方案使用 ASP.NET Development Server 承载服务。  
  
#### <a name="to-set-project-start-up-options"></a>设置项目启动选项  
  
1. 在 **解决方案资源管理器** 中，右键单击 **MyWFService** ，然后选择 " **属性** " 以显示 " **项目属性** " 对话框。  
  
2. 选择 " **Web** " 选项卡，选择 "**启动操作**" 下的 **特定页面**，然后 `Service1.xamlx` 在文本框中键入，如下图所示。  
  
     ![显示 "项目属性" 对话框的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/project-properties-dialog.jpg)  
  
     此步骤在 ASP.NET Development Server 中承载 Service1.xamlx 中定义的服务。  
  
3. 按 Ctrl + F5 启动服务。 ASP.NET Development Server 图标显示在桌面的右下角，如以下图像所示。  
  
     ![显示 ASP.NET 开发人员服务器图标的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/asp-net-dev-server-icon.jpg)  
  
     此外，Internet Explorer 显示服务的 WCF 服务帮助页。  
  
     ![显示 "WCF 服务帮助" 页的屏幕截图。](./media/how-to-create-a-workflow-service-with-messaging-activities/wcf-service-help-page.jpg)  
  
4. 继续 [学习如何：从工作流应用程序访问服务](how-to-access-a-service-from-a-workflow-application.md) 主题来创建调用此服务的工作流客户端。  
  
## <a name="see-also"></a>另请参阅

- [工作流服务](workflow-services.md)
- [承载工作流服务概述](hosting-workflow-services-overview.md)
- [消息传递活动](messaging-activities.md)
