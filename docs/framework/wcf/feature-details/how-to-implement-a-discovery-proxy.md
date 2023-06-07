---
title: 如何：实现发现代理
ms.date: 03/30/2017
ms.assetid: 78d70e0a-f6c3-4cfb-a7ca-f66ebddadde0
ms.openlocfilehash: ca7ab2ee434aef7649d71cbfc33273f48020788f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597067"
---
# <a name="how-to-implement-a-discovery-proxy"></a>如何：实现发现代理

本主题介绍如何实现发现代理。 有关 Windows Communication Foundation （WCF）中的发现功能的详细信息，请参阅[WCF 发现概述](wcf-discovery-overview.md)。 可以通过创建一个扩展 <xref:System.ServiceModel.Discovery.DiscoveryProxy> 抽象类的类来实现发现代理。 此示例中定义并使用了多个其他支持类。 `OnResolveAsyncResult`、 `OnFindAsyncResult`和 `AsyncResult`。 这些类实现 <xref:System.IAsyncResult> 接口。 有关详细信息， <xref:System.IAsyncResult> 请参阅 system.exception[接口](xref:System.IAsyncResult)。

 本主题分三个主要部分来讨论如何实现发现代理：

- 定义一个包含数据存储并扩展抽象 <xref:System.ServiceModel.Discovery.DiscoveryProxy> 类的类。

- 实现帮助器 `AsyncResult` 类。

- 承载发现代理。

### <a name="to-create-a-new-console-application-project"></a>创建新的控制台应用程序项目

1. 启动 Visual Studio 2012。

2. 创建新的控制台应用程序项目。 将项目命名为 `DiscoveryProxy`，并将解决方案命名为 `DiscoveryProxyExample`。

3. 添加对项目的以下引用

    1. System.ServiceModel.dll

    2. System.Servicemodel.Discovery.dll

    > [!CAUTION]
    > 确保引用这些程序集的版本 4.0 或更高版本。

### <a name="to-implement-the-proxydiscoveryservice-class"></a>实现 ProxyDiscoveryService 类

1. 向项目添加新代码文件并将其命名为 DiscoveryProxy.cs。

