---
title: 工作流事务
ms.date: 03/30/2017
ms.assetid: 6081fb02-c0f2-483d-97b8-f3b7dc03011d
ms.openlocfilehash: 223c18195c8ffd0b51eb5dff77aa81953f6e47a1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79182678"
---
# <a name="workflow-transactions"></a>工作流事务

[!INCLUDE[wf1](../../../includes/wf1-md.md)] 支持通过使用 <xref:System.Transactions> 活动确定事务处理工作单元的范围来参与 <xref:System.Activities.Statements.TransactionScope> 事务。 尽管必须显式完成 <xref:System.Transactions.TransactionScope?displayProperty=nameWithType>，但 <xref:System.Activities.Statements.TransactionScope?displayProperty=nameWithType> 活动在成功完成后将对事务隐式调用完成。 <xref:System.Activities.Statements.TransactionScope.Body%2A> 活动的 <xref:System.Activities.Statements.TransactionScope> 中包含的所有活动都会参与该事务。 通过使用 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动，WF 可以使事务流入某个工作流中。 与 <xref:System.Activities.Statements.TransactionScope> 活动一样，<xref:System.ServiceModel.Activities.TransactedReceiveScope.Body%2A> 中包含的所有活动都会参与该事务。 WF 确保依赖于 <xref:System.Transactions.Transaction.Current%2A?displayProperty=nameWithType> 的活动使用 <xref:System.Activities.Statements.TransactionScope> 和 <xref:System.ServiceModel.Activities.TransactedReceiveScope>。 如果系统提供的活动无法满足所有需求，可以使用 <xref:System.Activities.RuntimeTransactionHandle> 生成自定义活动，以便支持高级流控制和事务控制方案。  
  
在下面的示例中，工作流由包含子活动（包括<xref:System.Activities.Statements.Sequence><xref:System.Activities.Statements.TransactionScope>活动）的活动组成。 <xref:System.Activities.Statements.TransactionScope.Body%2A> 的 <xref:System.Activities.Statements.TransactionScope> 活动在 <xref:System.Activities.Statements.TransactionScope> 活动初始化的事务下执行。  
  
```csharp  
static Activity ScenarioOne()  
{  
    return new Sequence  
    {  
        Activities =  
        {  
            new WriteLine { Text = "    Begin workflow" },  
  
            new TransactionScope  
            {  
                Body = new Sequence  
                {  
                    Activities =
                    {  
                        new WriteLine { Text = "    Begin TransactionScope" },  
  
                        new PrintTransactionId(),  
  
                        new TransactionScopeTest(),  
  
                        new WriteLine { Text = "    End TransactionScope" },  
                    },  
                },  
            },  
  
            new WriteLine { Text = "    End workflow" },  
        }  
    };  
}  
```  
  
有关详细信息，请参阅有关使用<xref:System.ServiceModel.Activities.TransactedReceiveScope>的[请参阅将事务流入和流出工作流服务](../wcf/feature-details/flowing-transactions-into-and-out-of-workflow-services.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Statements.TransactionScope>
- <xref:System.Transactions.TransactionScope>
- <xref:System.Transactions.Transaction.Current%2A?displayProperty=nameWithType>
