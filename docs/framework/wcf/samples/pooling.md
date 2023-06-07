---
title: Pooling
ms.date: 03/30/2017
ms.assetid: 688dfb30-b79a-4cad-a687-8302f8a9ad6a
ms.openlocfilehash: 6b266dafa945fa44d6c857810df42eb5439f157d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255424"
---
# <a name="pooling"></a>Pooling

此示例演示如何扩展 Windows Communication Foundation (WCF) 以支持对象池。 本示例演示如何创建在语法和语义上与企业服务的 `ObjectPoolingAttribute` 属性功能相似的属性。 对象池可以显著提高应用程序的性能。 但是，如果使用不正确也可能产生负面影响。 对象池有助于减少重新创建经常使用且要求频繁进行初始化的对象的开销。 但是，如果调用缓冲池对象上的方法要花大量时间完成，则对象池在刚达到最大池大小时就会对其他请求排队。 因此可能不能为某些对象创建请求提供服务，这时将引发超时异常。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 创建 WCF 扩展的第一步是确定要使用的扩展点。  
  
 在 WCF 中，术语 *发送器* 指的是一个运行时组件，该组件负责将传入消息转换为用户服务上的方法调用，并将返回值从该方法转换为传出消息。 WCF 服务为每个终结点创建一个调度程序。 如果与客户端关联的协定是双工协定，则 WCF 客户端必须使用调度程序。  
  
 通道和终结点调度程序通过公开用于控制调度程序行为的不同属性，来提供通道和协定范围的扩展性。 使用 <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.DispatchRuntime%2A> 属性还可以检查、修改或自定义调度过程。 本示例重点介绍 <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> 属性，该属性指向提供服务类实例的对象。  
  
## <a name="the-iinstanceprovider"></a>IInstanceProvider  

 在 WCF 中，调度程序使用实现接口的来创建服务类的实例 <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> <xref:System.ServiceModel.Dispatcher.IInstanceProvider> 。 此接口有三个方法：  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29>：当消息到达调度程序时，调用 <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> 方法创建服务类的实例处理消息。 调用此方法的频率由 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 属性决定。 例如，如果 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 属性设置为 <xref:System.ServiceModel.InstanceContextMode.PerCall>，则创建一个新的服务类实例来处理到达的每个消息，因此每当消息到达时，都将调用 <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29>。  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%29>：与上一个方法基本相同，不同之处是此方法在没有 Message 自变量时调用。  
  
- <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%28System.ServiceModel.InstanceContext%2CSystem.Object%29>：当超过服务实例的生存期时，调度程序将调用 <xref:System.ServiceModel.Dispatcher.IInstanceProvider.ReleaseInstance%28System.ServiceModel.InstanceContext%2CSystem.Object%29> 方法。 与 <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> 方法相同，调用此方法的频率是由 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 属性确定的。  
  
## <a name="the-object-pool"></a>对象池  

 自定义 <xref:System.ServiceModel.Dispatcher.IInstanceProvider> 实现为服务提供所需的对象池语义。 因此，此示例有一个为池提供 `ObjectPoolingInstanceProvider` 自定义实现的 <xref:System.ServiceModel.Dispatcher.IInstanceProvider> 类型。 当 `Dispatcher` 调用 <xref:System.ServiceModel.Dispatcher.IInstanceProvider.GetInstance%28System.ServiceModel.InstanceContext%2CSystem.ServiceModel.Channels.Message%29> 方法时，自定义实现将在内存池中寻找现有对象，而不是创建新的实例。 如果找到一个对象，则返回该对象。 否则，将创建新对象。 下面的示例代码演示了 `GetInstance` 的实现。  
  
```csharp  
object IInstanceProvider.GetInstance(InstanceContext instanceContext, Message message)  
{  
    object obj = null;  
  
    lock (poolLock)  
    {  
        if (pool.Count > 0)  
        {  
            obj = pool.Pop();  
        }  
        else  
        {  
            obj = CreateNewPoolObject();  
        }  
        activeObjectsCount++;  
    }  
  
    WritePoolMessage(ResourceHelper.GetString("MsgNewObject"));  
  
    idleTimer.Stop();  
  
    return obj;
}  
```  
  
 自定义 `ReleaseInstance` 实现将已释放的实例重新添加回池中并减少 `ActiveObjectsCount` 值。 `Dispatcher` 可从不同的线程调用这些方法，因此需要同步访问 `ObjectPoolingInstanceProvider` 类中的类级别成员。  
  
