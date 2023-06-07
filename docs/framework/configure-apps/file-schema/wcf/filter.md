---
title: <filter>
ms.date: 03/30/2017
ms.assetid: 3266700b-904b-44e4-93a7-e06a1a445100
ms.openlocfilehash: 6e78275aaeb202405e327302455d56fa431d7f27
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855258"
---
# \<filter>

定义一个路由筛选器，该筛选器确定 <xref:System.ServiceModel.Dispatcher.MessageFilter> 要在评估传入消息时使用的 Windows Communication Foundation （WCF）的类型，以及筛选器所需的任何支持数据或参数。

[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;[**\<routing>**](routing.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<filters>**](filters-of-routing.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<filter>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<routing>
  <filters>
    <filter customType="String"
            filterData="String"
            filterType="Action/Address/AddressPrefix/And/Custom/Endpoint/MatchAll/XPath"
            name="String" />
  </filters>
</routing>
```  
  
## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

| 属性  | 说明 |
| ---------- | ----------- |
| customType | 一个字符串，包含要用作筛选器的自定义类型的完全限定类型名称。 如果 `filterType` 设置为 `custom` ，则此特性包含要创建的类的完全限定类型名称。  `filterData`还可以包含在计算自定义类型筛选器的过程中要使用的值。 |
| filterData | 一个包含筛选器数据的字符串。 有关如何指定此特性的更多信息，请参见 <xref:System.ServiceModel.Routing.Configuration.FilterElement.FilterData%2A>。 |
| filterType | 一个包含筛选器类型的字符串。 此特性的类型为 <xref:System.ServiceModel.Routing.Configuration.FilterType>。  有关如何使用此 `filterData` 特性的更多信息，请参见 <xref:System.ServiceModel.Routing.Configuration.FilterElement.FilterData%2A>。 |
| NAME       | 一个字符串，包含此筛选器元素的唯一名称。 |

### <a name="child-elements"></a>子元素

无。

### <a name="parent-elements"></a>父元素

| 元素 | 描述 |
| ------- | ----------- |
| [\<routing>](routing.md) | 用于定义一组路由筛选器的配置节，这些筛选器确定 <xref:System.ServiceModel.Dispatcher.MessageFilter> 计算传入消息时使用的 Windows Communication Foundation （WCF）的类型。 |

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Routing.Configuration.FilterElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Routing.Configuration.FilterElement.FilterData%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.Routing.Configuration.FilterType?displayProperty=nameWithType>
