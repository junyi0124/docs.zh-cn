---
title: 异步通信
ms.date: 03/30/2017
ms.assetid: 128dc092-9eb2-4e33-9470-9a7f62b60df6
ms.openlocfilehash: db5a8f415479967d7579357bd0c8058c7fb961c5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255840"
---
# <a name="asynchronous-communication"></a>异步通信

此示例演示如何在默认情况下异步执行两个不同 Windows Workflow Foundation (WF) 服务之间的通信。  
  
## <a name="demonstrates"></a>演示  

 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 服务间的异步通信。  
  
## <a name="discussion"></a>讨论 (Discussion)  

 此示例演示如何使用 .NET Framework 提供的消息传递活动来异步执行 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 应用程序间的通信。  
  
 此示例包含以下三个项目。  
  
 CreditCheckService  
 此服务接收特定人员的信用评分或要购买的商品的价值，然后决定是否为该人员提供贷款。  
  
 RentalApprovalService  
 此服务接收来自需要贷款的人员的申请。 此服务与 `CreditCheckService` 进行异步通信以决定贷款申请是否有效。  
  
 客户端  
 此客户端与 `RentalApprovalService` 进行同步通信以了解是否已批准贷款。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 右键单击 **AsynchronousCommunication** 解决方案，然后选择 " **属性**"。  
  
2. 在 " **通用属性**" 中，选择 " **启动项目**"，然后选择 " **多启动项目**"。  
  
3. 将 **RentalApprovalService** 移动到列表中的第一个位置，后跟 **CreditCheckService**，后跟 **客户端**。 在所有三个项目上设置 " **启动** " 操作。  
  
4. 单击 **"确定"**，然后按 F5 运行示例。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\Services\AsynchronousCommunication`
