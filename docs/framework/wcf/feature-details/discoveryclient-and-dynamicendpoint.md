---
title: DiscoveryClient 和 DynamicEndpoint
ms.date: 03/30/2017
ms.assetid: 7cd418f0-0eab-48d1-a493-7eb907867ec3
ms.openlocfilehash: 72e5beb1306b679cc3b546d77846b5d1e9bb8bb4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275006"
---
# <a name="discoveryclient-and-dynamicendpoint"></a>DiscoveryClient 和 DynamicEndpoint

<xref:System.ServiceModel.Discovery.DiscoveryClient> 和 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 是客户端上用来搜索服务的两个类。 <xref:System.ServiceModel.Discovery.DiscoveryClient> 为您提供与一组特定条件相匹配的服务列表，并允许您连接到这些服务。 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 执行相同的操作，另外会自动连接到所发现的服务之一。 可以将任何终结点转换为 <xref:System.ServiceModel.Discovery.DynamicEndpoint>，也可以在配置中添加搜索条件，这样，如果您需要在解决方案中执行发现操作，但不希望修改客户端逻辑，则 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 会非常有用 – 此时您只需修改终结点。 另一方面，<xref:System.ServiceModel.Discovery.DiscoveryClient> 可用于对您的搜索操作获得更精细的控制。 下面对每个类的用途和优势加以详细说明。  
  
## <a name="discoveryclient"></a>DiscoveryClient  

 <xref:System.ServiceModel.Discovery.DiscoveryClient> 定义同步和异步 Find 方法以及 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindCompleted> 和 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindProgressChanged> 事件。  它还定义了同步和异步 Resolve 方法以及 <xref:System.ServiceModel.Discovery.DiscoveryClient.ResolveCompleted> 事件。 使用 <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> 或 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindAsync%2A> 方法可以搜索服务。 这两个方法都接受 <xref:System.ServiceModel.Discovery.FindCriteria> 实例，该实例可用于指定协定类型名称、作用域、最大请求结果数和作用域匹配规则。 调用 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindCompleted> 方法时，可以使用 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindProgressChanged> 和 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindAsync%2A> 事件。 只要 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindProgressChanged> 从服务中收到响应，就会激发 <xref:System.ServiceModel.Discovery.DiscoveryClient>。 可使用该事件来显示指示查找操作进度的进度栏。 它还可用来在接收到查找响应时对这些响应进行操作。 查找操作完成时激发 <xref:System.ServiceModel.Discovery.DiscoveryClient.FindCompleted> 事件。 如果已接收到最大响应数或 <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A> 已结束，则可能发生此情况。 查找操作完成时，将在 <xref:System.ServiceModel.Discovery.FindResponse> 实例中返回结果。 <xref:System.ServiceModel.Discovery.FindResponse> 包含 <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> 的集合，该集合包含地址、协定类型名称、扩展、侦听 URI 和匹配的服务的作用域。 然后，您可以使用这些信息连接到匹配的服务并调用其中一个服务。 下面的示例演示了如何调用 (DiscoveryClient) 方法，并使用返回的元数据来调用找到的服务的方法：。 使用的好处 <xref:System.ServiceModel.Discovery.DiscoveryClient.Find(System.ServiceModel.Discovery.FindCriteria)> 是，你可以缓存找到的终结点列表并稍后使用这些终结点。 通过此缓存，您可以构建自定义逻辑来处理各种失败情况。  
  
```csharp
DiscoveryClient dc = new DiscoveryClient(new UdpDiscoveryEndpoint());  
  
FindCriteria criteria = new FindCriteria(typeof(ICalculatorService));  
FindResponse fr = dc.Find(criteria);  
  
if (fr.Endpoints.Count > 0)  
{  
   EndpointAddress ep = fr.Endpoints[0].Address;  
   CalculatorServiceClient client = new CalculatorServiceClient();  
  
    // Connect to the discovered service endpoint  
   client.Endpoint.Address = endpointAddress;  
   Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress);  
  
   double value1 = 100.00D;  
   double value2 = 15.99D;  
  
   // Call the Add service operation.  
   double result = client.Add(value1, value2);  
   Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
}  
else  
   Console.WriteLine("No matching endpoints found");  
```  
  
 下面的示例演示如何采用异步方式执行查找操作。  
  
