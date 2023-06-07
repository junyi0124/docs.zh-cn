---
title: <messageLogging>
ms.date: 03/30/2017
ms.assetid: 1d06a7e6-9633-4a12-8c5d-123adbbc19c5
ms.openlocfilehash: fd4d678b1e861a47762d8a64f85dcc052a30fe2b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204796"
---
# \<messageLogging>

该元素定义 Windows Communication Foundation (WCF) 的消息日志记录功能的设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<diagnostics>**](diagnostics.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<messageLogging>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <diagnostics>
    <messageLogging logEntireMessage="Boolean"
                    logMalformedMessages="Boolean"
                    logMessagesAtServiceLevel="Boolean"
                    logMessagesAtTransportLevel="Boolean"
                    maxMessagesToLog="Integer"
                    maxSizeOfMessageToLog="Integer">
      <filters>
        <clear />
      </filters>
    </messageLogging>
  </diagnostics>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`logEntireMessage`|一个布尔值，指定是否记录整个消息（消息头和正文）。 默认值为 `false`，这意味着仅记录消息头。 此设置会影响所有消息日志记录级别（服务、传输和格式不正确）。|  
|`logMalformedMessages`|一个布尔值，指定是否记录格式不正确的消息。 格式不正确的消息将不计入 `maxMessagesToLog`。 默认为 `false`。|  
|`logMessagesAtServiceLevel`|一个布尔值，指定是否在服务级别跟踪消息（在与加密和传输有关的转换之前）。 默认为 `false`。|  
|`logMessagesAtTransportLevel`|一个布尔值，指定是否在传输级别跟踪消息。 配置文件中指定的所有筛选器都会应用，但仅跟踪与这些筛选器相匹配的消息。 默认为 `false`。|  
|`maxMessagesToLog`|一个正整数，指定要记录的最大消息数。 默认值为 1000。|  
|`maxSizeOfMessageToLog`|一个正整数，指定要记录的最大消息大小（字节）。 大小超出限制的消息将不会被记录。 此设置会影响所有跟踪级别。 默认值为 262144 (0x4000)。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|筛选器|`filters` 元素包含 XPath 筛选器集合。 启用传输消息日志记录后（`logMessagesAtTransportLevel` 为 `true`），只有与筛选器匹配的消息才会记录下来。<br /><br /> 筛选器仅应用于传输层。 筛选器不影响服务级别和格式不正确的消息日志记录。<br /><br /> 此元素唯一的属性为 `filter` XpathFilter。<br /><br /> `<filters>     <add xmlns:soap="http://www.w3.org/2003/05/soap-envelope">/soap:Envelope</add> </filters>`|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|诊断|定义管理员运行时检查和控制的 WCF 设置。|  
  
## <a name="remarks"></a>备注  

 将在堆栈中以下三个不同级别记录消息：服务、传输和格式不正确。 可以单独激活每个级别。  
  
 可添加 XPath 筛选器以记录传输和服务级别的特定消息。 如果未定义任何筛选器，则记录所有消息。 筛选器仅应用于消息的标头。 正文会被忽略。 WCF 将忽略消息正文，以便提高性能。 如果要根据正文内容进行筛选，可以创建一个自定义侦听器，并采用具有相应功能的筛选器。  
  
 您需要创建一个跟踪侦听器以激活消息跟踪。 侦听器本身可以是使用 <xref:System.Diagnostics> 跟踪体系结构的任何侦听器。 下面的示例演示如何创建此类侦听器。  
  
```xml  
<system.diagnostics>
  <sources>
    <source name="System.ServiceModel"
            switchValue="Verbose">
      <listeners>
        <clear />
        <add type="System.Diagnostics.DefaultTraceListener"
             name="Default"
             traceOutputOptions="None" />
        <add name="ServiceModel Listener"
             traceOutputOptions="None" />
      </listeners>
    </source>
    <source name="System.ServiceModel.MessageLogging">
      <listeners>
        <clear />
        <add type="System.Diagnostics.DefaultTraceListener"
             name="Default"
             traceOutputOptions="None" />
        <add name="MessageLogging Listener"
             traceOutputOptions="None" />
      </listeners>
    </source>
  </sources>
  <sharedListeners>
    <add initializeData="C:\ItProTools\TraceLog.xml"
         type="System.Diagnostics.XmlWriterTraceListener, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
         name="ServiceModel Listener"
         traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" />
    <add initializeData="C:\ItProTools\MessageLog.log"
         type="System.Diagnostics.XmlWriterTraceListener, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
         name="MessageLogging Listener"
         traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" />
  </sharedListeners>
</system.diagnostics>
```  
  
## <a name="example"></a>示例  
  
```xml  
<messageLogging logEntireMessage="true"
                logMalformedMessages="true"
                logMessagesAtServiceLevel="true"
                logMessagesAtTransportLevel="true"
                maxMessagesToLog="42"
                maxSizeOfMessageToLog="42">
  <filters>
    <clear />
  </filters>
</messageLogging>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.DiagnosticSection>
- <xref:System.ServiceModel.Diagnostics>
- <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement>
- [配置消息日志记录](../../../wcf/diagnostics/configuring-message-logging.md)
