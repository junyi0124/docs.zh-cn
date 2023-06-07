---
title: 中间层客户端应用程序
ms.date: 03/30/2017
ms.assetid: f9714a64-d0ae-4a98-bca0-5d370fdbd631
ms.openlocfilehash: 5019215567f4c9127f2e53fd4cdf0d4a67b84d17
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248248"
---
# <a name="middle-tier-client-applications"></a>中间层客户端应用程序

本主题讨论了使用 Windows Communication Foundation (WCF) 的中间层客户端应用程序的各种问题。  
  
## <a name="increasing-middle-tier-client-performance"></a>提高中间层客户端应用程序的性能  

 与以前的通信技术（如使用 ASP.NET 的 Web 服务）相比，创建 WCF 客户端实例可能更为复杂，这是因为 WCF 的丰富功能集。 例如，打开 <xref:System.ServiceModel.ChannelFactory%601> 对象时，该对象会与服务建立一个安全会话，该过程将增加客户端实例的启动时间。 通常，这些附加功能不会对客户端应用程序产生很大影响，因为 WCF 客户端发出多个调用，然后关闭。  
  
 但是，中间层客户端应用程序可以快速地创建许多 WCF 客户端对象，因此，体验会提高初始化需求。 调用服务时，有两种主要的方法可以提高中间层应用程序的性能：  
  
- 缓存 WCF 客户端对象，并在可能的情况下重用它。  
  
- 创建一个 <xref:System.ServiceModel.ChannelFactory%601> 对象，然后使用该对象为每个调用创建新的 WCF 客户端通道对象。  
  
 使用这些方法时要考虑的问题包括：  
  
- 如果服务使用会话维护客户端特定的状态，则无法将中间层 WCF 客户端与多客户端层请求一起使用，因为该服务的状态与中间层客户端的状态关联。  
  
- 如果服务必须基于每个客户端执行身份验证，则必须在中间层为每个传入请求创建一个新的客户端，而不是重复使用中间层 WCF 客户端 (或 WCF 客户端信道对象) ，因为在创建 WCF 客户端后，不能修改中间层的客户端凭据 (或 <xref:System.ServiceModel.ChannelFactory%601>) 。  
  
- 由于通道及其创建的客户端是线程安全的，因此它们可能不支持同时向网络中写入多条消息。 如果要发送较大的消息，特别是消息流，则发送操作可能会阻塞，因为它要等待另一个发送操作完成。 这会导致两种问题：缺少并发性；当控制流返回到重用信道的服务（即，共享的客户端调用了一种服务，该服务的代码路径会导致对共享的客户端进行回调）时，存在死锁的可能性。 不管重用的 WCF 客户端的类型如何都是如此。  
  
- 无论您是否共享出错的通道，您都必须处理它。 然而，重用通道时，出错通道可以关闭多个挂起的请求或发送。  
  
 有关为多个请求重复使用客户端的最佳实践的示例，请参阅 [ASP.NET 客户端中的数据绑定](../samples/data-binding-in-an-aspnet-client.md)。  
  
 此外，如果客户端使用可用 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化的数据类型，则会在运行时生成并编译这些数据类型的序列化代码，从而导致启动性能降低；但你可以提高那些客户端的启动性能。 通过从应用程序的编译的程序集生成必要的序列化代码， [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 可提高这些应用程序的启动性能。 有关详细信息，请参阅 [如何：使用 XmlSerializer 改善 WCF 客户端应用程序的启动时间](startup-time-of-wcf-client-applications-using-the-xmlserializer.md)。  
  
## <a name="see-also"></a>另请参阅

- [使用 WCF 客户端访问服务](accessing-services-using-a-client.md)
