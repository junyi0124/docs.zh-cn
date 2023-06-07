---
title: <add> 的 <filters>
ms.date: 03/30/2017
ms.assetid: e3bf437c-dd99-49f3-9792-9a8721e6eaad
ms.openlocfilehash: c1de0605bc8afc502a85d9b2917b975ee45a3d26
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201650"
---
# <a name="add-of-filters"></a>\<add> 的 \<filters>

一个 XPath 筛选器，用于指定要记录的消息的种类。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<diagnostics>**](diagnostics.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<messageLogging>**](messagelogging.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<filters>**](filters.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<filters>
  <add filter="String" />
</filters>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|filter|一个字符串，用于指定由 XPath 1.0 表达式定义的 XML 文档的查询。 有关详细信息，请参阅 <xref:System.ServiceModel.Dispatcher.XPathMessageFilter>。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<filters>](filters.md)|包含用于控制所记录的消息类型的 XPath 筛选器集合。|  
  
## <a name="remarks"></a>备注  

 如果将 `logMessagesAtTransportLevel` 指定为 `true`，筛选器将只应用于传输层。 筛选器不影响服务级别和格式不正确的消息日志记录。  
  
 若要向集合添加筛选器，请使用 `add` 关键字。 如果定义一个或多个筛选器，则仅记录与其中至少一个筛选器相匹配的消息。 如果未定义任何筛选器，则所有消息都可通过。  
  
 筛选器支持完整的 XPath 语法，并按照其在配置文件中出现的顺序进行应用。 存在语法错误的筛选器会导致配置异常。  
  
 下面的示例演示如何配置一个筛选器，它仅记录具有 SOAP 标头部分的消息。  
  
## <a name="example"></a>示例  

 下面的示例演示如何配置一个筛选器，它仅记录具有 SOAP 标头部分的消息。  
  
```xml  
<messageLogging logEntireMessage="true"
                logMalformedMessages="true"
                logMessagesAtServiceLevel="true"
                logMessagesAtTransportLevel="true"
                maxMessagesToLog="420">
  <filters>
    <add xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
      /soap:Envelope/soap:Headers
    </add>
  </filters>
</messageLogging>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.DiagnosticSection>
- <xref:System.ServiceModel.Diagnostics>
- <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement.Filters%2A>
- <xref:System.ServiceModel.Configuration.XPathMessageFilterElement>
- <xref:System.ServiceModel.Dispatcher.XPathMessageFilter>
- [配置消息日志记录](../../../wcf/diagnostics/configuring-message-logging.md)
- [\<messageLogging>](messagelogging.md)
