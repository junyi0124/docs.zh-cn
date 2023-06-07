---
title: 有关跟踪的安全注意事项和有用提示
ms.date: 03/30/2017
ms.assetid: 88bc2880-ecb9-47cd-9816-39016a07076f
ms.openlocfilehash: 415b27f5ac40d097c5bdf7b09d63ce901003f83f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243872"
---
# <a name="security-concerns-and-useful-tips-for-tracing"></a>有关跟踪的安全注意事项和有用提示

本主题说明防止敏感信息公开的方法以及使用 WebHost 时的有用提示。  
  
## <a name="using-a-custom-trace-listener-with-webhost"></a>将自定义跟踪侦听器与 WebHost 一起使用  

 如果您正在编写自己的跟踪侦听器，则应当注意在 Web 承载的服务中，跟踪可能会丢失。 当 WebHost 回收时，它会关闭活动进程，由一个副本进程来接替该进程。 但是，在一段时间内，这两个进程必须仍具有对同一资源的访问权，具体取决于侦听器类型。 在这种情况下，`XmlWriterTraceListener` 会为第二个进程创建一个新的跟踪文件，Windows 事件跟踪会对同一会话中的多个进程进行管理并给予同一文件的访问权。 如果您自己的侦听器无法提供类似功能，则当该文件被这两个进程锁定时，跟踪可能会丢失。  
  
 您还应当注意，自定义跟踪侦听器可以在网络上发送跟踪和消息，例如发送到远程数据库。 作为应用程序部署人员，您应当使用适当的访问控制来配置自定义侦听器。 您还应当对可以在远程位置上公开的任何个人信息应用安全控制。  
  
## <a name="logging-sensitive-information"></a>记录敏感信息  

 当消息在范围内时，跟踪包含消息头。 启用跟踪后，特定于应用程序的标头中的个人信息（例如查询字符串）以及正文信息（例如信用卡号）会在日志中变为可见。 应用程序部署人员负责实施对配置和跟踪文件的访问控制。 如果您不希望此类信息可见，应当禁用跟踪，或者如果您希望共享跟踪日志，则筛选出其中的部分数据。  
  
 下面的提示可帮助您避免意外公开跟踪文件的内容：  
  
- 确保在 WebHost 和自承载方案中都使用访问控制列表 (ACL) 对日志文件进行保护。  
  
- 选择无法使用 Web 请求轻松提供的文件扩展名。 例如，.xml 文件扩展名不是一个安全的选择。 可以参考 IIS 管理指南以查看可提供的扩展名的列表。  
  
- 为日志文件位置指定绝对路径，该位置应当在 WebHost vroot 公共目录之外，以防止外部方使用 Web 浏览器来访问它。  
  
 默认情况下，不会在跟踪和已记录消息中记录密钥和个人身份信息 (PII)（例如用户名和密码）。 但是，计算机管理员可以使用 Machine.config 文件的 `enableLoggingKnownPII` 元素中的 `machineSettings` 属性来允许计算机上运行的应用程序记录已知的个人身份信息 (PII)，如下所示：  
  
```xml  
<configuration>  
   <system.ServiceModel>  
      <machineSettings enableLoggingKnownPii="Boolean"/>  
   </system.ServiceModel>  
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
            <add name="xml" />  
         </listeners>  
      </source>  
  </sources>  
</system.diagnostics>  
```  
  
 如果 `<machineSettings enableLoggingKnownPii="Boolean"/>`<xref:System.Configuration.ConfigurationErrorsException> 元素不在 Machine.config 文件中，则系统引发。  
  
 只有当应用程序启动或重新启动之后，更改才有效。 在两个属性都设置为 `true` 的情况下，会在启动时记录一个事件。 如果 `logKnownPii` 设置为 `true` 但 `enableLoggingKnownPii` 设置为 `false`，也会记录一个事件。  
  
 有关 PII 日志记录的详细信息，请参阅 [Pii 安全锁定](../../samples/pii-security-lockdown.md) 示例。  
  
 计算机管理员和应用程序部署人员应谨慎使用这两个开关。 如果启用了 PII 日志记录，则会记录安全密钥和 PII。 如果禁用了 PII 日志记录，仍会在消息头和正文中记录敏感数据和特定于应用程序的数据。 若要深入了解隐私并保护 PII，请参阅 [用户隐私](/previous-versions/dotnet/articles/aa480490(v=msdn.10))。  
  
 另外，对于面向连接的传输，每次连接时会记录一次消息发送方的 IP 地址；对于非面向连接的传输，每发送一条消息会记录一次消息发送方的 IP 地址。 这是在未经发送方同意的情况下进行的。 不过，只有在“信息”或“详细”跟踪级别才会发生此日志记录，这些级别不是生产中的默认或推荐跟踪级别（现场调试时除外）。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
