---
title: 消息日志记录的安全问题
ms.date: 03/30/2017
ms.assetid: 21f513f2-815b-47f3-85a6-03c008510038
ms.openlocfilehash: 8594329fb27aa1d77a2baffee2a7e37ea0d009c4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283765"
---
# <a name="security-concerns-for-message-logging"></a>消息日志记录的安全问题

本主题描述如何防止在消息日志以及由消息日志记录生成的事件中公开敏感数据。  
  
## <a name="security-concerns"></a>安全问题  
  
### <a name="logging-sensitive-information"></a>记录敏感信息  

 Windows Communication Foundation (WCF) 不会修改特定于应用程序的标头和正文中的任何数据。 WCF 还不跟踪特定于应用程序的标头或正文数据中的个人信息。  
  
 启用消息日志记录后，特定于应用程序的标头中的个人信息（例如查询字符串）以及正文信息（例如信用卡号）会在日志中变为可见。 应用程序部署人员负责对配置和日志文件实施访问控制。 如果您不希望此类信息可见，应当禁用日志记录，或者如果您希望共享日志，则筛选出其中的部分数据。  
  
 下面的提示有助于您避免意外公开日志文件的内容：  
  
- 确保在 Web 承载和自承载方案中都使用访问控制列表 (ACL) 对日志文件进行保护。  
  
- 选择无法使用 Web 请求轻松提供的文件扩展名。 例如，.xml 文件扩展名不是一个安全的选择。 可以参考 Internet 信息服务 (IIS) 管理指南以查看可提供的扩展名的列表。  
  
- 为日志文件位置指定绝对路径，该位置应当在 Web 主机虚拟根目录公共目录之外，以防止外部方使用 Web 浏览器进行访问。  
  
 默认情况下，不会在跟踪和已记录消息中记录密钥和个人身份信息 (PII)（例如用户名和密码）。 但是，计算机管理员可以在 Machine.config 文件的 `enableLoggingKnownPII` 元素中使用 `machineSettings` 属性来允许计算机上运行的应用程序记录已知的个人身份信息 (PII)。 下面的配置演示如何执行该操作：  
  
```xml  
<configuration>  
   <system.serviceModel>  
      <machineSettings enableLoggingKnownPii="true"/>  
   </system.serviceModel>  
</configuration>
```  
  
 应用程序部署人员然后可以使用 App.config 或 Web.config 文件中的 `logKnownPii` 属性来启用 PII 日志记录，如下所示：  
  
```xml  
<system.diagnostics>  
  <sources>  
      <source name="System.ServiceModel.MessageLogging"  
        logKnownPii="true">  
        <listeners>  
                 <add name="messages"  
                 type="System.Diagnostics.XmlWriterTraceListener"  
                 initializeData="c:\logs\messages.svclog" />  
          </listeners>  
      </source>  
    </sources>  
</system.diagnostics>  
```  
  
 只有当两个设置都为 `true` 时，才会启用 PII 日志记录。 两个开关的组合为记录每个应用程序的已知 PII 带来了很大的灵活性。  
  
> [!IMPORTANT]
> 在 [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 中，`logEntireMessage` 和 `logKnownPii` 标志还必须在 Web.config 文件或 App.config 文件中设置为 `true` 以启用 PII 日志记录，如下面的示例 `<system.serviceModel><messageLogging logEntireMessage="true" logKnownPii="true" …` 所示。  
  
 您应当注意，如果在一个配置文件中指定了两个或更多自定义源，则只读取第一个源的特性， 而忽略其他源的属性。 这意味着，对于以下 App.config 文件，即使已为第二个源显式启用了 PII 日志记录，也不会同时为这两个源记录 PII。  
  
```xml  
<system.diagnostics>  
   <sources>  
      <source name="System.ServiceModel.MessageLogging"  
              logKnownPii="false">  
              <listeners>  
                 <add name="messages"  
                      type="System.Diagnostics.XmlWriterTraceListener"  
                      initializeData="c:\logs\messages.svclog" />  
              </listeners>  
            </source>  
      <source name="System.ServiceModel"
              logKnownPii="true">  
              <listeners>  
                 <add name="traces"  
                      type="System.Diagnostics.XmlWriterTraceListener"  
                      initializeData="c:\logs\traces.svclog" />  
              </listeners>  
      </source>  
   </sources>  
</system.diagnostics>  
```  
  
 如果 `<machineSettings enableLoggingKnownPii="Boolean"/>`<xref:System.Configuration.ConfigurationErrorsException> 元素不在 Machine.config 文件中，则系统引发。  
  
 只有当应用程序启动或重新启动之后，更改才有效。 在两个属性都设置为 `true` 的情况下，会在启动时记录一个事件。 如果 `logKnownPii` 设置为 `true` 但 `enableLoggingKnownPii` 设置为 `false`，也会记录一个事件。  
  
 计算机管理员和应用程序部署人员应谨慎使用这两个开关。 如果启用了 PII 日志记录，则会记录安全密钥和 PII。 如果禁用了 PII 日志记录，仍会在消息头和正文中记录敏感数据和特定于应用程序的数据。 有关隐私和保护 PII 的详细讨论，请参阅 [用户隐私](/previous-versions/dotnet/articles/aa480490(v=msdn.10))。  
  
> [!CAUTION]
> 在格式不正确的消息中不会隐藏 PII。 这样的消息按原样记录，不进行任何修改。 前面提到的属性对此没有影响。  
  
### <a name="custom-trace-listener"></a>自定义跟踪侦听器  

 在消息日志记录跟踪源上添加定义跟踪侦听器是一种只限于管理员的特权。 这是因为恶意自定义侦听器可以配置为远程发送消息，从而导致敏感信息泄漏。 此外，如果您将自定义侦听器配置为在网络上发送消息（例如发送到远程数据库），您应该对远程计算机上的消息日志施加适当的访问控制。  
  
## <a name="events-triggered-by-message-logging"></a>消息日志记录触发的事件  

 下面列出了消息日志记录发出的所有事件。  
  
- 启用消息日志记录：当在配置中或通过 WMI 启用消息日志记录时发出此事件。 此事件的内容是“消息日志记录已打开。 将记录敏感信息，即使已在网络上对其进行加密: 例如，消息正文。”  
  
- 禁用消息日志记录：当通过 WMI 禁用消息日志记录时发出此事件。 此事件的内容是“消息日志记录已关闭”。  
  
- 启用记录已知 PII：当启用记录已知 PII 时发出此事件。 如果 `enableLoggingKnownPii` Machine.config 文件的元素中的属性 `machineSettings` 设置为 `true` ，并且 `logKnownPii` `source` App.config 或 Web.config 文件中元素的属性设置为， `true` 则会发生这种情况。  
  
- 不允许记录已知 PII：当不允许记录已知 PII 时发出此事件。 如果 `logKnownPii` `source` App.config 或 Web.config 文件中元素的属性设置为 `true` ，但 `enableLoggingKnownPii` Machine.config 文件的元素中的属性 `machineSettings` 设置为， `false` 则会发生这种情况。 不引发异常。  
  
 可以在 Windows 附带的事件查看器工具中查看这些事件。 有关此内容的详细信息，请参阅 [事件日志记录](./event-logging/index.md)。  
  
## <a name="see-also"></a>另请参阅

- [消息日志记录](message-logging.md)
- [有关跟踪的安全注意事项和有用提示](./tracing/security-concerns-and-useful-tips-for-tracing.md)
