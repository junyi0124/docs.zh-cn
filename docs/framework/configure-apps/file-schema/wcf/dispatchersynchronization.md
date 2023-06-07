---
title: <dispatcherSynchronization>
ms.date: 03/30/2017
ms.assetid: cc030f9c-4e38-4b14-94dc-9a0e41ec8e2d
ms.openlocfilehash: b95f25217c2a3558846cc7a0ef43e21aacd2ee2a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398000"
---
# \<dispatcherSynchronization>
  
指定允许服务进行异步答复的终结点行为。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<dispatcherSynchronization>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<dispatcherSynchronizationBehavior asynchronousSendEnabled="Boolean"
                                   maxPendingReceives="Integer" />
```  
  
## <a name="type"></a>类型  
  
`Type`  
  
## <a name="attributes-and-elements"></a>特性和元素  
  
下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性

| 属性               | 说明       |
| ----------------------- | ----------------- |
| asynchronousSendEnabled | 一个布尔值，指定是否启用异步发送行为。 |
| `maxPendingReceives`    | 一个整数，指定可在每个通道上执行的并发接收数。<br /><br /> 仅当已正确配置服务限制行为之后，才能配置此值。 |

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

| 元素 | 描述 |  
| ------- | ----------- |  
| [\<behavior>](behavior-of-endpointbehaviors.md)|指定终结点行为。 |

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Configuration.DispatcherSynchronizationElement>
- <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior>