2. 将以下 `using` 语句添加到 DiscoveryProxy.cs。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.ServiceModel;
    using System.ServiceModel.Discovery;
    using System.Xml;
    ```

3. 从 `DiscoveryProxyService` 派生 <xref:System.ServiceModel.Discovery.DiscoveryProxy>。 将 `ServiceBehavior` 特性应用到下面的示例所示的类。

    ```csharp
    // Implement DiscoveryProxy by extending the DiscoveryProxy class and overriding the abstract methods
    [ServiceBehavior(InstanceContextMode = InstanceContextMode.Single, ConcurrencyMode = ConcurrencyMode.Multiple)]
    public class DiscoveryProxyService : DiscoveryProxy
    {
    }
    ```

4. 在 `DiscoveryProxy` 类内，定义一个字典以保存注册的服务。

    ```csharp
    // Repository to store EndpointDiscoveryMetadata.
    Dictionary<EndpointAddress, EndpointDiscoveryMetadata> onlineServices;
    ```

5. 定义一个初始化该字典的构造函数。

    ```csharp
    public DiscoveryProxyService()
            {
                this.onlineServices = new Dictionary<EndpointAddress, EndpointDiscoveryMetadata>();
            }
    ```

### <a name="to-define-the-methods-used-to-update-the-discovery-proxy-cache"></a>定义用于更新发现代理缓存的方法

1. 实现 `AddOnlineservice` 方法以向缓存中添加服务。 每次代理接收到公告消息时，都会调用此方法。

    ```csharp
    void AddOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)
    {
        lock (this.onlineServices)
        {
            this.onlineServices[endpointDiscoveryMetadata.Address] = endpointDiscoveryMetadata;
        }

        PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Adding");
    }
    ```

2. 实现 `RemoveOnlineService` 方法，该方法用于从缓存删除服务。

    ```csharp
    void RemoveOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)
    {
        if (endpointDiscoveryMetadata != null)
        {
            lock (this.onlineServices)
            {
                this.onlineServices.Remove(endpointDiscoveryMetadata.Address);
            }

            PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Removing");
        }
    }
    ```

3. 实现 `MatchFromOnlineService` 方法，这些方法尝试将一个服务与字典中的服务相匹配。

    ```csharp
    void MatchFromOnlineService(FindRequestContext findRequestContext)
    {
        lock (this.onlineServices)
        {
            foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)
            {
                if (findRequestContext.Criteria.IsMatch(endpointDiscoveryMetadata))
                {
                    findRequestContext.AddMatchingEndpoint(endpointDiscoveryMetadata);
                }
            }
        }
    }
    ```

    ```csharp
    EndpointDiscoveryMetadata MatchFromOnlineService(ResolveCriteria criteria)
    {
        EndpointDiscoveryMetadata matchingEndpoint = null;
        lock (this.onlineServices)
        {
            foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)
            {
                if (criteria.Address == endpointDiscoveryMetadata.Address)
                {
                    matchingEndpoint = endpointDiscoveryMetadata;
                }
            }
        }
        return matchingEndpoint;
    }
    ```

4. 实现 `PrintDiscoveryMetadata` 方法，该方法为用户提供发现代理所执行的操作的控制台文本输出。

    ```csharp
    void PrintDiscoveryMetadata(EndpointDiscoveryMetadata endpointDiscoveryMetadata, string verb)
    {
        Console.WriteLine("\n**** " + verb + " service of the following type from cache. ");
        foreach (XmlQualifiedName contractName in endpointDiscoveryMetadata.ContractTypeNames)
        {
            Console.WriteLine("** " + contractName.ToString());
            break;
        }
        Console.WriteLine("**** Operation Completed");
    }
    ```

5. 将以下 AsyncResult 类添加到 DiscoveryProxyService。 这些类用于区分不同的异步操作结果。

    ```csharp
    sealed class OnOnlineAnnouncementAsyncResult : AsyncResult
    {
        public OnOnlineAnnouncementAsyncResult(AsyncCallback callback, object state)
            : base(callback, state)
        {
            this.Complete(true);
        }

        public static void End(IAsyncResult result)
        {
            AsyncResult.End<OnOnlineAnnouncementAsyncResult>(result);
        }
    }

    sealed class OnOfflineAnnouncementAsyncResult : AsyncResult
    {
        public OnOfflineAnnouncementAsyncResult(AsyncCallback callback, object state)
            : base(callback, state)
        {
            this.Complete(true);
        }

        public static void End(IAsyncResult result)
        {
            AsyncResult.End<OnOfflineAnnouncementAsyncResult>(result);
        }
    }

    sealed class OnFindAsyncResult : AsyncResult
    {
        public OnFindAsyncResult(AsyncCallback callback, object state)
            : base(callback, state)
        {
            this.Complete(true);
        }

        public static void End(IAsyncResult result)
        {
            AsyncResult.End<OnFindAsyncResult>(result);
        }
    }

    sealed class OnResolveAsyncResult : AsyncResult
    {
        EndpointDiscoveryMetadata matchingEndpoint;

        public OnResolveAsyncResult(EndpointDiscoveryMetadata matchingEndpoint, AsyncCallback callback, object state)
            : base(callback, state)
        {
            this.matchingEndpoint = matchingEndpoint;
            this.Complete(true);
        }

        public static EndpointDiscoveryMetadata End(IAsyncResult result)
        {
            OnResolveAsyncResult thisPtr = AsyncResult.End<OnResolveAsyncResult>(result);
            return thisPtr.matchingEndpoint;
        }
    }
    ```

### <a name="to-define-the-methods-that-implement-the-discovery-proxy-functionality"></a>定义实现发现代理功能的方法

1. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnBeginOnlineAnnouncement%2A?displayProperty=nameWithType> 方法。 当发现代理接收到联机公告消息时，将调用此方法。

    ```csharp
    // OnBeginOnlineAnnouncement method is called when a Hello message is received by the Proxy
    protected override IAsyncResult OnBeginOnlineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)
    {
        this.AddOnlineService(endpointDiscoveryMetadata);
        return new OnOnlineAnnouncementAsyncResult(callback, state);
    }
    ```

2. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnEndOnlineAnnouncement%2A?displayProperty=nameWithType> 方法。 当发现代理完成处理公告消息时，将调用此方法。

    ```csharp
    protected override void OnEndOnlineAnnouncement(IAsyncResult result)
    {
        OnOnlineAnnouncementAsyncResult.End(result);
    }
    ```

3. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnBeginOfflineAnnouncement%2A?displayProperty=nameWithType> 方法。 当发现代理接收到脱机公告消息时，将调用此方法。

    ```csharp
    // OnBeginOfflineAnnouncement method is called when a Bye message is received by the Proxy
    protected override IAsyncResult OnBeginOfflineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)
    {
        this.RemoveOnlineService(endpointDiscoveryMetadata);
        return new OnOfflineAnnouncementAsyncResult(callback, state);
    }
    ```

4. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnEndOfflineAnnouncement%2A?displayProperty=nameWithType> 方法。 当发现代理完成处理脱机公告消息时，将调用此方法。

    ```csharp
    protected override void OnEndOfflineAnnouncement(IAsyncResult result)
    {
        OnOfflineAnnouncementAsyncResult.End(result);
    }
    ```

5. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnBeginFind%2A?displayProperty=nameWithType> 方法。 当发现代理接收到查找请求时，将调用此方法。

    ```csharp
    // OnBeginFind method is called when a Probe request message is received by the Proxy
    protected override IAsyncResult OnBeginFind(FindRequestContext findRequestContext, AsyncCallback callback, object state)
    {
        this.MatchFromOnlineService(findRequestContext);
        return new OnFindAsyncResult(callback, state);
    }
    protected override IAsyncResult OnBeginFind(FindRequest findRequest, AsyncCallback callback, object state)
    {
        Collection<EndpointDiscoveryMetadata> matchingEndpoints = MatchFromCache(findRequest.Criteria);
        return new OnFindAsyncResult(
                    matchingEndpoints,
                    callback,
                    state);
    }
    ```

6. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnEndFind%2A?displayProperty=nameWithType> 方法。 当发现代理完成查找请求的处理时，将调用此方法。

    ```csharp
    protected override void OnEndFind(IAsyncResult result)
    {
        OnFindAsyncResult.End(result);
    }
    ```

7. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnBeginResolve%2A?displayProperty=nameWithType> 方法。 当发现代理接收到解决消息时，将调用此方法。

    ```csharp
    // OnBeginFind method is called when a Resolve request message is received by the Proxy
    protected override IAsyncResult OnBeginResolve(ResolveCriteria resolveCriteria, AsyncCallback callback, object state)
    {
        return new OnResolveAsyncResult(this.MatchFromOnlineService(resolveCriteria), callback, state);
    }
    protected override IAsyncResult OnBeginResolve(ResolveRequest resolveRequest, AsyncCallback callback, object state)
    {
        return new OnResolveAsyncResult(
            this.proxy.MatchFromOnlineService(resolveRequest.Criteria),
            callback,
            state);
    }
    ```

8. 重写 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnEndResolve%2A?displayProperty=nameWithType> 方法。 当发现代理完成处理解决消息时，将调用此方法。

    ```csharp
    protected override EndpointDiscoveryMetadata OnEndResolve(IAsyncResult result)
    {
        return OnResolveAsyncResult.End(result);
    }
    ```

OnBegin. / OnEnd. 方法提供后续发现操作的逻辑。 例如，<xref:System.ServiceModel.Discovery.DiscoveryProxy.OnBeginFind%2A> 和 <xref:System.ServiceModel.Discovery.DiscoveryProxy.OnEndFind%2A> 方法实现发现代理的查找逻辑。 当发现代理接收到探测消息时，将执行这些方法以便向客户端回发响应。 您可以根据需要修改查找逻辑，例如，可以将按算法实现的自定义范围匹配或应用程序特定 XML 元数据分析合并到查找操作中。

### <a name="to-implement-the-asyncresult-class"></a>实现 AsyncResult 类

1. 定义用于派生各种异步结果类的抽象基类 AsyncResult。

2. 创建一个名为 AsyncResult.cs 的新代码文件。

3. 将以下 `using` 语句添加到 AsyncResult.cs。

    ```csharp
    using System;
    using System.Threading;
    ```

4. 添加以下 AsyncResult 类。

    ```csharp
    abstract class AsyncResult : IAsyncResult
    {
        AsyncCallback callback;
        bool completedSynchronously;
        bool endCalled;
        Exception exception;
        bool isCompleted;
        ManualResetEvent manualResetEvent;
        object state;
        object thisLock;

        protected AsyncResult(AsyncCallback callback, object state)
        {
            this.callback = callback;
            this.state = state;
            this.thisLock = new object();
        }

        public object AsyncState
        {
            get
            {
                return state;
            }
        }

        public WaitHandle AsyncWaitHandle
        {
            get
            {
                if (manualResetEvent != null)
                {
                    return manualResetEvent;
                }
                lock (ThisLock)
                {
                    manualResetEvent ??= new ManualResetEvent(isCompleted);
                }
                return manualResetEvent;
            }
        }

        public bool CompletedSynchronously
        {
            get
            {
                return completedSynchronously;
            }
        }

        public bool IsCompleted
        {
            get
            {
                return isCompleted;
            }
        }

        object ThisLock
        {
            get
            {
                return this.thisLock;
            }
        }

        protected static TAsyncResult End<TAsyncResult>(IAsyncResult result)
            where TAsyncResult : AsyncResult
        {
            if (result == null)
            {
                throw new ArgumentNullException("result");
            }

            TAsyncResult asyncResult = result as TAsyncResult;

            if (asyncResult == null)
            {
                throw new ArgumentException("Invalid async result.", "result");
            }

            if (asyncResult.endCalled)
            {
                throw new InvalidOperationException("Async object already ended.");
            }

            asyncResult.endCalled = true;

            if (!asyncResult.isCompleted)
            {
                asyncResult.AsyncWaitHandle.WaitOne();
            }

            if (asyncResult.manualResetEvent != null)
            {
                asyncResult.manualResetEvent.Close();
            }

            if (asyncResult.exception != null)
            {
                throw asyncResult.exception;
            }

            return asyncResult;
        }

        protected void Complete(bool completedSynchronously)
        {
            if (isCompleted)
            {
                throw new InvalidOperationException("This async result is already completed.");
            }

            this.completedSynchronously = completedSynchronously;

            if (completedSynchronously)
            {
                this.isCompleted = true;
            }
            else
            {
                lock (ThisLock)
                {
                    this.isCompleted = true;
                    if (this.manualResetEvent != null)
                    {
                        this.manualResetEvent.Set();
                    }
                }
            }

            if (callback != null)
            {
                callback(this);
            }
        }

        protected void Complete(bool completedSynchronously, Exception exception)
        {
            this.exception = exception;
            Complete(completedSynchronously);
        }
    }
    ```

### <a name="to-host-the-discoveryproxy"></a>承载 DiscoveryProxy

1. 打开 DiscoveryProxyExample 项目中的 Program.cs 文件。

2. 添加以下 `using` 语句。

    ```csharp
    using System;
    using System.ServiceModel;
    using System.ServiceModel.Discovery;
    ```

3. 在 `Main()` 方法中，添加下面的代码。 这将创建 `DiscoveryProxy` 类的一个实例。

    ```csharp
    Uri probeEndpointAddress = new Uri("net.tcp://localhost:8001/Probe");
    Uri announcementEndpointAddress = new Uri("net.tcp://localhost:9021/Announcement");

    // Host the DiscoveryProxy service
    ServiceHost proxyServiceHost = new ServiceHost(new DiscoveryProxyService());
    ```

4. 接下来，将以下代码添加到发现终结点和公告终结点。

    ```csharp
    try
    {
        // Add DiscoveryEndpoint to receive Probe and Resolve messages
        DiscoveryEndpoint discoveryEndpoint = new DiscoveryEndpoint(new NetTcpBinding(), new EndpointAddress(probeEndpointAddress));
        discoveryEndpoint.IsSystemEndpoint = false;

        // Add AnnouncementEndpoint to receive Hello and Bye announcement messages
        AnnouncementEndpoint announcementEndpoint = new AnnouncementEndpoint(new NetTcpBinding(), new EndpointAddress(announcementEndpointAddress));

        proxyServiceHost.AddServiceEndpoint(discoveryEndpoint);
        proxyServiceHost.AddServiceEndpoint(announcementEndpoint);

        proxyServiceHost.Open();

        Console.WriteLine("Proxy Service started.");
        Console.WriteLine();
        Console.WriteLine("Press <ENTER> to terminate the service.");
        Console.WriteLine();
        Console.ReadLine();

        proxyServiceHost.Close();
    }
    catch (CommunicationException e)
    {
        Console.WriteLine(e.Message);
    }
    catch (TimeoutException e)
    {
        Console.WriteLine(e.Message);
    }

    if (proxyServiceHost.State != CommunicationState.Closed)
    {
        Console.WriteLine("Aborting the service...");
        proxyServiceHost.Abort();
    }
    ```

您已完成实现发现代理。 继续[操作如何：实现使用发现代理注册的可发现服务](discoverable-service-that-registers-with-the-discovery-proxy.md)。

## <a name="example"></a>示例

下面是本主题中使用的代码的完整清单。

```csharp
// DiscoveryProxy.cs
//----------------------------------------------------------------
// Copyright (c) Microsoft Corporation.  All rights reserved.
//----------------------------------------------------------------

