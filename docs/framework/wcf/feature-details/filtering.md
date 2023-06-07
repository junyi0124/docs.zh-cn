---
title: 筛选
ms.date: 03/30/2017
ms.assetid: 4002946c-e34a-4356-8cfb-e25912a4be63
ms.openlocfilehash: cc95852a5eb191befe7e3cd563939c340ef16e06
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268477"
---
# <a name="filtering"></a>筛选

Windows Communication Foundation (WCF) 筛选系统可以使用声明性筛选器来匹配消息并做出操作决策。 使用筛选器，可以通过检查消息的某个部分来确定如何处理消息。 例如，查询过程可以使用 XPath 1.0 查询来检查已知标头的优先级元素，以确定是否将消息移动到队列的靠前位置。  
  
 筛选系统由一组类组成，这些类可以有效地确定一组筛选器中哪个 `true` 特定的 WCF 消息。  
  
 筛选系统是 WCF 消息传递的核心组件;它的设计速度非常快。 每个筛选器实现已针对特定类型的 WCF 消息进行了优化。  
  
 筛选系统不是线程安全的。 应用程序必须处理所有锁定语义。 但是，筛选系统的确支持多读取器、单编写器操作模式。  
  
## <a name="where-filtering-fits"></a>筛选的适用范围  

 筛选在接收到消息之后执行，并且是将消息调度到正确的应用程序组件这一过程的一部分。 筛选系统的设计满足了多个 WCF 子系统的要求，包括消息传递、路由、安全、事件处理和系统管理。  
  
## <a name="filters"></a>筛选器  

 筛选器引擎具有两个主要组件，即筛选器和筛选器表。 筛选器基于用户指定的逻辑条件做出有关消息的布尔逻辑判定。 筛选器实现 <xref:System.ServiceModel.Dispatcher.MessageFilter> 类。  
  
 <xref:System.ServiceModel.Dispatcher.MessageFilter.Match%2A> 方法用于确定消息是否满足筛选器的条件。 其中一个方法测试消息头，但无法检查消息正文。 另一种方法采用 *消息缓冲区* 作为输入参数，并且可以检查消息正文。  
  
 筛选器通常不单独进行测试，而是作为筛选器表（它是 <xref:System.ServiceModel.Dispatcher.MessageFilterTable%601.CreateFilterTable%2A> 方法创建的一个泛型类）的一部分进行测试。  
  
 每种筛选器都专用于根据特定种类的布尔条件进行匹配。 一旦构造了一个筛选器，就无法更改筛选器使用的条件；若要修改筛选器的条件，需要构造一个新的筛选器并删除现有的筛选器。  
  
### <a name="action-filters"></a>操作筛选器  

 <xref:System.ServiceModel.Dispatcher.ActionMessageFilter> 包含一个操作字符串列表。 如果筛选器的列表中的任一操作与消息或消息缓冲区中的 Action 标头匹配，则 `Match` 方法返回 `true`。 如果该列表为空，则将该筛选器视为全匹配型筛选器，任何消息或消息缓冲区都与该筛选器匹配，并且 `Match` 返回 `true`。 如果筛选器列表中没有任何操作与消息或消息缓冲区中的 Action 标头匹配，则 `Match` 返回 `false`。 如果消息中不存在任何操作且筛选器的列表非空，则 `Match` 返回 `false`。  
  
### <a name="endpoint-address-filters"></a>终结点地址筛选器  

 <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> 基于终结点地址筛选消息和消息缓冲区（终结点地址在消息和消息缓冲区的标头集合中表示）。 消息要想通过这样的筛选器，必须满足下列条件：  
  
- 筛选器的地址统一资源标识符 (URI) 必须与消息 To 标头中的统一资源标识符相同。  
  
- 筛选器地址中的每个终结点参数（`address.Headers` 集合）都必须在消息中找到一个与之匹配的标头。 消息或消息缓冲区中的额外标头可使匹配结果保持为 `true`。  
  
### <a name="prefix-endpoint-address-filters"></a>前缀终结点地址筛选器  
  
1. <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> 的工作方式与 <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> 筛选器非常类似，不同之处在于可以对消息 URI 的前缀进行匹配。 例如，指定地址的筛选器将 `http://www.adatum.com` 匹配发送到 `http://www.adatum.com/userA` 的消息。  
  
