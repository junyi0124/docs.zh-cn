---
title: 更改发送活动的缓存共享级别
ms.date: 03/30/2017
ms.assetid: 03926a64-753d-460e-ac06-2a4ff8e1bbf5
ms.openlocfilehash: cbb937ac47c93307db922b28e3df0ea694a77960
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262043"
---
# <a name="changing-the-cache-sharing-levels-for-send-activities"></a>更改发送活动的缓存共享级别

使用 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 扩展可以自定义缓存共享级别、通道工厂缓存的设置，以及使用 <xref:System.ServiceModel.Activities.Send> 消息传递活动将消息发送到服务终结点的工作流的通道缓存的设置。 这些工作流通常是客户端工作流，但也可以是在 <xref:System.ServiceModel.WorkflowServiceHost> 中承载的工作流服务。 通道工厂缓存包含已缓存的 <xref:System.ServiceModel.ChannelFactory%601> 对象。 通道缓存包含已缓存的通道。  
  
> [!NOTE]
> 工作流可以使用 <xref:System.ServiceModel.Activities.Send> 消息传递活动发送消息或参数。 工作流运行时将通道工厂添加到缓存，该缓存在您将 <xref:System.ServiceModel.Channels.IRequestChannel> 活动与 <xref:System.ServiceModel.Activities.ReceiveReply> 活动一起使用时会创建 <xref:System.ServiceModel.Activities.Send> 类型的通道，而在只使用 <xref:System.ServiceModel.Channels.IOutputChannel> 活动时（无 <xref:System.ServiceModel.Activities.Send>）会创建 <xref:System.ServiceModel.Activities.ReceiveReply> 类型的通道。  
  
## <a name="the-cache-sharing-levels"></a>缓存共享级别  

 默认情况下，在 <xref:System.ServiceModel.WorkflowServiceHost> 所承载的工作流中，由 <xref:System.ServiceModel.Activities.Send> 消息传递活动使用的缓存可在 <xref:System.ServiceModel.WorkflowServiceHost> 中的所有工作流实例之间共享（宿主级缓存）。 对于未由 <xref:System.ServiceModel.WorkflowServiceHost> 承载的客户端工作流，缓存仅对该工作流实例可用（实例级缓存）。 除非启用了不安全缓存，否则缓存只对未使用配置中定义的终结点的 <xref:System.ServiceModel.Activities.Send> 活动可用。  
  
 下面介绍了可供工作流中的 <xref:System.ServiceModel.Activities.Send> 活动使用的各种缓存共享级别及其建议用法：  
  
- **主机级别**：在宿主共享级别，缓存仅对工作流服务主机中承载的工作流实例可用。 缓存还可在进程范围的缓存中的工作流服务主机之间共享。  
  
- **实例级**：在实例共享级别，缓存在其整个生存期内可用于特定工作流实例，但该缓存不适用于其他工作流实例。  
  
- **无缓存**：默认情况下，如果你有使用配置中定义的终结点的工作流，则缓存处于关闭状态。 在这种情况下，由于打开缓存可能会造成不安全的情况，因此也建议关闭缓存。 例如，在针对每次发送都需要使用不同的标识（不同的凭据或使用模拟）时。  
  
## <a name="changing-the-cache-sharing-level-for-a-client-workflow"></a>更改客户端工作流的缓存共享级别  

 若要在客户端工作流中设置缓存共享，请将 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 类的实例作为扩展添加到所需的工作流实例集。 这样可在所有工作流实例之间共享缓存。 下面的代码示例演示如何执行这些步骤。  
  
 首先，声明一个 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 类型的实例。  
  
```csharp  
// Create an instance of SendMessageChannelCache with default cache settings.  
static SendMessageChannelCache sharedChannelCacheExtension =  
    new SendMessageChannelCache();  
```  
  
 接下来，将缓存扩展添加到各个客户端工作流实例。  
  
```csharp
WorkflowApplication clientInstance1 = new WorkflowApplication(new clientWorkflow1());  
WorkflowApplication clientInstance2 = new WorkflowApplication(new clientWorkflow2());  
  
// Share the cache extension object
  
clientInstance1.Extensions.Add(sharedChannelCacheExtension);  
clientInstance2.Extensions.Add(sharedChannelCacheExtension);  
```  
  
