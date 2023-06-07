---
title: <filters> 的 <routing>
ms.date: 03/30/2017
ms.assetid: 7993cf90-9afd-4c3c-9608-184d5da1105c
ms.openlocfilehash: c9bc3a2c379e14d8cf687676a3ec40702d150e1e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855247"
---
# <a name="filters-of-routing"></a>\<filters> 的 \<routing>

表示用于定义一组路由筛选器的配置节，这些筛选器确定 <xref:System.ServiceModel.Dispatcher.MessageFilter> 计算传入消息时使用的 Windows Communication Foundation （WCF）的类型。

[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;[**\<routing>**](routing.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<filters>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <routing>
    <filters>
      <filter customType="String"
              filterData="String"
              filterType="Action/Address/AddressPrefix/And/Custom/Endpoint/MatchAll/XPath"
              name="String" />
    </filters>
  </routing>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

无

### <a name="child-elements"></a>子元素

|     | 说明 |
| --- | ----------- |
| [**\<filter>**](filter.md) | 包含一个路由筛选器，该筛选器确定 <xref:System.ServiceModel.Dispatcher.MessageFilter> 计算传入消息时将使用的 Windows Communication Foundation （WCF）的类型。 |

### <a name="parent-elements"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<routing>**](routing.md) | 表示用于定义一组路由筛选器的配置节，这些筛选器确定计算传入消息时使用的 Windows Communication Foundation （WCF）的类型，以及用于 <xref:System.ServiceModel.Dispatcher.MessageFilter> 定义在筛选器匹配时要将消息发送到的目标终结点的路由表。 |

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Routing.Configuration.FilterElement?displayProperty=nameWithType>
