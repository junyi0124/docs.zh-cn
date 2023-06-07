---
title: 配置工作流服务中的序列化
ms.date: 03/30/2017
ms.assetid: aa70b290-a2ee-4c3c-90ea-d0a7665096ae
ms.openlocfilehash: d1cda33c8a469b4a54b6d282b99c07b23e91543e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96284194"
---
# <a name="configuring-serialization-in-a-workflow-service"></a>配置工作流服务中的序列化

工作流服务 Windows Communication Foundation (WCF) 服务，因此可以选择使用 <xref:System.Runtime.Serialization.DataContractSerializer> (默认) 或 <xref:System.Xml.Serialization.XmlSerializer> 。 编写非工作流服务时，将在服务或操作协定上指定要使用的序列化程序的类型。 创建 WCF 工作流服务时，不在代码中指定这些协定，而是在运行时通过协定推理生成这些协定。 有关协定推理的详细信息，请参阅  [在工作流中使用约定](using-contracts-in-workflow.md)。  序列化程序是使用 <xref:System.ServiceModel.Activities.Receive.SerializerOption%2A> 属性指定的。 这可在设计器中进行设置，如下图所示。  
  
 ![在 "属性" 窗口中设置 SerializerOption 属性。](./media/configuring-serialization-in-a-workflow-service/setting-serializer-property.png)  
  
 序列化程序也可在代码中进行设置，如下面的示例所示。  
  
```csharp  
Receive approveExpense = new Receive  
            {  
                OperationName = "ApproveExpense",  
                CanCreateInstance = true,  
                ServiceContractName = "FinanceService",  
                SerializerOption = SerializerOption.DataContractSerializer,  
                Content = ReceiveContent.Create(new OutArgument<Expense>(expense))  
            };  
```  
  
  也可在工作流服务上指定已知类型。 有关已知类型的详细信息，请参阅 [数据协定已知类型](data-contract-known-types.md)。 可在设计器或代码中指定已知类型。 若要在设计器中指定已知类型，请在活动的 " **属性" 窗口** 中单击 "KnownTypes" 属性旁边的省略号按钮，如下 <xref:System.ServiceModel.Activities.Receive> 图所示。
  
 !["KnownTypes 属性" 对话框的屏幕截图。](./media/configuring-serialization-in-a-workflow-service/known-types-properties.png)  
  
 此时将显示 "类型集合编辑器"，可以在其中搜索和指定已知类型。  
  
 ![类型集合编辑器的屏幕截图。](./media/configuring-serialization-in-a-workflow-service/type-collection-editor.gif)  
  
 单击 " **添加新类型** " 链接，然后使用下拉选择或搜索要添加到已知类型集合的类型。 若要在代码中指定已知类型，请使用 <xref:System.ServiceModel.Activities.Receive.KnownTypes%2A> 属性，如下面的示例所示。  
  
```csharp
Receive approveExpense = new Receive  
            {  
                OperationName = "ApproveExpense",  
                CanCreateInstance = true,  
                ServiceContractName = "FinanceService",  
                SerializerOption = SerializerOption.DataContractSerializer,  
                Content = ReceiveContent.Create(new OutArgument<Expense>(expense))  
            };  
            approveExpense.KnownTypes.Add(typeof(Travel));  
            approveExpense.KnownTypes.Add(typeof(Meal));  
```