### <a name="xpath-message-filters"></a>XPath 消息筛选器  

 <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> 使用 XPath 表达式来确定 XML 文档是否包含特定元素、属性、文本或其他 XML 语法构造。 该筛选器经过优化，可以非常高效地筛选 XPath 的严格子集。 XML 路径语言在 [W3C Xml 路径语言1.0 规范](https://www.w3.org/TR/xpath/all/)中进行了介绍。  
  
 通常，应用程序在终结点使用 <xref:System.ServiceModel.Dispatcher.XPathMessageFilter> 来查询 SOAP 消息的内容，然后基于该查询的结果采取适当的操作。 例如，查询过程可能使用 XPath 查询来检查已知标头的优先级元素，以决定是否将消息移动到队列的靠前位置。  
  
## <a name="filter-tables"></a>筛选器表  

 筛选器表用于存储键-值对，其中筛选器为键，而某些关联数据为值。 筛选器数据可用于指示当消息与筛选器匹配时要采取的操作，筛选器数据的类型是筛选器表类的泛型参数。 筛选器数据可以包含路由规则、会话安全状态、通道上的侦听器等等。 在需要进行数据流控制的场合下，可以使用该数据。  
  
 筛选器表实现泛型接口 <xref:System.ServiceModel.Dispatcher.IMessageFilterTable%601>。  
  
 筛选器表具有多个方法，这些方法将消息与表中的所有筛选器进行匹配，并返回匹配的筛选器或数据的无序集合。 其中一些匹配方法是多匹配的，并返回所有匹配项。 其他方法是单匹配的，仅返回一个项，并在有多个筛选器匹配时引发 <xref:System.ServiceModel.Dispatcher.MultipleFilterMatchesException>。  
  
### <a name="message-filter-table"></a>消息筛选器表  

 <xref:System.ServiceModel.Dispatcher.MessageFilterTable%601> 是 <xref:System.ServiceModel.Dispatcher.IMessageFilterTable%601> 的最普通的实现。 您可以在该表中存储所有类型的筛选器。  
  
 可以将数值型优先级分配给筛选器，其中最高优先级由最大数字表示。 多个类型的筛选器可以具有相同的优先级。 某个特定类型的筛选器可以出现在多个优先级别中。  
  
 匹配是从最高优先级开始进行的，一旦找到具有给定优先级的匹配筛选器，则不再检查具有较低优先级的筛选器。 因此，如果使用的是单筛选器匹配方法，并且有多个筛选器与消息匹配，但每个匹配的筛选器都具有不同的优先级，则不会引发异常，并返回具有最高优先级的筛选器。 同样，多筛选器匹配方法仅返回那些具有最高优先级的匹配筛选器。  
  
### <a name="xpath-message-filter-table"></a>XPath 消息筛选器表  

 <xref:System.ServiceModel.Dispatcher.XPathMessageFilterTable%601> 针对声明性 XPath 筛选器进行了优化，因此表键为 <xref:System.ServiceModel.Dispatcher.XPathMessageFilter>。  
  
 <xref:System.ServiceModel.Dispatcher.XPathMessageFilterTable%601> 类针对 XPath 的子集优化了匹配，该类适用于大多数消息处理方案，同时支持完整的 XPath 1.0 语法。 该类具有适合于高效并行匹配的优化算法。  
  
 此表具有多个专用 `Match` 方法，这些方法对一个 <xref:System.Xml.XPath.XPathNavigator> 和一个 <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> 进行操作。 通过添加 <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> 属性，<xref:System.Xml.XPath.XPathNavigator> 扩展了 <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator.CurrentPosition%2A> 类。 利用此属性可以快速保存和加载 XML 文档内的位置，而无需克隆导航器 — 此克隆操作需要执行大量内存分配，并且是 <xref:System.Xml.XPath.XPathNavigator> 执行类似的保存和加载操作所需要的。 在对 XML 文档执行查询的过程中，WCF XPath 引擎必须频繁地记录游标的位置，因此为 <xref:System.ServiceModel.Dispatcher.SeekableXPathNavigator> 消息处理提供了一个重要的优化。  
  
## <a name="customer-scenarios"></a>客户方案  

 只要您希望根据消息中包含的数据将消息发送到不同处理模块，就可以使用筛选。 两种典型的方案是基于消息的操作代码对消息进行路由，以及基于消息的终结点地址对消息流进行多路分解。  
  
### <a name="routing"></a>路由  

 终结点的侦听器对 SOAP 标头中含有一个或多个操作代码的消息进行侦听。 通过将一个包含操作代码的数组传递给其构造函数，可以创建一个 <xref:System.ServiceModel.Dispatcher.ActionMessageFilter>，从而实现此功能。 它使用筛选器向 `ListenerFactory` 进行注册，因此只有当消息的操作与筛选器中的某个操作相匹配时，该消息才能到达该特定终结点。  
  
### <a name="de-multiplexing"></a>多路分解  

 当多个终结点与同一个 `ServiceListener` 连接并散布在网络中时，若要对消息进行多路分解并了解消息是否属于某个终结点地址，唯一的方式是使用 <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter>，此类筛选器通过对标头中存储的信息执行查找来选择发送到已注册终结点的消息。 在这些筛选器中，只有那些通过筛选的消息才具有所有必要的标头，这些标头同时对应于：  
  
- `EndpointAddress` 中的 URI。  
  
- `EndpointAddress` 中的其余终结点参数（如 <xref:System.ServiceModel.Dispatcher.EndpointAddressMessageFilter> 中指定）。  
  
## <a name="see-also"></a>另请参阅

- [数据传输和序列化](data-transfer-and-serialization.md)
