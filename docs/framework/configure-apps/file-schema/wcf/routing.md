---
title: <routing>
ms.date: 03/30/2017
ms.assetid: a210c209-3940-4288-9a8e-39b1e62606bc
ms.openlocfilehash: fcf2d4eec93fd7127c6f800e1c739ad1fac49203
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399962"
---
# \<routing>

表示用于定义一组路由筛选器的配置节，这些筛选器确定计算传入消息时使用的 Windows Communication Foundation （WCF）的类型，以及用于 <xref:System.ServiceModel.Dispatcher.MessageFilter> 定义在筛选器匹配时要将消息发送到的目标终结点的路由表。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<routing>**
  
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
    <routingTables>
      <table name="String">
        <entries>
          <add endpoint="String"
               filterName="String"
               priority="Integer" />
        </entries>
      </table>
    </routingTables>
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
| [**\<filters>**](filters-of-routing.md) | 包含一组路由筛选器，用于确定在计算传入消息时将使用的 Windows Communication Foundation （WCF） MessageFilter 的类型。 |
| [**\<filterTables>**](filtertables.md) | 包含路由筛选器和目标终结点之间的映射，以便指定在筛选器匹配时使用的终结点。 |

### <a name="parent-elements"></a>父元素

|     | 说明 |
| --- | ----------- |
| **\<system.ServiceModel>** | 所有 WCF 配置元素的根元素。 |

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Routing.Configuration.RoutingSection?displayProperty=nameWithType>
