---
title: 使用 TryCatch 在 Flowchart 活动中进行错误处理
ms.date: 03/30/2017
ms.assetid: 50922964-bfe0-4ba8-9422-0e7220d514fd
ms.openlocfilehash: 8e3ca59bc9743300a230877a6fbcbed5468a1589
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74710826"
---
# <a name="fault-handling-in-a-flowchart-activity-using-trycatch"></a>使用 TryCatch 在 Flowchart 活动中进行错误处理

此示例演示如何在复杂控制流活动中使用 <xref:System.Activities.Statements.TryCatch> 活动。

在此示例中，将促销代码和孩子数量作为变量传递到 <xref:System.Activities.Statements.Flowchart> 活动，该活动将根据与促销代码相对应的公式计算折扣。 此示例分为命令性代码版本和工作流设计器版本。

下表详细描述了 `CreateFlowchartWithFaults` 活动的变量。

|参数|描述|
|----------------|-----------------|
|promoCode|促销代码。 类型：String<br /><br /> 可能的值，括号中带有说明：<br /><br /> -Single （单个）<br />-MNK （没有儿童结婚。）<br />-MWK （与儿童结婚。）|
|numKids|孩子数量。 类型：int|

`CreateFlowchartWithFaults` 活动使用 <xref:System.Activities.Statements.FlowSwitch%601> 活动，后者根据 `promoCode` 参数进行切换并使用以下公式计算折扣。

|`promoCode` 的值|折扣 (%)|
|--------------------------|--------------------|
|Single|10|
|MNK|15|
|MWK|15 + （1– 1/`numberOfKids`）\*10**注意：** 此计算可能会引发 <xref:System.DivideByZeroException>。 因此，将折扣计算包装在 <xref:System.Activities.Statements.TryCatch> 活动中，该活动可捕获 <xref:System.DivideByZeroException> 异常并将折扣设置为零。|

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2010 打开 FlowchartWithFaultHandling 解决方案文件。

2. 要生成解决方案，按 Ctrl+Shift+B。

3. 若要运行解决方案，请按 F5。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Built-InActivities\FlowChartWithFaultHandling`

## <a name="see-also"></a>另请参阅

- [流程图工作流](../flowchart-workflows.md)
- [异常](../exceptions.md)