using System;
using System.Collections.Generic;
using System.ServiceModel;
using System.ServiceModel.Discovery;
using System.Xml;

namespace Microsoft.Samples.Discovery
{
    // Implement DiscoveryProxy by extending the DiscoveryProxy class and overriding the abstract methods
    [ServiceBehavior(InstanceContextMode = InstanceContextMode.Single, ConcurrencyMode = ConcurrencyMode.Multiple)]
    public class DiscoveryProxyService : DiscoveryProxy
    {
        // Repository to store EndpointDiscoveryMetadata. A database or a flat file could also be used instead.
        Dictionary<EndpointAddress, EndpointDiscoveryMetadata> onlineServices;

        public DiscoveryProxyService()
        {
            this.onlineServices = new Dictionary<EndpointAddress, EndpointDiscoveryMetadata>();
        }

        // OnBeginOnlineAnnouncement method is called when a Hello message is received by the Proxy
        protected override IAsyncResult OnBeginOnlineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)
        {
            this.AddOnlineService(endpointDiscoveryMetadata);
            return new OnOnlineAnnouncementAsyncResult(callback, state);
        }

        protected override void OnEndOnlineAnnouncement(IAsyncResult result)
        {
            OnOnlineAnnouncementAsyncResult.End(result);
        }

        // OnBeginOfflineAnnouncement method is called when a Bye message is received by the Proxy
        protected override IAsyncResult OnBeginOfflineAnnouncement(DiscoveryMessageSequence messageSequence, EndpointDiscoveryMetadata endpointDiscoveryMetadata, AsyncCallback callback, object state)
        {
            this.RemoveOnlineService(endpointDiscoveryMetadata);
            return new OnOfflineAnnouncementAsyncResult(callback, state);
        }

