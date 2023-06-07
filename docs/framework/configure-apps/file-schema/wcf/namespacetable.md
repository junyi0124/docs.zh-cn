---
title: <namespaceTable>
ms.date: 03/30/2017
ms.assetid: 64801766-01b7-4c65-9ce6-70ad5af67689
ms.openlocfilehash: aefe7beec7335d80341e670961800907c2bd0200
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855096"
---
# \<namespaceTable>

表示用于定义一组元素的配置节，这些元素包含随后可在 XPath 筛选器中用于路由的前缀映射的命名空间。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<routing>**](routing.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<namespaceTable>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <routing>
    <namespaceTable>
      <add namespace="String"
           prefix="String" />
    </namespaceTable>
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
| [**\<filter>**](filter.md) | 定义用于 XPath 表达式的命名空间前缀映射。 |

### <a name="parent-elements"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<routing>**](routing.md) | 表示用于定义一组路由筛选器的配置节，这些筛选器确定计算传入消息时使用的 Windows Communication Foundation （WCF）的类型，以及用于 <xref:System.ServiceModel.Dispatcher.MessageFilter> 定义在筛选器匹配时要将消息发送到的目标终结点的路由表。 |

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Routing.Configuration.NamespaceElementCollection?displayProperty=nameWithType>