```csharp  
void IInstanceProvider.ReleaseInstance(InstanceContext instanceContext, object instance)  
{  
    lock (poolLock)  
    {  
        pool.Push(instance);  
        activeObjectsCount--;  
  
        WritePoolMessage(  
        ResourceHelper.GetString("MsgObjectPooled"));  
  
        // When the service goes completely idle (no requests
        // are being processed), the idle timer is started  
        if (activeObjectsCount == 0)  
            idleTimer.Start();
    }  
}  
```  
  
 `ReleaseInstance`方法提供 "清理初始化" 功能。 通常，该池为其生存期保持最少数量的对象。 但是，可能有过度使用的时期，此时需要在池中创建更多对象以达到配置中指定的最大限制。 最终，当池变得不是很活跃时，那些多余的对象可能成为额外的开销。 因此，当 `activeObjectsCount` 到达 0 时，会启用一个空闲计时器，该计时器将触发并执行清除循环。  
  
## <a name="adding-the-behavior"></a>添加行为  

 使用下面的行为将调度程序层扩展挂钩：  
  
- 服务行为。 这些行为允许自定义整个服务运行时。  
  
- 终结点行为。 这些行为允许自定义服务终结点，特别是通道和终结点调度程序。  
  
- 协定行为。 这些行为允许分别自定义客户端和服务上的 <xref:System.ServiceModel.Dispatcher.ClientRuntime> 和 <xref:System.ServiceModel.Dispatcher.DispatchRuntime> 类。  
  
 为了实现对象池扩展，必须创建服务行为。 服务行为是通过实现 <xref:System.ServiceModel.Description.IServiceBehavior> 接口创建的。 可以通过多种方式使服务模型注意到自定义行为：  
  
- 使用自定义属性。  
  
- 强制将它添加到服务说明的行为集合中。  
  
- 扩展配置文件。  
  
 本示例使用自定义属性。 当构造 <xref:System.ServiceModel.ServiceHost> 时，本示例检查用于服务的类型定义的属性并将可用行为添加到服务说明的行为集合中。  
  
 接口 <xref:System.ServiceModel.Description.IServiceBehavior> 包含三个方法 -- <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A>、<xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A> 和 <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A>。 <xref:System.ServiceModel.Description.IServiceBehavior.Validate%2A> 方法用于确保该行为可以应用于服务。 在本示例中，此实现确保不使用 <xref:System.ServiceModel.InstanceContextMode.Single> 配置服务。 <xref:System.ServiceModel.Description.IServiceBehavior.AddBindingParameters%2A> 方法用于配置服务的绑定。 它不是本方案所必需的。 <xref:System.ServiceModel.Description.IServiceBehavior.ApplyDispatchBehavior%2A> 用于配置服务的调度程序。 初始化时，WCF 将调用此方法 <xref:System.ServiceModel.ServiceHost> 。 下列参数将传递到此方法：  
  
- `Description`：此自变量提供整个服务的服务说明。 它可用于检查有关服务的终结点、协定、绑定和其他数据的说明数据。  
  
- `ServiceHostBase`：此自变量提供当前正在初始化的 <xref:System.ServiceModel.ServiceHostBase>。  
  
 在自定义 <xref:System.ServiceModel.Description.IServiceBehavior> 实现中，会实例化一个新的 `ObjectPoolingInstanceProvider` 实例，并将该实例分配到 ServiceHostBase 的每个 <xref:System.ServiceModel.Dispatcher.DispatchRuntime.InstanceProvider%2A> 的 <xref:System.ServiceModel.Dispatcher.DispatchRuntime> 属性。  
  