        protected override void OnEndOfflineAnnouncement(IAsyncResult result)
        {
            OnOfflineAnnouncementAsyncResult.End(result);
        }

        // OnBeginFind method is called when a Probe request message is received by the Proxy
        protected override IAsyncResult OnBeginFind(FindRequestContext findRequestContext, AsyncCallback callback, object state)
        {
            this.MatchFromOnlineService(findRequestContext);
            return new OnFindAsyncResult(callback, state);
        }

        protected override void OnEndFind(IAsyncResult result)
        {
            OnFindAsyncResult.End(result);
        }

        // OnBeginFind method is called when a Resolve request message is received by the Proxy
        protected override IAsyncResult OnBeginResolve(ResolveCriteria resolveCriteria, AsyncCallback callback, object state)
        {
            return new OnResolveAsyncResult(this.MatchFromOnlineService(resolveCriteria), callback, state);
        }

        protected override EndpointDiscoveryMetadata OnEndResolve(IAsyncResult result)
        {
            return OnResolveAsyncResult.End(result);
        }

        // The following are helper methods required by the Proxy implementation
        void AddOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)
        {
            lock (this.onlineServices)
            {
                this.onlineServices[endpointDiscoveryMetadata.Address] = endpointDiscoveryMetadata;
            }

            PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Adding");
        }

        void RemoveOnlineService(EndpointDiscoveryMetadata endpointDiscoveryMetadata)
        {
            if (endpointDiscoveryMetadata != null)
            {
                lock (this.onlineServices)
                {
                    this.onlineServices.Remove(endpointDiscoveryMetadata.Address);
                }

                PrintDiscoveryMetadata(endpointDiscoveryMetadata, "Removing");
            }
        }

        void MatchFromOnlineService(FindRequestContext findRequestContext)
        {
            lock (this.onlineServices)
            {
                foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)
                {
                    if (findRequestContext.Criteria.IsMatch(endpointDiscoveryMetadata))
                    {
                        findRequestContext.AddMatchingEndpoint(endpointDiscoveryMetadata);
                    }
                }
            }
        }

        EndpointDiscoveryMetadata MatchFromOnlineService(ResolveCriteria criteria)
        {
            EndpointDiscoveryMetadata matchingEndpoint = null;
            lock (this.onlineServices)
            {
                foreach (EndpointDiscoveryMetadata endpointDiscoveryMetadata in this.onlineServices.Values)
                {
                    if (criteria.Address == endpointDiscoveryMetadata.Address)
                    {
                        matchingEndpoint = endpointDiscoveryMetadata;
                    }
                }
            }
            return matchingEndpoint;
        }

        void PrintDiscoveryMetadata(EndpointDiscoveryMetadata endpointDiscoveryMetadata, string verb)
        {
            Console.WriteLine("\n**** " + verb + " service of the following type from cache. ");
            foreach (XmlQualifiedName contractName in endpointDiscoveryMetadata.ContractTypeNames)
            {
                Console.WriteLine("** " + contractName.ToString());
                break;
            }
            Console.WriteLine("**** Operation Completed");
        }

        sealed class OnOnlineAnnouncementAsyncResult : AsyncResult
        {
            public OnOnlineAnnouncementAsyncResult(AsyncCallback callback, object state)
                : base(callback, state)
            {
                this.Complete(true);
            }

            public static void End(IAsyncResult result)
            {
                AsyncResult.End<OnOnlineAnnouncementAsyncResult>(result);
            }
        }

        sealed class OnOfflineAnnouncementAsyncResult : AsyncResult
        {
            public OnOfflineAnnouncementAsyncResult(AsyncCallback callback, object state)
                : base(callback, state)
            {
                this.Complete(true);
            }

            public static void End(IAsyncResult result)
            {
                AsyncResult.End<OnOfflineAnnouncementAsyncResult>(result);
            }
        }

        sealed class OnFindAsyncResult : AsyncResult
        {
            public OnFindAsyncResult(AsyncCallback callback, object state)
                : base(callback, state)
            {
                this.Complete(true);
            }

            public static void End(IAsyncResult result)
            {
                AsyncResult.End<OnFindAsyncResult>(result);
            }
        }

        sealed class OnResolveAsyncResult : AsyncResult
        {
            EndpointDiscoveryMetadata matchingEndpoint;

            public OnResolveAsyncResult(EndpointDiscoveryMetadata matchingEndpoint, AsyncCallback callback, object state)
                : base(callback, state)
            {
                this.matchingEndpoint = matchingEndpoint;
                this.Complete(true);
            }

            public static EndpointDiscoveryMetadata End(IAsyncResult result)
            {
                OnResolveAsyncResult thisPtr = AsyncResult.End<OnResolveAsyncResult>(result);
                return thisPtr.matchingEndpoint;
            }
        }
    }
}
```

```csharp
// AsyncResult.cs
//----------------------------------------------------------------
// Copyright (c) Microsoft Corporation.  All rights reserved.
//----------------------------------------------------------------

