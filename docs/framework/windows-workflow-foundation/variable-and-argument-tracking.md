---
title: 变量和自变量跟踪
ms.date: 03/30/2017
ms.assetid: 8f3d9d30-d899-49aa-b7ce-a8d0d32c4ff0
ms.openlocfilehash: 85cecbfaf1db224152d4582325326f1f80e08266
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242898"
---
# <a name="variable-and-argument-tracking"></a>变量和自变量跟踪

当跟踪工作流的执行时，提取数据往往很有用。 这在访问跟踪记录后续执行时可提供其他上下文。 在 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 中，你可以使用跟踪在工作流的任意活动范围内提取所有可见变量或自变量。 跟踪配置文件简化了对数据的提取。  
  
## <a name="variables-and-arguments"></a>变量和自变量  

 变量和参数在活动发出 ActivityStateRecord 时提取。  仅当变量位于活动范围中时，才能提取该变量。 按如下方式指定将在活动中提取的变量：  
  
- 如果变量用变量名称指定，跟踪则在正跟踪的当前活动和父活动中查找变量。 系统将在当前活动范围和父范围中搜索变量。  
  
- 如果要提取的变量使用 name = " \* " 指定，则提取正在跟踪的当前活动中的所有变量。 此种情况下，将不会提取范围中在父活动中定义的变量。  
  
 当提取自变量时，提取的自变量依赖于活动状态。 当活动状态为 Executing 时，只能提取 `InArguments`。 对于任何其他活动状态（Closed、Faulted、Canceled），则可以提取所有自变量，即 InArguments 和 OutArguments。  
  
 下面的示例演示用于在发出活动的 `Closed` 跟踪记录时提取变量和自变量的活动状态查询。 变量和自变量只能使用 <xref:System.Activities.Tracking.ActivityStateRecord> 来提取，因此使用 <xref:System.Activities.Tracking.ActivityStateQuery> 在跟踪配置文件内进行订阅。  
  
```xml  
<activityStateQuery activityName="SendEmailActivity">  
  <states>  
    <state name="Closed"/>  
  </states>  
  <variables>  
    <variable name="FromAddress"/>  
  </variables>  
  <arguments>  
    <argument name="Result"/>  
  </arguments>  
</activityStateQuery>  
```  
  
## <a name="protecting-information-stored-within-variables-and-arguments"></a>保护存储在变量和参数中的信息  

 默认情况下，跟踪变量或自变量对 WF 运行时可见。 工作流开发人员可以采取以下步骤，阻止访问这些变量或参数：  
  
1. 对变量的值进行加密。  
  
2. 控制跟踪配置文件的创作，以阻止提取变量或参数。  
  
3. 对于自定义跟踪参与者，请确保 WF 代码不会泄露存储在变量或参数中的敏感信息。  
  
## <a name="see-also"></a>另请参阅

- [Windows Server App Fabric 监视](/previous-versions/appfabric/ee677251(v=azure.10))
- [用 App Fabric 监视应用程序](/previous-versions/appfabric/ee677276(v=azure.10))
