---
title: 流程图工作流
description: 本文介绍流程图活动，该活动通常用于实现 Workflow Foundation 中的非顺序工作流。
ms.date: 03/30/2017
ms.assetid: b0a3475c-d22f-49eb-8912-973c960aebf5
ms.openlocfilehash: ce0661653a1d50b3f7264246b810faabbd12bf5f
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83419910"
---
# <a name="flowchart-workflows"></a>流程图工作流

流程图是用于设计程序的已知范例。 Flowchart 活动通常用于实现非顺序工作流，但如果未使用 `FlowDecision` 节点，则也可以用于顺序工作流。

## <a name="flowchart-workflow-structure"></a>流程图工作流结构

 Flowchart 活动是一种活动，该活动包含要执行的活动集合。  流程图还包含流控制元素，如 <xref:System.Activities.Statements.FlowDecision> 和 <xref:System.Activities.Statements.FlowSwitch%601>，这些元素基于变量的值指示所包含活动之间的执行。

## <a name="types-of-flow-nodes"></a>流节点类型

 根据执行元素时所需的流控制类型，使用不同的元素类型。 流程图元素的类型包括：

- `FlowStep` - 在流程图中建立一个执行步骤的模型。

- `FlowDecision` - 基于布尔条件建立执行分支，类似于 <xref:System.Activities.Statements.If>。

- `FlowSwitch` – 基于独占 Switch 建立执行分支，类似于 <xref:System.Activities.Statements.Switch%601>。

每个链接都具有一个 `Action` 属性，该属性定义可用于执行子活动的 <xref:System.Activities.ActivityAction>，并且每个链接还具有一个或多个 `Next` 属性，这些属性定义当前元素完成执行后要执行的元素。

### <a name="creating-a-basic-activity-sequence-with-a-flowstep-node"></a>使用 FlowStep 节点创建基本活动序列

若要建立一个基本的顺序模型，在该模型中依次执行两个活动，请使用 `FlowStep` 元素。 在下面的示例中，使用了两个 `FlowStep` 元素按顺序执行两个活动。

```xml
<Flowchart>
  <FlowStep>
    <Assign DisplayName="Get Name">
      <Assign.To>
        <OutArgument x:TypeArguments="x:String">[result]</OutArgument>
      </Assign.To>
      <Assign.Value>
        <InArgument x:TypeArguments="x:String">["User"]</InArgument>
      </Assign.Value>
    </Assign>
    <FlowStep.Next>
      <FlowStep>
        <WriteLine Text="Hello, " & [result]/>
      </FlowStep>
    </FlowStep.Next>
  </FlowStep>
</Flowchart>
```

### <a name="creating-a-conditional-flowchart-with-a-flowdecision-node"></a>使用 FlowDecision 节点创建条件流程图

若要在流程图工作流中建立条件流节点的模型（即，创建一个充当传统流程图的决策符号的链接），请使用 <xref:System.Activities.Statements.FlowDecision> 节点。 该节点的 <xref:System.Activities.Statements.FlowDecision.Condition%2A> 属性设置为定义条件的表达式，并且 <xref:System.Activities.Statements.FlowDecision.True%2A> 和 <xref:System.Activities.Statements.FlowDecision.False%2A> 属性设置为表达式计算结果为 <xref:System.Activities.Statements.FlowNode> 或 `true` 时要执行的 `false` 实例。 下面的示例演示如何定义使用 <xref:System.Activities.Statements.FlowDecision> 节点的工作流。

```xml
<Flowchart>
  <FlowStep>
    <Read Result="[s]"/>
    <FlowStep.Next>
      <FlowDecision>
        <IsEmpty Input="[s]" />
        <FlowDecision.True>
          <FlowStep>
            <Write Text="Empty"/>
          </FlowStep>
        </FlowDecision.True>
        <FlowDecision.False>
          <FlowStep>
            <Write Text="Non-Empty"/>
          </FlowStep>
        </FlowDecision.False>
      </FlowDecision>
    </FlowStep.Next>
  </FlowStep>
</Flowchart>
```

### <a name="creating-an-exclusive-switch-with-a-flowswitch-node"></a>使用 FlowSwitch 节点创建独占开关

若要建立某个流程图的模型，在该流程图中基于匹配的值选择了一个独占路径，请使用 <xref:System.Activities.Statements.FlowSwitch%601> 节点。 <xref:System.Activities.Statements.FlowSwitch%601.Expression%2A> 属性设置为 <xref:System.Activities.Activity%601>，其中类型参数 <xref:System.Object> 定义匹配选择的值。 <xref:System.Activities.Statements.FlowSwitch%601.Cases%2A> 属性定义要匹配条件表达式的键和 <xref:System.Activities.Statements.FlowNode> 对象的字典，以及一组 <xref:System.Activities.Statements.FlowNode> 对象，这些对象定义在给定情况与条件表达式匹配时的执行流方式。 <xref:System.Activities.Statements.FlowSwitch%601> 还定义一个 <xref:System.Activities.Statements.FlowSwitch%601.Default%2A> 属性，该属性定义没有情况与条件表达式匹配时的执行流方式。 下面的示例演示如何定义使用 <xref:System.Activities.Statements.FlowSwitch%601> 元素的工作流。

```xml
<Flowchart>
  <FlowSwitch>
    <FlowStep x:Key="Red">
      <WriteRed/>
    </FlowStep>
    <FlowStep x:Key="Blue">
      <WriteBlue/>
    </FlowStep>
    <FlowStep x:Key="Green">
      <WriteGreen/>
    </FlowStep>
  </FlowSwitch>
</Flowchart>
```