## <a name="changing-the-cache-sharing-level-for-a-hosted-workflow-service"></a>更改所承载的工作流服务的缓存共享级别  

 若要在所承载的工作流服务中设置缓存共享，请将 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 类的实例作为扩展添加到所有工作流服务主机。 这样可在所有工作流服务主机之间共享缓存。 下面的代码示例演示如何执行这些步骤。  
  
 首先，在类级别声明一个 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 类型的实例。  
  
```csharp  
// Create static instance of SendMessageChannelCache with default cache settings.  
static SendMessageChannelCache sharedChannelCacheExtension = new  
    SendMessageChannelCache();  
```  
  
 接下来，将静态缓存扩展添加到各个工作流服务主机。  
  
```csharp  
WorkflowServiceHost host1 = new WorkflowServiceHost(new serviceWorkflow1(), new Uri(baseAddress1));  
WorkflowServiceHost host2 = new WorkflowServiceHost(new serviceWorkflow2(), new Uri(baseAddress2));  
  
// Share the static cache to get an AppDomain level cache.  
host1.WorkflowExtensions.Add(sharedChannelCacheExtension);  
host2.WorkflowExtensions.Add(sharedChannelCacheExtension);  
```  
  
 若要将所承载的工作流服务的缓存共享设置为实例级缓存，请将 `Func<SendMessageChannelCache>` 委托作为扩展添加到工作流服务主机，并将此委托分配到实例化 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 类的新实例的代码中。 这将为各个工作流实例使用不同的缓存，而不是在工作流服务主机中的所有工作流实例之间共享单个缓存。 下面的代码示例演示如何使用 lambda 表达式直接定义委托所指向的 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 扩展来实现这一结果。  
  
```csharp
serviceHost.WorkflowExtensions.Add(() => new SendMessageChannelCache  
{  
    // Use FactorySettings property to add custom factory cache settings.  
    FactorySettings = new ChannelCacheSettings
    { MaxItemsInCache = 5, },  
    // Use ChannelSettings property to add custom channel cache settings.  
    ChannelSettings = new ChannelCacheSettings
    { MaxItemsInCache = 10 },  
});  
```  
  
## <a name="customizing-cache-settings"></a>自定义缓存设置  

 您可以为通道工厂缓存和通道缓存自定义缓存设置。 缓存设置在 <xref:System.ServiceModel.Activities.ChannelCacheSettings> 类中定义。 <xref:System.ServiceModel.Activities.SendMessageChannelCache>类在其无参数构造函数中为通道工厂缓存和通道缓存定义默认缓存设置。 下表针对各种缓存类型列出了这些缓存设置的默认值。  
  
|设置|LeaseTimeout（分钟）|IdleTimeout（分钟）|MaxItemsInCache|  
|-|-|-|-|  
|工厂缓存默认值|TimeSpan.MaxValue|2|16|  
|通道缓存默认值|5|2|16|  
  
 若要自定义工厂缓存和通道缓存设置，请使用参数化构造函数 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 实例化 <xref:System.ServiceModel.Activities.SendMessageChannelCache.%23ctor%2A> 类，并将 <xref:System.ServiceModel.Activities.ChannelCacheSettings> 的新实例以及自定义值传递到各个 `factorySettings` 和 `channelSettings` 参数。 接下来，将此类的新实例作为扩展添加到工作流服务主机或工作流实例。 下面的代码示例演示如何对工作流实例执行这些步骤。  
  
