---
title: ActivityTransfer
ms.date: 03/30/2017
ms.assetid: fc40ef17-2a92-4ce2-853c-6ba8e5d571f3
ms.openlocfilehash: f1978881517fe5010ccc0f5192b21bd6688f063a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96291110"
---
# <a name="activitytransfer"></a>ActivityTransfer

活动传输事件  
  
## <a name="syntax"></a>语法  
  
```csharp
class ActivityTransfer : WSAT_TraceEvent  
{  
  object ActivityID;  
  object RelatedActivityID;  
};  
```  
  
## <a name="methods"></a>方法  

 ActivityTransfer 类不定义任何方法。  
  
## <a name="properties"></a>属性  

 ActivityTransfer 类具有以下属性：  
  
### <a name="activityid"></a>ActivityID  
  
- 数据类型：object  
    访问类型：只读  
  
- 活动 ID  
  
### <a name="relatedactivityid"></a>RelatedActivityID  
  
- 数据类型：object  
    访问类型：只读  
  
- 相关活动 ID  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义。|
