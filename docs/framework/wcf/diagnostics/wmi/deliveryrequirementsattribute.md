---
title: DeliveryRequirementsAttribute
ms.date: 03/30/2017
ms.assetid: 40c5435c-a325-4cf8-9dd0-d6e24b4a56a3
ms.openlocfilehash: a70a4ba40b569acc7893b21d796194224dc4ee78
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96274044"
---
# <a name="deliveryrequirementsattribute"></a>DeliveryRequirementsAttribute

DeliveryRequirementsAttribute  
  
## <a name="syntax"></a>语法  
  
```csharp
class DeliveryRequirementsAttribute : Behavior  
{  
  string QueuedDeliveryRequirements;  
  boolean RequireOrderedDelivery;  
  string TargetContract;  
};  
```  
  
## <a name="methods"></a>方法  

 DeliveryRequirementsAttribute 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 DeliveryRequirementsAttribute 类具有以下属性：  
  
### <a name="queueddeliveryrequirements"></a>QueuedDeliveryRequirements  

 数据类型：字符串  
  
 访问类型：只读  
  
 指定服务的绑定是否支持协定。  
  
### <a name="requireordereddelivery"></a>RequireOrderedDelivery  

 数据类型：Boolean  
  
 访问类型：只读  
  
 指定绑定是否支持已排序消息。  
  
### <a name="targetcontract"></a>TargetContract  

 数据类型：字符串  
  
 访问类型：只读  
  
 该类应用到的协定。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.DeliveryRequirementsAttribute>
