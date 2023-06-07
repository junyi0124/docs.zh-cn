---
title: 补偿
ms.date: 03/30/2017
ms.assetid: 722e9766-48d7-456c-9496-d7c5c8f0fa76
ms.openlocfilehash: 60e2789c4bbd29185af50eaf6555307b894d3902
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289186"
---
# <a name="compensation"></a>补偿

 (WF) Windows Workflow Foundation 中的补偿是一种机制，通过该机制，可以根据应用程序定义的逻辑来撤消或 (补偿之前完成的工作，) 发生后续失败时。 本节介绍如何在工作流中使用补偿。  
  
## <a name="compensation-vs-transactions"></a>补偿与事务  

 通过事务可以将多个操作合并为单个工作单元。 使用事务时，如果事务进程中任何部分出现错误，则你的应用程序可以中止（回滚）在事务内执行的所有更改。 但是，如果工作长时间运行，使用事务可能不合适。 例如，一个作为工作流实现的差旅计划应用程序。 该工作流的步骤可能包含预订航班、等待经理批准，然后支付机票费用。 这个过程会花费几天的时间，不适合将预订航班步骤和支付机票费用步骤合并到同一事务中。 在此类方案中，如果在以后的处理中出现失败，可以使用补偿来撤消工作流的预订步骤。  
  
> [!NOTE]
> 本主题介绍工作流中的补偿。 有关工作流中事务的详细信息，请参阅 [事务](workflow-transactions.md) 和 <xref:System.Activities.Statements.TransactionScope> 。 有关事务的详细信息，请参阅 <xref:System.Transactions?displayProperty=nameWithType> 和 <xref:System.Transactions.Transaction?displayProperty=nameWithType> 。  
  
