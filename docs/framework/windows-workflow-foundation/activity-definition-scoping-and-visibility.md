---
title: 活动定义范围和可见性
ms.date: 03/30/2017
ms.assetid: ccdffa07-9503-4eea-a61b-17f1564368b7
ms.openlocfilehash: c7f656fee4960a8c43ac58abafba400ed9b35bc1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289160"
---
# <a name="activity-definition-scoping-and-visibility"></a>活动定义范围和可见性

正如对象的范围和可见性一样，活动定义范围和可见性是其他对象或活动访问此活动成员的能力。 通过以下实现来执行活动定义：  
  
1. 确定活动可向其用户公开的成员（<xref:System.Activities.Argument>、<xref:System.Activities.Variable> 和 <xref:System.Activities.ActivityDelegate> 对象以及子活动）。  
  
2. 实现活动的执行逻辑  
  
 该实现可能涉及未向活动的使用者公开但却作为该实现的详细信息的成员。  与类型定义类似，作者利用活动模型可以限定与要定义的活动定义有关的活动成员的可见性。  此可见性控制成员使用的各个方面，如数据范围。  
  
## <a name="scope"></a>范围  

 除了数据范围之外，活动模型可见性还可限制对活动的其他方面的访问，例如验证、调试、跟踪或追踪。 执行属性使用可见性和范围，将执行特征约束到特定的定义范围。 辅助根使用可见性和范围，将 <xref:System.Activities.Statements.CompensableActivity> 捕获的状态约束到使用补偿活动的定义范围。  
  
## <a name="definition-and-usage"></a>定义和用法  

 通过继承基活动类，并通过使用 [内置活动库](net-framework-4-5-built-in-activity-library.md)中的活动，编写工作流。 若要使用某个活动，该活动的作者必须配置其定义的每个组件的可见性。  
  
### <a name="activity-members"></a>活动成员  

 活动模型定义活动提供给使用者的自变量、变量、委托和子活动。 其中每个成员均可声明为 `public` 或 `private`。 公共成员由活动的使用者进行配置，而`private`成员使用活动的作者确定的实现。 数据范围的可见性规则如下：  
  
1. 公共成员和公共子活动的公共成员可引用公共变量。  
  
2. 私有成员和公共子活动的公共成员可引用参数和私有变量。  
  
 绝对不要将可由活动的使用者设置的成员设置为私有成员。  
  
### <a name="authoring-models"></a>创作模型  

 通过使用 <xref:System.Activities.NativeActivity>、<xref:System.Activities.Activity>、<xref:System.Activities.CodeActivity> 或 <xref:System.Activities.AsyncCodeActivity> 定义自定义活动。 派生自这些类的活动可公开具有不同可见性的不同成员类型。  
  
#### <a name="nativeactivity"></a>NativeActivity  

 派生自 <xref:System.Activities.NativeActivity> 的活动具有使用命令性代码编写的行为，而且还可选择使用现有活动定义这些活动。 从 <xref:System.Activities.NativeActivity> 派生活动可授予对运行时公开的所有功能的访问权限。 此类活动的任何成员都可通过使用公共或私有可见性（只能声明为 `public` 的自变量除外）进行定义。  
  
 使用传递给 <xref:System.Activities.NativeActivity> 方法的 <xref:System.Activities.NativeActivityMetadata> 结构，将派生自 <xref:System.Activities.NativeActivity.CacheMetadata%2A> 的类的成员声明为运行时。  
  
#### <a name="activity"></a>活动  

 使用 <xref:System.Activities.Activity> 创建的活动具有通过撰写其他活动而严格设计的行为。 <xref:System.Activities.Activity> 类具有一个实现子活动，该活动由运行时通过使用 <xref:System.Activities.Activity.Implementation%2A> 获取。 派生自 <xref:System.Activities.Activity> 的活动可定义公共自变量、公共变量、导入的 ActivityDelegates 以及导入的 Activities。  
  
 导入的 ActivityDelegates 和 Activities 声明为活动的公共子活动，但不能由活动直接计划。 验证期间使用此信息，可避免在从不执行活动的位置运行面向父活动的验证。 另外，正如公共子活动一样，导入的子活动也可由活动的实现进行引用和计划。 这意味着，用来导入 Activity1 的活动可将 <xref:System.Activities.Statements.Sequence> 包含在其计划 Activity1 的实现中。  
  
#### <a name="codeactivity-asynccodeactivity"></a>CodeActivity/ AsyncCodeActivity  

 此基类用于使用命令性代码创作行为。 派生自此类的活动仅具有对其公开的自变量的访问权限。 这意味着仅这些活动能够公开的成员为公共自变量。 任何其他成员或可见性都不适用于这些活动。  
  
#### <a name="summary-of-visibilities"></a>可见性摘要  

 下表汇总了本节前面的信息。  
  
|成员类型|NativeActivity|活动|CodeActivity/ AsyncCodeActivity|  
|-----------------|--------------------|--------------|--------------------------------------|  
|参数|公共/私有|公共|不适用|  
|变量|公共/私有|公共|不适用|  
|子活动|公共/私有|公共，在实现中定义的一个固定的私有子活动。|不适用|  
|ActivityDelegates|公共/私有|公共|不适用|  
  
 一般来说，活动的使用者无法设置的成员不应成为公共成员。  
  
### <a name="execution-properties"></a>执行属性  

 在有些方案中，将特定执行属性的范围限定为某活动的公共子活动很有用。 <xref:System.Activities.ExecutionProperties> 集合使用 <xref:System.Activities.ExecutionProperties.Add%2A> 方法提供此功能。 此方法具有一个 Boolean 参数，该参数指示是将特定属性的范围限定为所有子活动，还是只限定为公共活动。 如果此参数设置为 `true`，则属性仅对公共成员及其公共子活动的公共成员可见。  
  
### <a name="secondary-roots"></a>辅助根  

 辅助根是运行时的内部机制，可用于管理补偿活动的状态。 当 <xref:System.Activities.Statements.CompensableActivity> 完成运行时，不会立即清除其状态。 而是由运行时将此状态维护在辅助根中，直到补偿段完成。 使用辅助根捕获的位置环境与使用补偿活动的定义的范围相对应。