```csharp  
void IServiceBehavior.ApplyDispatchBehavior(ServiceDescription description, ServiceHostBase serviceHostBase)  
{  
    // Create an instance of the ObjectPoolInstanceProvider.  
    ObjectPoolingInstanceProvider instanceProvider = new  
           ObjectPoolingInstanceProvider(description.ServiceType,
                                                    minPoolSize);  
  
    // Forward the call if we created a ServiceThrottlingBehavior.  
    if (this.throttlingBehavior != null)  
    {  
        ((IServiceBehavior)this.throttlingBehavior).ApplyDispatchBehavior(description, serviceHostBase);  
    }  
  
    // In case there was already a ServiceThrottlingBehavior
    // (this.throttlingBehavior==null), it should have initialized
    // a single ServiceThrottle on all ChannelDispatchers.
    // As we loop through the ChannelDispatchers, we verify that
    // and modify the ServiceThrottle to guard MaxPoolSize.  
    ServiceThrottle throttle = null;  
  
    foreach (ChannelDispatcherBase cdb in
            serviceHostBase.ChannelDispatchers)  
    {  
        ChannelDispatcher cd = cdb as ChannelDispatcher;  
        if (cd != null)  
        {  
            // Make sure there is exactly one throttle used by all
            // endpoints. If there were others, we could not enforce
            // MaxPoolSize.  
            if ((this.throttlingBehavior == null) &&
                        (this.maxPoolSize != Int32.MaxValue))  
            {  
                throttle ??= cd.ServiceThrottle;
                if (cd.ServiceThrottle == null)  
                {  
                    throw new
InvalidOperationException(ResourceHelper.GetString("ExNullThrottle"));  
                }  
                if (throttle != cd.ServiceThrottle)  
                {  
                    throw new InvalidOperationException(ResourceHelper.GetString("ExDifferentThrottle"));  
                }  
             }  
  
             foreach (EndpointDispatcher ed in cd.Endpoints)  
             {  
                 // Assign it to DispatchBehavior in each endpoint.  
                 ed.DispatchRuntime.InstanceProvider =
                                      instanceProvider;  
             }  
         }  
     }  
  
     // Set the MaxConcurrentInstances to limit the number of items
     // that will ever be requested from the pool.  
     if ((throttle != null) && (throttle.MaxConcurrentInstances >
                                      this.maxPoolSize))  
     {  
         throttle.MaxConcurrentInstances = this.maxPoolSize;  
     }  
}  
```  
  
 除了 <xref:System.ServiceModel.Description.IServiceBehavior> 实现外，<xref:System.EnterpriseServices.ObjectPoolingAttribute> 类有多个可以使用属性自变量自定义对象池的成员。 这些成员包括 <xref:System.EnterpriseServices.ObjectPoolingAttribute.MaxPoolSize%2A>、<xref:System.EnterpriseServices.ObjectPoolingAttribute.MinPoolSize%2A> 和 <xref:System.EnterpriseServices.ObjectPoolingAttribute.CreationTimeout%2A>，用于匹配由 .NET 企业服务提供的对象池功能集。  
  
 现在可以通过使用新创建的自定义特性对服务实现进行批注，将对象池行为添加到 WCF 服务 `ObjectPooling` 。  
  
```csharp  
[ObjectPooling(MaxPoolSize=1024, MinPoolSize=10, CreationTimeout=30000)]
public class PoolService : IPoolService  
{  
  // …  
}  
```  
  
## <a name="running-the-sample"></a>运行示例  

 本实例演示在某些方案中使用对象池可以获得的性能优点。  
  
 服务应用程序实现两个服务 -- `WorkService` 和 `ObjectPooledWorkService`。 这两个服务共享相同的实现 -- 它们都需要成本较高的初始化，再公开成本相对低廉的 `DoWork()` 方法。 唯一的区别是 `ObjectPooledWorkService` 配置了对象池：  
  
```csharp  
[ObjectPooling(MinPoolSize = 0, MaxPoolSize = 5)]  
public class ObjectPooledWorkService : IDoWork  
{  
    public ObjectPooledWorkService()  
    {  
        Thread.Sleep(5000);  
        ColorConsole.WriteLine(ConsoleColor.Blue, "ObjectPooledWorkService instance created.");  
    }  
  
    public void DoWork()  
    {  
        ColorConsole.WriteLine(ConsoleColor.Blue, "ObjectPooledWorkService.GetData() completed.");  
    }
}  
```  
  
 当运行客户端时，它定时调用 `WorkService` 5 次。 接着定时调用 `ObjectPooledWorkService` 5 次。 然后显示时间上的区别：  
  
```console
Press <ENTER> to start the client.  
  
Calling WorkService:  
1 - DoWork() Done  
2 - DoWork() Done  
3 - DoWork() Done  
4 - DoWork() Done  
5 - DoWork() Done  
Calling WorkService took: 26722 ms.  
Calling ObjectPooledWorkService:  
1 - DoWork() Done  
2 - DoWork() Done  
3 - DoWork() Done  
4 - DoWork() Done  
5 - DoWork() Done  
Calling ObjectPooledWorkService took: 5323 ms.  
Press <ENTER> to exit.  
```  
  
> [!NOTE]
> 第一次运行客户端时，这两个服务看起来占用相等的时间。 如果重新运行此示例，则将看到 `ObjectPooledWorkService` 的返回速度更快，原因是池中已经存在该对象的实例了。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!NOTE]
> 如果使用 Svcutil.exe 为此示例重新生成配置，请确保在客户端配置中修改终结点名称以与客户端代码匹配。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Pooling`  