## <a name="using-compensableactivity"></a>使用 CompensableActivity  

 <xref:System.Activities.Statements.CompensableActivity> 是 [!INCLUDE[wf1](../../../includes/wf1-md.md)] 中的核心补偿活动。 将执行可能需要补偿的工作的所有活动放置到 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 的 <xref:System.Activities.Statements.CompensableActivity> 中。 在本示例中，将购买机票的预订步骤放置到 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 的 <xref:System.Activities.Statements.CompensableActivity> 中，并且将取消预订放置到 <xref:System.Activities.Statements.CompensableActivity.CompensationHandler%2A> 中。 紧接着在工作流中的 <xref:System.Activities.Statements.CompensableActivity> 之后是两个活动，即等待经理批准，然后完成购买机票的步骤。 如果在 <xref:System.Activities.Statements.CompensableActivity> 成功完成之后某个错误条件导致要取消工作流，则会安排 <xref:System.Activities.Statements.CompensableActivity.CompensationHandler%2A> 处理程序中的活动并且取消相应的航班。  
  
 [!code-csharp[CFX_CompensationExample#1](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#1)]  
  
 下面的示例是 XAML 形式的工作流。  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken" />  
    </CompensableActivity.Result>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
    <c:ReserveFlight />  
  </CompensableActivity>  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
</Sequence>  
```  
  
 在调用工作流时，将下面的输出显示到控制台。  
  
 **ReserveFlight：保留票证。**  
**ManagerApproval：接收了经理批准。** 
**PurchaseFlight：已购买票证。** 
**工作流已成功完成，状态为：已关闭。**
> [!NOTE]
> 本主题中的示例活动（例如 `ReserveFlight`）显示将其名称和用途显示到控制台，以帮助说明在发生补偿时执行这些活动的顺序。  
  
### <a name="default-workflow-compensation"></a>默认的工作流补偿  

 默认情况下，如果工作流被取消，则会对已成功完成以及尚未确认或补偿的任何可补偿的活动运行补偿逻辑。  
  
> [!NOTE]
> 确认后 <xref:System.Activities.Statements.CompensableActivity> ， *confirmed* 将无法再调用活动的补偿。 本节的后面部分介绍确认过程。  
  
 在本示例中，预订航班之后，但在经理批准步骤之前引发了一个异常。  
  
 [!code-csharp[CFX_CompensationExample#2](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#2)]  
  
 此示例是 XAML 形式的工作流。  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken" />  
    </CompensableActivity.Result>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
    <c:ReserveFlight />  
  </CompensableActivity>  
  <c:SimulatedErrorCondition />  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
</Sequence>  
```  
  
 [!code-csharp[CFX_CompensationExample#100](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#100)]  
  
 当调用工作流时，将由 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 中的主机应用程序处理模拟错误条件异常，工作流被取消并调用补偿逻辑。  
  
 **ReserveFlight：保留票证。**  
**SimulatedErrorCondition：引发 ApplicationException。** 
**工作流未处理异常：** 
**ApplicationException：工作流中的模拟错误条件。** 
**CancelFlight：票证已取消。** 
**工作流已成功完成，状态为：已取消。**

### <a name="cancellation-and-compensableactivity"></a>取消和 CompensableActivity  

 如果 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 的 <xref:System.Activities.Statements.CompensableActivity> 中的活动未完成并且取消了该活动，则执行 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> 中的活动。  
  
> [!NOTE]
> 如果 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> 的 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 中的活动未完成并且取消了该活动，则只调用 <xref:System.Activities.Statements.CompensableActivity>。 如果 <xref:System.Activities.Statements.CompensableActivity.CompensationHandler%2A> 的 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 中的活动已成功完成并且随后对该活动调用了补偿，则只执行 <xref:System.Activities.Statements.CompensableActivity>。  
  
 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> 为工作流作者创造了机会来提供任何适当的取消逻辑。 在下面的示例中，在执行 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 期间引发异常，然后调用 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A>。  
  
```csharp  
Activity wf = new Sequence()  
{  
    Activities =  
    {  
        new CompensableActivity  
        {  
            Body = new Sequence  
            {  
                Activities =
                {  
                    new ChargeCreditCard(),  
                    new SimulatedErrorCondition(),  
                    new ReserveFlight()  
                }  
            },  
            CompensationHandler = new CancelFlight(),  
            CancellationHandler = new CancelCreditCard()  
        },  
        new ManagerApproval(),  
        new PurchaseFlight()  
    }  
};  
```  
  
 此示例是 XAML 形式的工作流  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken" />  
    </CompensableActivity.Result>  
    <Sequence>  
      <c:ChargeCreditCard />  
      <c:SimulatedErrorCondition />  
      <c:ReserveFlight />  
    </Sequence>  
    <CompensableActivity.CancellationHandler>  
      <c:CancelCreditCard />  
    </CompensableActivity.CancellationHandler>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
  </CompensableActivity>  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
</Sequence>  
```  
  
 当调用工作流时，将由 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 中的宿主应用程序处理模拟错误条件异常，工作流被取消并调用 <xref:System.Activities.Statements.CompensableActivity> 的取消逻辑。 在此示例中，补偿逻辑和取消逻辑具有不同的目的。 如果成功完成了 <xref:System.Activities.Statements.CompensableActivity.Body%2A>，则意味着对这张信用卡已收取费用并且航班已预订，因此补偿应撤消这两个步骤。 在此示例中 (，取消航班会自动取消信用卡收费。 ) 不过，如果 <xref:System.Activities.Statements.CompensableActivity> 取消，这意味着未 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 完成，因此的逻辑 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> 需要能够确定如何最好地处理取消。 在此示例中，<xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> 取消信用卡收费，但由于 `ReserveFlight` 是 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 中的最后一个活动，因此它不会尝试取消航班。 因为 `ReserveFlight` 是 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 中的最后一个活动，所以，如果它已经成功完成，则 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 应该已完成并可能没有取消。  
  
 **ChargeCreditCard：对信用卡收取航班费用。**  
**SimulatedErrorCondition：引发 ApplicationException。** 
**工作流未处理异常：** 
**ApplicationException：工作流中的模拟错误条件。** 
**CancelCreditCard：取消信用卡收费。** 
**工作流已成功完成，状态为：已取消。**  有关取消的详细信息，请参阅 [取消](modeling-cancellation-behavior-in-workflows.md)。  
  
### <a name="explicit-compensation-using-the-compensate-activity"></a>使用 Compensate 活动的显式补偿  

 在上一节中，我们介绍了隐式补偿。 隐式补偿可能适合于简单方案，但如果在安排补偿处理时需要更多显式控件，则可以使用 <xref:System.Activities.Statements.Compensate> 活动。 若要使用 <xref:System.Activities.Statements.Compensate> 活动启动补偿过程，请使用需要补偿的 <xref:System.Activities.Statements.CompensationToken> 的 <xref:System.Activities.Statements.CompensableActivity>。 可以使用 <xref:System.Activities.Statements.Compensate> 活动在尚未确认或补偿的任何已完成的 <xref:System.Activities.Statements.CompensableActivity> 上启动补偿。 例如，可以在 <xref:System.Activities.Statements.Compensate> 活动的 <xref:System.Activities.Statements.TryCatch.Catches%2A> 部分中或完成 <xref:System.Activities.Statements.TryCatch> 之后的任何时间使用 <xref:System.Activities.Statements.CompensableActivity> 活动。 在本示例中，在 <xref:System.Activities.Statements.Compensate> 活动的 <xref:System.Activities.Statements.TryCatch.Catches%2A> 部分中使用了 <xref:System.Activities.Statements.TryCatch> 活动以反转 <xref:System.Activities.Statements.CompensableActivity> 的操作。  
  
 [!code-csharp[CFX_CompensationExample#3](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#3)]  
  
 此示例是 XAML 形式的工作流。  
  
```xaml  
<TryCatch  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:s="clr-namespace:System;assembly=mscorlib"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <TryCatch.Variables>  
    <Variable  
       x:TypeArguments="CompensationToken"  
       x:Name="__ReferenceID0"  
       Name="token1" />  
  </TryCatch.Variables>  
  <TryCatch.Try>  
    <Sequence>  
      <CompensableActivity>  
        <CompensableActivity.Result>  
          <OutArgument  
             x:TypeArguments="CompensationToken">  
            <VariableReference  
               x:TypeArguments="CompensationToken"  
               Variable="{x:Reference __ReferenceID0}">  
              <VariableReference.Result>  
                <OutArgument  
                   x:TypeArguments="Location(CompensationToken)" />  
              </VariableReference.Result>  
            </VariableReference>  
          </OutArgument>  
        </CompensableActivity.Result>  
        <CompensableActivity.CompensationHandler>  
          <c:CancelFlight />  
        </CompensableActivity.CompensationHandler>  
        <CompensableActivity.ConfirmationHandler>  
          <c:ConfirmFlight />  
        </CompensableActivity.ConfirmationHandler>  
        <c:ReserveFlight />  
      </CompensableActivity>  
      <c:SimulatedErrorCondition />  
      <c:ManagerApproval />  
      <c:PurchaseFlight />  
    </Sequence>  
  </TryCatch.Try>  
  <TryCatch.Catches>  
    <Catch  
       x:TypeArguments="s:ApplicationException">  
      <ActivityAction  
         x:TypeArguments="s:ApplicationException">  
        <Compensate>  
          <Compensate.Target>  
            <InArgument  
               x:TypeArguments="CompensationToken">  
              <VariableValue  
                 x:TypeArguments="CompensationToken"  
                 Variable="{x:Reference __ReferenceID0}">  
                <VariableValue.Result>  
                  <OutArgument  
                     x:TypeArguments="CompensationToken" />  
                </VariableValue.Result>  
              </VariableValue>  
            </InArgument>  
          </Compensate.Target>  
        </Compensate>  
      </ActivityAction>  
    </Catch>  
  </TryCatch.Catches>  
</TryCatch>  
```  
  
 在调用工作流时，将下面的输出显示到控制台。  
  
 **ReserveFlight：保留票证。**  
**SimulatedErrorCondition：引发 ApplicationException。** 
**CancelFlight：票证已取消。** 
**工作流已成功完成，状态为：已关闭。**

### <a name="confirming-compensation"></a>确认补偿  

 默认情况下，可以在完成活动之后的任何时间对可补偿的活动进行补偿。 但是，在某些情况下，这可能不太合适。 在上一个示例中，对预订机票的补偿是取消预订。 但是，在航程完成之后，此补偿步骤将不再有效。 确认可补偿的活动会调用由 <xref:System.Activities.Statements.CompensableActivity.ConfirmationHandler%2A> 指定的活动。 该操作的一个可能用处是允许释放对于执行补偿非常必要的所有资源。 在确认可补偿的活动之后，不能再对该活动进行补偿，并且如果尝试该操作，则会引发一个 <xref:System.InvalidOperationException> 异常。 工作流成功完成之后，会按相反的完成顺序确认成功完成的所有未确认以及未补偿的可补偿活动。 在本示例中，预订航班、购买机票并完成，然后确认可补偿的活动。 若要确认某个 <xref:System.Activities.Statements.CompensableActivity>，请使用 <xref:System.Activities.Statements.Confirm> 活动并指定要确认的 <xref:System.Activities.Statements.CompensationToken> 的 <xref:System.Activities.Statements.CompensableActivity>。  
  
 [!code-csharp[CFX_CompensationExample#4](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_CompensationExample/cs/Program.cs#4)]  
  
 此示例是 XAML 形式的工作流。  
  
```xaml  
<Sequence  
   xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"  
   xmlns:c="clr-namespace:CompensationExample;assembly=CompensationExample"  
   xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">  
  <Sequence.Variables>  
    <x:Reference>__ReferenceID0</x:Reference>  
  </Sequence.Variables>  
  <CompensableActivity>  
    <CompensableActivity.Result>  
      <OutArgument  
         x:TypeArguments="CompensationToken">  
        <VariableReference  
           x:TypeArguments="CompensationToken">  
          <VariableReference.Result>  
            <OutArgument  
               x:TypeArguments="Location(CompensationToken)" />  
          </VariableReference.Result>  
          <VariableReference.Variable>  
            <Variable  
               x:TypeArguments="CompensationToken"  
               x:Name="__ReferenceID0"  
               Name="token1" />  
          </VariableReference.Variable>  
        </VariableReference>  
      </OutArgument>  
    </CompensableActivity.Result>  
    <CompensableActivity.CompensationHandler>  
      <c:CancelFlight />  
    </CompensableActivity.CompensationHandler>  
    <CompensableActivity.ConfirmationHandler>  
      <c:ConfirmFlight />  
    </CompensableActivity.ConfirmationHandler>  
    <c:ReserveFlight />  
  </CompensableActivity>  
  <c:ManagerApproval />  
  <c:PurchaseFlight />  
  <c:TakeFlight />  
  <Confirm>  
    <Confirm.Target>  
      <InArgument  
         x:TypeArguments="CompensationToken">  
        <VariableValue  
           x:TypeArguments="CompensationToken"  
           Variable="{x:Reference __ReferenceID0}">  
          <VariableValue.Result>  
            <OutArgument  
               x:TypeArguments="CompensationToken" />  
          </VariableValue.Result>  
        </VariableValue>  
      </InArgument>  
    </Confirm.Target>  
  </Confirm>  
</Sequence>  
```  
  
在调用工作流时，将下面的输出显示到控制台。  
  
**ReserveFlight：保留票证。**  
**ManagerApproval：接收了经理批准。** 
**PurchaseFlight：已购买票证。** 
**TakeFlight：航班已完成。** 
**ConfirmFlight：已采用航班，无需补偿。** 
**工作流已成功完成，状态为：已关闭。**

## <a name="nesting-compensation-activities"></a>嵌套补偿活动  

可以将 <xref:System.Activities.Statements.CompensableActivity> 放置到另一个 <xref:System.Activities.Statements.CompensableActivity.Body%2A> 的 <xref:System.Activities.Statements.CompensableActivity> 部分中。 <xref:System.Activities.Statements.CompensableActivity> 不能放置于另一个 <xref:System.Activities.Statements.CompensableActivity> 的处理程序中。 在父级完成取消、确认或补偿操作前，确保在 <xref:System.Activities.Statements.CompensableActivity> 父级取消、确认或补偿时，所有已成功完成的和尚未得到确认或补偿的子级可补偿活动必须得到确认和补偿是该父级的责任。 如果不对此进行显式建模，在该父级接收到取消或补偿信号时，<xref:System.Activities.Statements.CompensableActivity> 将隐式补偿可补偿的子级活动。 如果父级收到了确认信号，则该父级将隐式确认可补偿的子级活动。 如果用于处理取消、确认或补偿的逻辑在父 <xref:System.Activities.Statements.CompensableActivity> 的处理程序中被显式建模，则未显式处理的所有子级都将被隐式确定。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Statements.CompensableActivity>
- <xref:System.Activities.Statements.Compensate>
- <xref:System.Activities.Statements.Confirm>
- <xref:System.Activities.Statements.CompensationToken>
