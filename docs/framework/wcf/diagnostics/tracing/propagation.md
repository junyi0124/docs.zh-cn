---
title: 传播
ms.date: 03/30/2017
ms.assetid: f8181e75-d693-48d1-b333-a776ad3b382a
ms.openlocfilehash: be010178d8f0face8f6c7e986107e4ea90d91953
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240129"
---
# <a name="propagation"></a>传播

本主题介绍 Windows Communication Foundation (WCF) 跟踪模型中的活动传播。  
  
## <a name="using-propagation-to-correlate-activities-across-endpoints"></a>使用传播关联终结点之间的活动  

 传播可向用户提供应用程序终结点之间相同处理单元的错误跟踪的直接关联，如请求。 在不同终结点为相同处理单元发出的错误将被分组到相同的活动中，甚至跨应用程序域。 这是通过活动 ID 在消息头中的传播实现的。 因此，如果客户端由于服务器内部错误而超时，则这两个错误都会显示在同一活动中以便直接关联。  
  
 为此，可以按前面示例所示使用 `ActivityTracing` 设置。 此外，在所有终结点处设置 `propagateActivity` 跟踪源的 `System.ServiceModel` 属性。  
  
```xml  
<source name="System.ServiceModel" switchValue="Verbose,ActivityTracing" propagateActivity="true" >  
```  
  
 活动传播是一种可配置的功能，可使 WCF 向出站消息添加标头，其中包括 TLS 上的活动 ID。 通过在服务器端的后续跟踪上包括此 ID，可以关联客户端和服务器活动。  
  
## <a name="propagation-definition"></a>传播定义  

 如果下列所有条件都适用，则活动 M 的 gAId 会传播到活动 N。  
  
- N 是为 M 创建的  
  
- N 已知 M 的 gAId  
  
- N 的 gAId 与 M 的 gAId 相同。  
  
 gAId 通过 ActivityId 消息头传播，如下面的 XML 架构所示。  
  
```xml  
<xsd:element name="ActivityId" type="integer" minOccurs="0">  
  <xsd:attribute name="CorrelationId" type="integer" minOccurs="0"/>  
</xsd:element>  
```  
  
 下面是一个消息头示例。  
  
```xml  
<MessageLogTraceRecord>  
  <s:Envelope xmlns:s="http://www.w3.org/2003/05/soap-envelope"
                      xmlns:a="http://www.w3.org/2005/08/addressing">  
    <s:Header>  
      <a:Action s:mustUnderstand="1">http://Microsoft.ServiceModel.Samples/ICalculator/Subtract  
      </a:Action>  
      <a:MessageID>urn:uuid:f0091eae-d339-4c7e-9408-ece34602f1ce  
      </a:MessageID>  
      <ActivityId CorrelationId="f94c6af1-7d5d-4295-b693-4670a8a0ce34"
               xmlns="http://schemas.microsoft.com/2004/09/ServiceModel/Diagnostics">  
        17f59a29-b435-4a15-bf7b-642ffc40eac8  
      </ActivityId>  
      <a:ReplyTo>  
          <a:Address>http://www.w3.org/2005/08/addressing/anonymous</a:Address>  
      </a:ReplyTo>  
      <a:To s:mustUnderstand="1">net.tcp://localhost/servicemodelsamples/service</a:To>  
   </s:Header>  
   <s:Body>  
     <Subtract xmlns="http://Microsoft.ServiceModel.Samples">  
       <n1>145</n1>  
       <n2>76.54</n2>  
     </Subtract>  
   </s:Body>  
  </s:Envelope>  
</MessageLogTraceRecord>  
```  
  
## <a name="propagation-and-activity-boundaries"></a>传播和活动边界  

 当活动 ID 在终结点之间传播时，消息接收方使用该（传播的）活动 ID 发出开始跟踪和停止跟踪。 因此，每个跟踪源的 gAId 都具有开始和停止跟踪。 如果终结点位于同一个进程中，且使用相同的跟踪源名称，则会创建多个具有相同 lAId（相同 gAId、相同跟踪源和相同进程）的开始和停止跟踪。  
  
## <a name="synchronization"></a>同步  

 为了同步不同计算机上运行的各终结点之间的事件，向在消息中传播的 ActivityId 标头中添加了一个 CorrelationId。 工具可使用此 ID 来同步计算机之间具有时钟差的事件。 具体地说，服务跟踪查看器工具可使用此 ID 显示终结点之间的消息流。  
  
## <a name="see-also"></a>另请参阅

- [配置跟踪](configuring-tracing.md)
- [使用服务跟踪查看器查看相关跟踪和进行故障诊断](using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md)
- [端到端跟踪方案](end-to-end-tracing-scenarios.md)
- [服务跟踪查看器工具 (SvcTraceViewer.exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md)
