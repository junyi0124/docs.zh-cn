---
title: 配置消息日志记录
description: 了解如何配置消息日志记录，包括如何启用日志记录、日志记录级别、消息筛选器以及如何在 WCF 中配置自定义侦听器。
ms.date: 03/30/2017
helpviewer_keywords:
- message logging [WCF]
ms.assetid: 0ff4c857-8f09-4b85-9dc0-89084706e4c9
ms.openlocfilehash: 5203f19a18e5fa6b0ed7f68e1d1de0447da41abd
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247657"
---
# <a name="configuring-message-logging"></a>配置消息日志记录

本主题描述如何针对不同的方案配置消息日志记录。

## <a name="enabling-message-logging"></a>启用消息日志记录

默认情况下，Windows Communication Foundation （WCF）不记录消息。 若要激活消息日志记录，必须向 `System.ServiceModel.MessageLogging` 跟踪源添加跟踪侦听器，并在配置文件中设置 `<messagelogging>` 元素的特性。

下面的示例演示如何启用日志记录和指定其他选项。

```xml
<system.diagnostics>
  <sources>
    <source name="System.ServiceModel.MessageLogging">
      <listeners>
         <add name="messages"
              type="System.Diagnostics.XmlWriterTraceListener"
              initializeData="c:\logs\messages.svclog" />
        </listeners>
    </source>
  </sources>
</system.diagnostics>

<system.serviceModel>
  <diagnostics>
    <messageLogging
         logEntireMessage="true"
         logMalformedMessages="false"
         logMessagesAtServiceLevel="true"
         logMessagesAtTransportLevel="false"
         maxMessagesToLog="3000"
         maxSizeOfMessageToLog="2000"/>
  </diagnostics>
</system.serviceModel>
```

有关消息日志记录设置的详细信息，请参阅[用于跟踪和消息日志记录的推荐设置](./tracing/recommended-settings-for-tracing-and-message-logging.md)。

可以使用 `add` 指定要使用的侦听器的名称和类型。 在示例配置中，侦听器命名为“messages”，并添加了标准 .NET Framework 跟踪侦听器(`System.Diagnostics.XmlWriterTraceListener`)，作为要使用的类型。 如果使用 `System.Diagnostics.XmlWriterTraceListener`，则必须在配置文件中指定输出文件的位置和名称。 这是通过将 `initializeData` 设置为该日志文件的名称来完成的。 否则，系统将引发异常。 还可以实现能向默认文件发出日志的自定义侦听器。

> [!NOTE]
> 因为消息日志记录访问磁盘空间，所以应限制为特定服务而写入磁盘的消息数量。 当达到消息限制时，会生成信息级别的跟踪，并停止所有消息日志记录活动。

日志记录的级别以及其他选项在 Logging Level and Options（日志记录的级别和选项）部分中讨论。

`switchValue` 的 `source` 属性只对跟踪有效。 如果按下面这样为 `switchValue` 跟踪源指定 `System.ServiceModel.MessageLogging` 属性，则无效。

```xml
<source name="System.ServiceModel.MessageLogging" switchValue="Verbose">
</source>
```

如果希望禁用跟踪源，应该使用 `logMessagesAtServiceLevel`, `logMalformedMessages` 以及 `logMessagesAtTransportLevel` 元素的 `messageLogging` 属性。 应将所有这些属性设置为 `false`。 可以使用前面的代码示例中的配置文件、通过配置编辑器的 UI 接口或使用 WMI 来完成此操作。 有关配置编辑器工具的详细信息，请参阅[配置编辑器工具（SvcConfigEditor.exe）](../configuration-editor-tool-svcconfigeditor-exe.md)。 有关 WMI 的详细信息，请参阅[使用 Windows Management Instrumentation 诊断](./wmi/index.md)。

## <a name="logging-levels-and-options"></a>日志记录的级别和选项

对于传入消息，在消息形成后、在消息到达服务级别中的用户代码之前以及在检测到格式不正确的消息时，都会立即进行日志记录。

对于传出消息，在消息离开用户代码之后和消息进入网络之前，都会立即进行日志记录。

