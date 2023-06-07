---
title: ServiceTimeoutsBehavior
ms.date: 03/30/2017
ms.assetid: 4412525d-a3cc-4eae-b3e8-a50ce766d09d
ms.openlocfilehash: 867219130fc853f3ba2c1c2f807b1651f6480f13
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273966"
---
# <a name="servicetimeoutsbehavior"></a>ServiceTimeoutsBehavior

ServiceTimeoutsBehavior  
  
## <a name="syntax"></a>语法  
  
```csharp
class ServiceTimeoutsBehavior : Behavior  
{  
  datetime TransactionTimeout;  
};  
```  
  
## <a name="methods"></a>方法  

 ServiceTimeoutsBehavior 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 ServiceTimeoutsBehavior 类具有以下属性：  
  
### <a name="transactiontimeout"></a>TransactionTimeout  

 数据类型：DateTime  
  
 访问类型：只读  
  
 事务必须在此期间完成的时间段。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Configuration.ServiceTimeoutsElement>
