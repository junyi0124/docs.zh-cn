---
title: WF 中的活动创作选项
ms.date: 03/30/2017
ms.assetid: b9061f5f-12c3-47f0-adbe-1330e2714c94
ms.openlocfilehash: e80750b3865a21d072f45b0245ae114660012e66
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289199"
---
# <a name="activity-authoring-options-in-wf"></a>WF 中的活动创作选项

[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)]提供了若干用于创建自定义活动的选项。 用于创作给定活动的正确方法取决于所需的运行时功能。  
  
## <a name="deciding-which-base-activity-class-to-use-for-authoring-custom-activities"></a>确定哪个基本活动类用于创作自定义活动  

 下表列出了自定义活动基类中可用的功能。  
  
|基本活动类|可用的功能|  
|-------------------------|------------------------|  
|<xref:System.Activities.Activity>|将一组系统提供的活动和一组自定义活动组成一个复合活动。|  
|<xref:System.Activities.CodeActivity>|通过提供可以重写的 <xref:System.Activities.CodeActivity%601.Execute%2A> 方法实现命令性功能。 还提供对跟踪、变量以及参数的访问。|  
|<xref:System.Activities.NativeActivity>|提供 <xref:System.Activities.CodeActivity> 的所有功能，另外还可中止活动执行、取消子活动执行、使用书签以及计划活动、活动操作和功能。|  
|<xref:System.Activities.DynamicActivity>|提供一个类似于 DOM 的方法，使用该方法可以构造通过 <xref:System.ComponentModel.ICustomTypeDescriptor> 与 WF 设计器和运行时系统交互的活动，从而允许在不定义新类型的情况下创建新活动。|  
  
## <a name="authoring-activities-using-activity"></a>使用 Activity 创作活动  

 从 <xref:System.Activities.Activity> 派生的活动通过组合其他现有活动来构成功能。 这些活动可以是现有的自定义活动，也可以是来自 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)]活动库的活动。 组合这些活动是创建自定义功能的最基本方法。 使用可视化设计环境创作工作流时这种方法最常用。  
  
## <a name="authoring-activities-using-codeactivity-or-asynccodeactivity"></a>使用 CodeActivity 或 AsyncCodeActivity 创作活动  

 从 <xref:System.Activities.CodeActivity> 或 <xref:System.Activities.AsyncCodeActivity> 派生的活动可以通过用自定义的命令性代码重写 <xref:System.Activities.CodeActivity%601.Execute%2A> 方法来实现命令性功能。 活动由运行时执行时将执行该自定义代码。 尽管使用这种方法创建的活动可以访问自定义功能，但是它们无法访问运行时的所有功能，如对执行环境的完全访问、安排子活动、书签的功能或对 Cancel 或 Abort 方法的支持。 执行 <xref:System.Activities.CodeActivity> 时，它可以访问简化版本的执行环境（通过 <xref:System.Activities.CodeActivityContext> 或 <xref:System.Activities.AsyncCodeActivityContext> 类）。 使用 <xref:System.Activities.CodeActivity> 创建的活动可以访问参数和变量解析、扩展以及跟踪。 可以使用 <xref:System.Activities.AsyncCodeActivity> 进行异步活动计划。  
  
## <a name="authoring-activities-using-nativeactivity"></a>使用 NativeActivity 创作活动  

 从 <xref:System.Activities.NativeActivity> 派生的活动，与从 <xref:System.Activities.CodeActivity> 派生的活动一样，可通过重写 <xref:System.Activities.NativeActivity.Execute%2A> 来创建命令性功能，除此之外还可以通过传递给 <xref:System.Activities.NativeActivityContext> 方法的 <xref:System.Activities.NativeActivity.Execute%2A> 访问工作流运行时的所有功能。 此上下文支持多种功能，包括安排和取消子活动、执行 <xref:System.Activities.ActivityAction> 和 <xref:System.Activities.ActivityFunc%601> 对象、将事务流入工作流、调用异步进程、取消和中止执行、访问执行属性和扩展以及书签（恢复暂停工作流的处理程序）。  
  
## <a name="authoring-activities-using-dynamicactivity"></a>使用 DynamicActivity 创作活动  

 与其他三种活动类型不同，不会通过从 <xref:System.Activities.DynamicActivity>（该类是密封的）派生新类型来创建新功能，而是通过使用活动文档对象模型 (DOM) 将功能组合到 <xref:System.Activities.DynamicActivity.Properties%2A> 和 <xref:System.Activities.DynamicActivity.Implementation%2A> 属性中。  
  
## <a name="authoring-activities-that-return-a-result"></a>创作返回结果的活动  

 很多活动必须在其执行之后返回结果。 尽管可以在活动上始终定义一个自定义 <xref:System.Activities.OutArgument%601> 来实现此目的，但是建议改用 <xref:System.Activities.Activity%601> 或者从 <xref:System.Activities.CodeActivity%601> 或 <xref:System.Activities.NativeActivity%601> 派生。 这些基类中的每个类都具有一个名为 Result 的 <xref:System.Activities.OutArgument%601>，您的活动可以使用它作为其返回值。 只有在一个结果需要从某一活动返回时，才应使用返回某一结果的活动；如果需要返回多个结果，则应改用单独的 <xref:System.Activities.OutArgument%601> 成员。
