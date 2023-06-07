---
title: 任务 1：创建一个新的 Windows Presentation Foundation 应用程序
ms.date: 03/30/2017
ms.assetid: 270eaeba-9492-4532-af9f-403ce5c9935b
ms.openlocfilehash: 3205840da575041b449eb841fc8084e89937fca7
ms.sourcegitcommit: 10db6551ea3c971470cf5d2cc21ba1cbcefe5c55
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2019
ms.locfileid: "72031893"
---
# <a name="task-1-create-a-new-windows-presentation-foundation-application"></a>任务 1：创建一个新的 Windows Presentation Foundation 应用程序

在此任务中，您将使用 WPF 应用程序 Visual Studio 模板创建一个空的 Windows Presentation Foundation （WPF）应用程序，并向相应的 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 工作流程序集添加引用。  
  
## <a name="to-create-the-wpf-application-project"></a>创建 WPF 应用程序项目

1. 打开 Visual Studio，在 "**文件**" 菜单上，指向 "**新建**"，然后单击 "**项目**"。

2. 在 "**新建项目**" 对话框中，从框左侧的 "**已安装的模板**" 窗格中选择 "**视觉C#对象**" 或 " **Visual Basic** "。 如果你选择的语言未显示，请在 "**其他语言**" 下查看。

3. 在 "**已安装的模板**" 窗格中选择 "**窗口**"。

4. 在顶部窗格中，确认已在下拉列表框中选择了 " **.NET Framework 4** " （默认值），然后选择 " **WPF 应用程序**"。

5. 在窗口底部，将项目的名称设置为**HostingApplication** 。

6. 将解决方案名称设置为**RehostingTheDesigner**。

7. 单击 **"确定"** 以创建应用程序项目。 Visual Studio 为应用程序创建一个基本 WPF UI，并包含相应的 XAML 和代码隐藏文件。

8. 添加对**WorkflowModel**程序集的引用。 为此，请在**解决方案资源管理器**中，右键单击**HostingApplication**项目，然后选择 "**添加引用**"。

9. 在 "**添加引用**" 对话框中，单击 " **.net** " 选项卡，按住 CTRL 键，选择以下程序集，然后单击 **"确定"** ：

    - System.Activities
    - System.Activities.Presentation
    - System.Activities.Core.Presentation

10. 请参阅[任务2：宿主工作流设计器](task-2-host-the-workflow-designer.md)以了解如何承载工作流设计器设计画布。

## <a name="see-also"></a>另请参阅

- [重新托管工作流设计器](rehosting-the-workflow-designer.md)
- [任务 2：托管工作流设计器](task-2-host-the-workflow-designer.md)