using System;
using System.Threading;

namespace Microsoft.Samples.Discovery
{
    abstract class AsyncResult : IAsyncResult
    {
        AsyncCallback callback;
        bool completedSynchronously;
        bool endCalled;
        Exception exception;
        bool isCompleted;
        ManualResetEvent manualResetEvent;
        object state;
        object thisLock;

        protected AsyncResult(AsyncCallback callback, object state)
        {
            this.callback = callback;
            this.state = state;
            this.thisLock = new object();
        }

        public object AsyncState
        {
            get
            {
                return state;
            }
        }

        public WaitHandle AsyncWaitHandle
        {
            get
            {
                if (manualResetEvent != null)
                {
                    return manualResetEvent;
                }
                lock (ThisLock)
                {
                    manualResetEvent ??= new ManualResetEvent(isCompleted);
                }
                return manualResetEvent;
            }
        }

        public bool CompletedSynchronously
        {
            get
            {
                return completedSynchronously;
            }
        }

        public bool IsCompleted
        {
            get
            {
                return isCompleted;
            }
        }

        object ThisLock
        {
            get
            {
                return this.thisLock;
            }
        }

        protected static TAsyncResult End<TAsyncResult>(IAsyncResult result)
            where TAsyncResult : AsyncResult
        {
            if (result == null)
            {
                throw new ArgumentNullException("result");
            }

            TAsyncResult asyncResult = result as TAsyncResult;

            if (asyncResult == null)
            {
                throw new ArgumentException("Invalid async result.", "result");
            }

            if (asyncResult.endCalled)
            {
                throw new InvalidOperationException("Async object already ended.");
            }

            asyncResult.endCalled = true;

            if (!asyncResult.isCompleted)
            {
                asyncResult.AsyncWaitHandle.WaitOne();
            }

            if (asyncResult.manualResetEvent != null)
            {
                asyncResult.manualResetEvent.Close();
            }

            if (asyncResult.exception != null)
            {
                throw asyncResult.exception;
            }

            return asyncResult;
        }