WCF 在两个不同的级别、服务和传输中记录消息。 也记录格式不正确的消息。 这三种类别是互相独立的，可以在配置中单独激活。

可以通过设置 `logMessagesAtServiceLevel` 元素的 `logMalformedMessages`、`logMessagesAtTransportLevel` 和 `messageLogging` 属性来控制日志记录的级别。

### <a name="service-level"></a>服务级别

在这个层上记录的消息是关于进入用户代码（在接收时）或离开用户代码（在发送时）的情况。 如果已定义筛选器，则仅记录与筛选器相匹配的消息。 否则将记录服务级别上的所有消息。 基础结构消息（事务、对等通道和安全性）也在此级别上记录，但可靠传递消息除外。 对于经过流处理的消息，则只记录标头。 此外，安全消息在此级别上按解密记录。

### <a name="transport-level"></a>“传输”级别

在这个层上记录的消息已准备好进行编码以便在网络上传输，或已准备好在经过网络传输后进行解码。 如果已定义筛选器，则仅记录与筛选器相匹配的消息。 否则将记录传输层上的所有消息。 所有基础结构消息都在此层上记录，包括可靠传递消息。 对于经过流处理的消息，则只记录标头。 此外，安全消息在此级别上按加密记录，但使用诸如 HTTPS 的安全传输时除外。

### <a name="malformed-level"></a>“格式不正确”级别

格式不正确的消息是指在任何处理阶段由 WCF 堆栈拒绝的消息。 格式错误的消息将被如实记录：已加密（如果是加密的）、带有不适当的 XML 等。 `maxSizeOfMessageToLog` 定义了记录为 CDATA 的消息大小。 默认情况下，`maxSizeOfMessageToLog` 等于 256K。 有关此属性的详细信息，请参阅 "其他选项" 部分。

### <a name="other-options"></a>其他选项

除了日志记录的级别外，用户可以指定以下选项：

- 记录整个消息（`logEntireMessage` 属性）：该值指定是否记录整个消息（标头和正文）。 默认值为 `false`，这意味着仅记录标头。 此设置会影响服务和传输消息日志记录级别。

- 要记录的最大消息数（`maxMessagesToLog` 属性）：该值指定要记录的最大消息数。 所有的消息（服务、传输和格式不正确消息）都统计到该配额中。 达到配额上限时，会发出一个跟踪，并且不再记录更多的消息。 默认值为 10000。

- 要记录的消息的最大大小（`maxSizeOfMessageToLog` 属性）：该值指定要记录的消息的最大大小（以字节为单位）。 超过该大小限制的消息将不作记录，也不对该消息执行任何其他活动。 此设置会影响所有跟踪级别。 如果 ServiceModel 跟踪打开，将在第一个记录点发出警告级别的跟踪（ServiceModelSend* 或 TransportReceive）以通知用户。 服务级别和传输级别的消息的默认值为 256K，而格式不正确消息的默认值为 4K。

  > [!CAUTION]
  > 计算出来与 `maxSizeOfMessageToLog` 进行比较的消息大小是序列化之前内存中的消息大小。 该大小可能与正在记录的消息字符串的实际长度不同，在很多情况下比实际大小更大。 结果可能无法记录消息。 可以通过将 `maxSizeOfMessageToLog` 属性指定为比预期的消息大小大 10% 来解决这个问题。 此外，如果记录了格式不正确的消息，消息日志所占用的实际磁盘空间可能达到 `maxSizeOfMessageToLog` 所指定的值的 5 倍。

如果在配置文件中没有定义跟踪侦听器，则不论指定什么记录级别，都不会生成记录输出。

对于消息日志记录选项（例如在本部分描述的属性），可以使用 Windows Management Instrumentation (WMI) 在运行时进行更改。 这可以通过访问[AppDomainInfo](./wmi/appdomaininfo.md)实例来完成，此实例将公开以下布尔属性： `LogMessagesAtServiceLevel` 、 `LogMessagesAtTransportLevel` 和 `LogMalformedMessages` 。 因此，如果为消息日志记录配置了跟踪侦听器，但是在配置中将这些选项设置为 `false`，那么可以在以后运行应用程序时将它们更改为 `true`。 这会在运行时有效地启用消息日志记录。 同样，如果在配置文件中启用了消息日志记录，可以在运行时使用 WMI 将其禁用。 有关详细信息，请参阅[使用诊断 Windows Management Instrumentation](./wmi/index.md)。

