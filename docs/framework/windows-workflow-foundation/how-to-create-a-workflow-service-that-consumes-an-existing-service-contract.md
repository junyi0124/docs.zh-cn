---
title: 如何：创建使用现有服务协定的工作流服务
ms.date: 03/30/2017
ms.assetid: 11d11b59-acc4-48bf-8e4b-e97b516aa0a9
ms.openlocfilehash: 05c59bde424049eb5bef8f8bd09c472b58eaa9ef
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248820"
---
# <a name="how-to-create-a-workflow-service-that-consumes-an-existing-service-contract"></a>如何：创建使用现有服务协定的工作流服务

.NET Framework 4.5 以协定优先工作流开发的形式，在 web 服务和工作流之间更好地集成。 通过协定优先的工作流开发工具，可以在代码中优先设计协定。 然后该工具在工具箱中为协定中的操作自动生成活动模板。  
  
> [!NOTE]
> 本主题提供了有关创建协定优先工作流服务的分步指南。 有关协定优先工作流服务开发的详细信息，请参阅 [约定优先工作流服务开发](contract-first-workflow-service-development.md)。  
  
### <a name="creating-the-workflow-project"></a>创建工作流项目  
  
1. 在 Visual Studio 中，选择“文件”“新建项目”。 选择 "**模板**" 树中 " **c #** " 节点下的 " **wcf** " 节点，并选择 " **wcf 工作流服务应用程序**" 模板。  
  
2. 为新项目命名 `ContractFirst` ，然后单击 **"确定"**。  
  
### <a name="creating-the-service-contract"></a>创建服务协定  
  
1. 在 **解决方案资源管理器** 中右键单击该项目，然后选择 " **添加**"、" **新建项 ...**"。 选择左侧的 " **代码** " 节点，并在右侧选择 " **类** " 模板。 将新类命名为 `IBookService` ，然后单击 **"确定"**。  
  
2. 在显示的代码窗口的顶部，向 `System.Servicemodel` 添加一个 Using 语句。  
  
    ```csharp  
    using System.ServiceModel;  
    ```  
  
3. 将示例类定义更改为以下接口定义。  
  
    ```csharp  
    [ServiceContract]  
        public interface IBookService  
        {  
            [OperationContract]  
            void Buy(string bookName);  
  
            [OperationContract(IsOneWay=true)]  
            void Checkout();  
        }  
    ```  
  
4. 按 **Ctrl + Shift + B** 生成项目。  
  
### <a name="importing-the-service-contract"></a>导入服务协定  
  
1. 在 **解决方案资源管理器** 中右键单击该项目，然后选择 " **导入服务协定**"。 在下 **\<Current Project>** ，打开所有子节点并选择 " **IBookService**"。 单击" **确定**"。  
  
2. 将会打开一个对话框，提示您该操作已成功完成，并且在生成该项目后，所生成的活动将出现在工具箱中。 单击" **确定**"。  
  
3. 按 **Ctrl + Shift + B** 生成项目，以便导入的活动将显示在工具箱中。  
  
4. 在 **解决方案资源管理器** 中，打开 .xamlx。 该工作流服务将出现在设计器中。  
  
5. 选择 " **序列** " 活动。 在属性窗口中，单击 **...** 按钮的 **ImplementedContract** 。 在出现的 "**类型集合编辑器**" 窗口中，单击 "**类型**" 下拉列表，然后选择 "**浏览类型 ...** " 项。 在 " **浏览并选择 .Net 类型** " 对话框的 "" 下， **\<Current Project>** 打开 "所有子节点"，然后选择 " **IBookService**"。 单击" **确定**"。 在 " **类型集合编辑器** " 对话框中，单击 **"确定"**。  
  
6. 选择并删除 " **ReceiveRequest** " 和 " **SendResponse** " 活动。  
  
7. 从 "工具箱" 中，将 " **Buy_ReceiveAndSendReply** " 和 " **Checkout_Receive** " 活动拖到 " **顺序服务** " 活动上。