```csharp  
ChannelCacheSettings factorySettings = new ChannelCacheSettings{  
                        MaxItemsInCache = 5,
                        IdleTimeout = TimeSpan.FromMinutes(5),
                        LeaseTimeout = TimeSpan.FromMinutes(20)};  
  
ChannelCacheSettings channelSettings = new ChannelCacheSettings{  
                        MaxItemsInCache = 5,
                        IdleTimeout = TimeSpan.FromMinutes(2),  
                        LeaseTimeout = TimeSpan.FromMinutes(10) };  
  
SendMessageChannelCache customChannelCacheExtension =
    new SendMessageChannelCache(factorySettings, channelSettings);  
  
clientInstance.Extensions.Add(customChannelCacheExtension);  
```  
  
 当工作流服务的配置中定义了终结点时，如果要启用缓存，请使用参数化构造函数 <xref:System.ServiceModel.Activities.SendMessageChannelCache> 实例化 <xref:System.ServiceModel.Activities.SendMessageChannelCache.%23ctor%2A> 类，并将 `allowUnsafeCaching` 参数设置为 `true`。 接下来，将此类的新实例作为扩展添加到工作流服务主机或工作流实例。 下面的代码示例演示如何为工作流实例启用缓存。  
  
```csharp  
SendMessageChannelCache customChannelCacheExtension =
    new SendMessageChannelCache{ AllowUnsafeCaching = true };  
  
clientInstance.Extensions.Add(customChannelCacheExtension);  
```  
  
 若要完全禁用通道工厂和通道的缓存，请禁用通道工厂缓存。 这样还将关闭通道缓存，因为通道由其对应的通道工厂所拥有。 若要禁用通道工厂缓存，请将 `factorySettings` 参数传递到 <xref:System.ServiceModel.Activities.SendMessageChannelCache.%23ctor%2A> 构造函数，该参数使用 <xref:System.ServiceModel.Activities.ChannelCacheSettings> 值 0 初始化为 <xref:System.ServiceModel.Activities.ChannelCacheSettings.MaxItemsInCache%2A> 实例。 下面的代码示例演示了此过程。  
  
```csharp  
// Disable the factory cache. This results in the channel cache to be turned off as well.  
ChannelCacheSettings factorySettings = new ChannelCacheSettings  
    { MaxItemsInCache = 0 };  
  
ChannelCacheSettings channelSettings = new ChannelCacheSettings();  
  
SendMessageChannelCache customChannelCacheExtension =
    new SendMessageChannelCache(factorySettings, channelSettings);
  
clientInstance.Extensions.Add(customChannelCacheExtension);  
```  
  
 通过以下方法，可以选择仅使用通道工厂缓存并禁用通道缓存：将 `channelSettings` 参数 <xref:System.ServiceModel.Activities.SendMessageChannelCache.%23ctor%2A> 传递到构造函数，该参数使用 <xref:System.ServiceModel.Activities.ChannelCacheSettings> 值 0 初始化为 <xref:System.ServiceModel.Activities.ChannelCacheSettings.MaxItemsInCache%2A> 实例。 下面的代码示例演示了此过程。  
  
```csharp  
ChannelCacheSettings factorySettings = new ChannelCacheSettings();  
// Disable only the channel cache.  
ChannelCacheSettings channelSettings = new ChannelCacheSettings  
    { MaxItemsInCache = 0};  
  
SendMessageChannelCache customChannelCacheExtension =
    new SendMessageChannelCache(factorySettings, channelSettings);
  
clientInstance.Extensions.Add(customChannelCacheExtension);  
```  
  
 在承载的工作流服务中，可以在应用程序配置文件中指定工厂缓存和通道缓存设置。 为此，应添加一个包含工厂和通道缓存的缓存设置的服务行为，并将此服务行为添加到您的服务中。 下面的示例显示了配置文件的内容，其中包含 `MyChannelCacheBehavior` 具有自定义工厂缓存和通道缓存设置的服务行为。 此服务行为通过属性添加到服务 `behaviorConfiguration` 。  
  
```xml  
<configuration>
  <system.serviceModel>  
    <!-- List of other config sections here -->
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="MyChannelCacheBehavior">  
          <sendMessageChannelCache allowUnsafeCaching ="false" >  
            <!-- Control only the host level settings -->
            <factorySettings maxItemsInCache = "8" idleTimeout = "00:05:00" leaseTimeout="10:00:00" />  
            <channelSettings maxItemsInCache = "32" idleTimeout = "00:05:00" leaseTimeout="00:06:00" />  
          </sendMessageChannelCache>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service name="MyService" behaviorConfiguration="MyChannelCacheBehavior" />  
    </services>  
  </system.serviceModel>  
</configuration>  
```
