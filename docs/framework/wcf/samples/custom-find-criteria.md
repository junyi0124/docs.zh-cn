---
title: 自定义查找条件
ms.date: 03/30/2017
ms.assetid: b2723929-8829-424d-8015-a37ba2ab4f68
ms.openlocfilehash: 9271ae1ec4bbd555fe93df24c7d38f0f345a03ab
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96241806"
---
# <a name="custom-find-criteria"></a>自定义查找条件

本示例演示如何使用逻辑来创建自定义范围匹配以及如何实现自定义发现服务。 客户端使用自定义范围匹配功能在系统提供的 WCF Discovery 查找功能基础之上进行优化并进一步生成。 本示例包含的方案如下所示：  
  
1. 客户端查找计算器服务。  
  
2. 若要优化其搜索，客户端必须使用自定义范围匹配规则。  
  
3. 根据此规则，如果服务的终结点与客户端指定的任何范围匹配，则服务响应回客户端。  
  
## <a name="demonstrates"></a>演示  
  
- 创建自定义发现服务。  
  
- 通过算法实现自定义范围匹配。  
  
## <a name="discussion"></a>讨论 (Discussion)  

 客户端查找 "或" 类型匹配条件。 如果服务终结点上的范围与客户端提供的任何范围匹配，则服务响应回客户端。 在本例中，客户端查找具有下面列表中任何范围的计算器服务：  
  
1. `net.tcp://Microsoft.Samples.Discovery/RedmondLocation`  
  
2. `net.tcp://Microsoft.Samples.Discovery/SeattleLocation`  
  
3. `net.tcp://Microsoft.Samples.Discovery/PortlandLocation`  
  
 为实现此功能，客户端指示服务通过按 URI 传入自定义范围匹配来使用自定义范围匹配规则。 为便于进行自定义范围匹配，服务必须使用了解自定义范围匹配规则并实现关联匹配逻辑的自定义发现服务。  
  
 在客户端项目中，打开 Program.cs 文件。 请注意，`ScopeMatchBy` 对象的 `FindCriteria` 字段设置为特定 URI。 此标识符会发送给服务。 如果服务不了解此规则，则会忽略客户端的查找请求。  
  
 打开服务项目。 有三个文件用于实现自定义发现服务：  
  
1. **AsyncResult.cs**：这是 `AsyncResult` 发现方法所需的的实现。  
  
2. **CustomDiscoveryService.cs**：此文件实现自定义发现服务。 该实现扩展 <xref:System.ServiceModel.Discovery.DiscoveryService> 类并重写必需的方法。 请注意 <xref:System.ServiceModel.Discovery.DiscoveryService.OnBeginFind%2A> 方法的实现。 该方法进行检查以确定客户端是否指定了按规则进行的自定义范围匹配。 这与客户端之前指定的自定义 URI 相同。 如果指定了自定义规则，则遵循实现 "OR" 匹配逻辑的代码路径。  
  
     此自定义逻辑遍历服务具有的每个终结点上的所有范围。 如果有任何终结点的范围与客户端提供的任何范围匹配，则发现服务会将该终结点添加到发送回客户端的响应中。  
  
3. **CustomDiscoveryExtension.cs**：实现发现服务的最后一步是将此自定义发现服务的实现连接到服务主机。 此处使用的帮助程序类是 `CustomDiscoveryExtension` 类。 此类扩展 <xref:System.ServiceModel.Discovery.DiscoveryServiceExtension> 类。 用户必须重写 <xref:System.ServiceModel.Discovery.DiscoveryServiceExtension.GetDiscoveryService%2A> 方法。 在这种情况中，方法返回之前创建的自定义发现服务的实例。 `PublishedEndpoints` 是 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601>，它包含要添加到 <xref:System.ServiceModel.ServiceHost> 的所有应用程序终结点。 自定义发现服务用此填充其内部列表。 用户也可以添加其他终结点元数据。  
  
 最后，打开 Program.cs。 请注意，<xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> 和 `CustomDiscoveryExtension` 都添加到主机中。 在此操作完成，并且主机具有可用于接收发现消息的终结点后，应用程序便可以使用自定义发现服务。  
  
 观察客户端是否能够在不知道服务地址的情况下找到服务。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 打开包含项目的解决方案。  
  
2. 生成项目。  
  
3. 运行服务应用程序。  
  
4. 运行客户端应用程序。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\CustomFindCriteria`
