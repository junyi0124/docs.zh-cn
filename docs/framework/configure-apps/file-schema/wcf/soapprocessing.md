---
title: <soapProcessing>
ms.date: 03/30/2017
ms.assetid: e8707027-e6b8-4539-893d-3cd7c13fbc18
ms.openlocfilehash: 0728e22205d4ac2c7674f7690e142aed51d42440
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399544"
---
# \<soapProcessing>

定义用于封送不同绑定类型和消息版本之间消息的客户端终结点行为。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<soapProcessing>**
  
## <a name="syntax"></a>语法  
  
```xml  
<soapProcessing processMessages="true|false" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
  
下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|                   | 描述 |
| ----------------- | ----------- |
| `processMessages` | 一个布尔值，指定是否应封送 SOAP 消息版本之间的消息。 |

### <a name="child-elements"></a>子元素

无

### <a name="parent-elements"></a>父元素

|     | 说明 |
| --- | ----------- |
| [**\<behavior>**](behavior-of-endpointbehaviors.md) | 指定终结点行为。 |

## <a name="remarks"></a>注解

SOAP 处理是指在各消息版本之间转换消息的过程。

Windows Communication Foundation （WCF）路由服务可以将消息从一个协议转换为另一个协议。 如果传入消息和传出消息的版本不相同，则会创建一条具有正确版本的新消息。 通过构造新的 WCF 消息（该消息包含传入 WCF 消息的正文部分和相关标头）来完成对一个  到另一个  的消息的处理。 在构造新的 WCF 消息的过程中，不会使用特定于寻址的标头或在路由器级别理解的标头，这是因为这些标头要么具有不同的版本（如果为寻址标头），要么已作为客户端和路由器之间的通信的一部分处理。

是否将一个标头置于出站消息内由此标头在通过传入通道层时是否已标记为已理解来决定。 不会移除不理解的标头（如自定义标头），并且会将此标头复制到出站消息中，以通过路由服务。 系统会将消息正文复制到出站消息中， 然后将此消息外发到出站通道，此时将创建和添加特定于该通信协议/传输的所有标头和其他信封数据。

当指定了 SOAP 处理行为时，将执行此类处理步骤。 此 [\<soapProcessingExtension>](soapprocessing.md) 行为是在启动路由服务时应用于所有客户端（传出）终结点的终结点行为。 默认情况下，该 [\<routing>](routing-of-servicebehavior.md) 行为将 [\<soapProcessingExtension>](soapprocessing.md) `processMessages` `true` 为每个客户端终结点创建一个新行为，并将设置为。 如果您具有路由服务无法理解的协议，或者希望重写默认处理行为，则可以针对整个路由服务或仅针对特定终结点禁用 SOAP 处理。  若要对所有终结点上的整个路由服务禁用 SOAP 处理，请将 `soapProcessing` 行为的特性设置 [\<routing>](routing-of-servicebehavior.md) 为 `false` 。 若要针对特定终结点禁用 SOAP 处理，请使用此行为，并将其 `processMessages` 特性设置为 `false`，然后将此行为附加到不希望运行默认处理代码的终结点。  当 [\<routing>](routing-of-servicebehavior.md) 行为设置路由服务时，它将跳过重新应用终结点行为，因为已存在一个终结点行为。