```csharp
static void FindServiceAsync()  
{  
   DiscoveryClient dc = new DiscoveryClient(new UdpDiscoveryEndpoint());
   dc.FindCompleted += new EventHandler<FindCompletedEventArgs>( discoveryClient_FindCompleted);  
   dc.FindProgressChanged += new EventHandler<FindProgressChangedEventArgs>(discoveryClient_FindProgressChanged);  
   dc.FindAsync(new FindCriteria(typeof(ICalculatorService)));
}
static void discoveryClient_FindProgressChanged(object sender, FindProgressChangedEventArgs e)  
{  
   Console.WriteLine("Found service at: " + e.EndpointDiscoveryMetadata.Address  
}
  
static void discoveryClient_FindCompleted(object sender, FindCompletedEventArgs e)  
{
      if (e.Result.Endpoints.Count > 0)  
            {  
                EndpointAddress ep = e.Result.Endpoints[0].Address;  
                CalculatorServiceClient client = new CalculatorServiceClient();  
  
                // Connect to the discovered service endpoint  
                client.Endpoint.Address = ep;  
                Console.WriteLine("Invoking CalculatorService at {0}", ep);  
  
                double value1 = 100.00D;  
                double value2 = 15.99D;  
  
                double result = client.Add(value1, value2);  
                Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
            }  
            else  
                Console.WriteLine("No matching endpoints found");  
  
        }  
```
  
 使用 <xref:System.ServiceModel.Discovery.DiscoveryClient.Resolve%2A> 和 <xref:System.ServiceModel.Discovery.DiscoveryClient.ResolveAsync%28System.ServiceModel.Discovery.ResolveCriteria%29> 方法可以查找基于其终结点地址的服务。 如果终结点地址不是网络可寻址地址，则此操作非常有用。 Resolve 方法接受 <xref:System.ServiceModel.Discovery.ResolveCriteria> 的实例，该实例可用于指定要解析的服务终结点地址、解析操作的最大持续时间和一组扩展。 下面的示例演示如何使用 <xref:System.ServiceModel.Discovery.DiscoveryClient.Resolve%2A> 方法来解析服务。  
  
```csharp  
DiscoveryClient dc = new DiscoveryClient(new UdpDiscoveryEndpoint());  
ResolveCriteria criteria = new ResolveCriteria(endpointAddress);  
ResolveResponse response = dc.Resolve(criteria);  
EndpointAddress newEp = response.EndpointDiscoveryMetadata.Address;  
```  
  
## <a name="dynamicendpoint"></a>DynamicEndpoint  

 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 是一个标准终结点 (有关详细信息，请参阅用于执行发现的 [标准终结点](standard-endpoints.md)) ，并自动选择匹配的服务。 只需要创建一个传入要搜索的协定和要使用的绑定的 <xref:System.ServiceModel.Discovery.DynamicEndpoint>，然后将 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 实例传递给 WCF 客户端。 下面的示例演示如何创建并使用 <xref:System.ServiceModel.Discovery.DynamicEndpoint> 来调用计算器服务。 每次打开客户端时都会执行发现操作。 <xref:System.ServiceModel.Discovery.DynamicEndpoint>通过将 `kind ="dynamicEndpoint"` 属性添加到终结点配置元素，还可以将配置中定义的任何终结点转换为。  
  
```csharp  
DynamicEndpoint dynamicEndpoint = new DynamicEndpoint(ContractDescription.GetContract(typeof(ICalculatorService)), new WSHttpBinding());  
CalculatorServiceClient client = new CalculatorServiceClient(dynamicEndpoint);  
  
Console.WriteLine("Invoking CalculatorService");  
Console.WriteLine();  
  
double value1 = 100.00D;  
double value2 = 15.99D;  
  
double result = client.Add(value1, value2);  
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
```  
  
## <a name="see-also"></a>另请参阅

- [通过范围进行发现](../samples/discovery-with-scopes-sample.md)
- [基本](../samples/basic-sample.md)