消息记录中的 `source` 字段指定了在何种上下文中记录消息：在发送/接收请求消息时、在进行请求-答复或单向请求时、在服务模型或传输层上或者在发现格式不正确的消息时。

对于格式不正确的消息， `source` 等于 `Malformed` 。 否则，根据上下文，源具有以下值。

对于请求/答复

||发送请求|接收请求|发送答复|接收答复|
|-|------------------|---------------------|----------------|-------------------|
|服务模型层|服务<br /><br /> Level<br /><br /> Send<br /><br /> 请求|服务<br /><br /> Level<br /><br /> 接收<br /><br /> 请求|服务<br /><br /> Level<br /><br /> Send<br /><br /> 回复|服务<br /><br /> Level<br /><br /> 接收<br /><br /> 回复|
|传输层|传输<br /><br /> Send|传输<br /><br /> 接收|传输<br /><br /> Send|传输<br /><br /> 接收|

对于单向请求

||发送请求|接收请求|
|-|------------------|---------------------|
|服务模型层|服务<br /><br /> Level<br /><br /> Send<br /><br /> 数据报|服务<br /><br /> Level<br /><br /> 接收<br /><br /> 数据报|
|传输层|传输<br /><br /> Send|传输<br /><br /> 接收|

## <a name="message-filters"></a>消息筛选器

消息筛选器在 `messageLogging` 配置节的 `diagnostics` 配置元素中定义。 它们在服务和传输级别上应用。 如果定义一个或多个筛选器，则仅记录与其中至少一个筛选器相匹配的消息。 如果未定义任何筛选器，则所有消息都可通过。

筛选器支持完整的 XPath 语法，并按照其在配置文件中出现的顺序进行应用。 存在语法错误的筛选器会导致配置异常。

筛选器还使用 `nodeQuota` 属性提供安全功能，该属性限制 XPath DOM 中可以进行检查以便与筛选器匹配的最大节点数量。

下面的示例演示如何配置一个筛选器，它仅记录具有 SOAP 标头部分的消息。

```xml
<messageLogging logEntireMessage="true"
    logMalformedMessages="true"
    logMessagesAtServiceLevel="true"
    logMessagesAtTransportLevel="true"
    maxMessagesToLog="420">
    <filters>
        <add nodeQuota="10" xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
                 /soap:Envelope/soap:Header
        </add>
     </filters>
</messageLogging>
```

筛选器不能应用于消息正文。 尝试对消息正文进行操作的筛选器会从筛选器列表中删除。 此外还会发出一个指示此事的事件。 例如，下面的筛选器会从筛选器表中删除。

```xml
<add xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">/s:Envelope/s:Body[contains(text(), "Hello")]</add>
```

## <a name="configuring-a-custom-listener"></a>配置自定义侦听器

您还可以配置具有其他选项的自定义侦听器。 若要在记录之前将特定于应用程序的 PII 元素从消息中筛选出来，自定义侦听器可能很有用。 下面的示例演示自定义侦听器配置。

```xml
<system.diagnostics>
   <sources>
     <source name="System.ServiceModel.MessageLogging">
           <listeners>
             <add name="MyListener"
                    type="YourCustomListener"
                    initializeData="c:\logs\messages.svclog"
                    maxDiskSpace="1000"/>
           </listeners>
     </source>
   </sources>
</system.diagnostics>
```

请您注意，`type` 属性应该设置为该类型的限定程序集名称。

## <a name="see-also"></a>请参阅

- [\<messageLogging>](../../configure-apps/file-schema/wcf/messagelogging.md)
- [消息日志记录](message-logging.md)
- [跟踪和消息日志记录的推荐设置](./tracing/recommended-settings-for-tracing-and-message-logging.md)
