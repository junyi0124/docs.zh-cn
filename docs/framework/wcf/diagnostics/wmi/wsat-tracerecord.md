---
title: WSAT_TraceRecord
ms.date: 03/30/2017
ms.assetid: 99bc7f66-1335-40d8-aa68-e754d569dc0d
ms.openlocfilehash: 0409277821a7cca3f97fcec1bb383aba9583a1f6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262211"
---
# <a name="wsat_tracerecord"></a>WSAT_TraceRecord

WSAT_TraceRecord  
  
## <a name="syntax"></a>语法  
  
```csharp
class WSAT_TraceRecord : WSAT_TraceEvent  
{  
  object ActivityID;  
  sint32 EventID;  
  string TraceRecord;  
};  
```  
  
## <a name="methods"></a>方法  

 WSAT_TraceRecord 类未定义任何方法。  
  
## <a name="properties"></a>属性  

 WSAT_TraceRecord 类具有以下属性：  
  
### <a name="activityid"></a>ActivityID  

 数据类型：object  
访问类型：只读  
  
 跟踪记录的活动 ID。  
  
### <a name="eventid"></a>EventID  

 数据类型：sint32  
访问类型：只读  
  
 跟踪记录的事件 ID。  
  
### <a name="tracerecord"></a>TraceRecord  

 数据类型：字符串  
访问类型：只读  
  
 跟踪记录  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|