        protected void Complete(bool completedSynchronously)
        {
            if (isCompleted)
            {
                throw new InvalidOperationException("This async result is already completed.");
            }

            this.completedSynchronously = completedSynchronously;

            if (completedSynchronously)
            {
                this.isCompleted = true;
            }
            else
            {
                lock (ThisLock)
                {
                    this.isCompleted = true;
                    if (this.manualResetEvent != null)
                    {
                        this.manualResetEvent.Set();
                    }
                }
            }

            if (callback != null)
            {
                callback(this);
            }
        }

        protected void Complete(bool completedSynchronously, Exception exception)
        {
            this.exception = exception;
            Complete(completedSynchronously);
        }
    }
}
```

```csharp
// program.cs
//----------------------------------------------------------------
// Copyright (c) Microsoft Corporation.  All rights reserved.
//----------------------------------------------------------------

using System;
using System.ServiceModel;
using System.ServiceModel.Discovery;

namespace Microsoft.Samples.Discovery
{
    class Program
    {
        public static void Main()
        {
            Uri probeEndpointAddress = new Uri("net.tcp://localhost:8001/Probe");
            Uri announcementEndpointAddress = new Uri("net.tcp://localhost:9021/Announcement");

            // Host the DiscoveryProxy service
            ServiceHost proxyServiceHost = new ServiceHost(new DiscoveryProxyService());

            try
            {
                // Add DiscoveryEndpoint to receive Probe and Resolve messages
                DiscoveryEndpoint discoveryEndpoint = new DiscoveryEndpoint(new NetTcpBinding(), new EndpointAddress(probeEndpointAddress));
                discoveryEndpoint.IsSystemEndpoint = false;

                // Add AnnouncementEndpoint to receive Hello and Bye announcement messages
                AnnouncementEndpoint announcementEndpoint = new AnnouncementEndpoint(new NetTcpBinding(), new EndpointAddress(announcementEndpointAddress));

                proxyServiceHost.AddServiceEndpoint(discoveryEndpoint);
                proxyServiceHost.AddServiceEndpoint(announcementEndpoint);

                proxyServiceHost.Open();

                Console.WriteLine("Proxy Service started.");
                Console.WriteLine();
                Console.WriteLine("Press <ENTER> to terminate the service.");
                Console.WriteLine();
                Console.ReadLine();

                proxyServiceHost.Close();
            }
            catch (CommunicationException e)
            {
                Console.WriteLine(e.Message);
            }
            catch (TimeoutException e)
            {
                Console.WriteLine(e.Message);
            }

            if (proxyServiceHost.State != CommunicationState.Closed)
            {
                Console.WriteLine("Aborting the service...");
                proxyServiceHost.Abort();
            }
        }
    }
}
```

## <a name="see-also"></a>另请参阅

- [WCF Discovery 概述](wcf-discovery-overview.md)
- [如何：实现向发现代理注册的可发现的服务](discoverable-service-that-registers-with-the-discovery-proxy.md)
- [如何：实现使用发现代理查找服务的客户端应用程序](client-app-discovery-proxy-to-find-a-service.md)
- [如何：测试发现代理](how-to-test-the-discovery-proxy.md)
