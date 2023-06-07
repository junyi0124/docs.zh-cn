---
title: 联合体系结构
ms.date: 03/30/2017
ms.assetid: ed4ca86e-e3d8-4acb-87aa-1921fbc353be
ms.openlocfilehash: bc8c9cfbdc4f5d8ee01dfcf5098cfbc74afc2467
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96234799"
---
# <a name="architecture-of-syndication"></a>联合体系结构

联合 API 是专为提供不限制格式编程模型而设计的，该编程模型允许将各种格式的联合内容写入网络中。 抽象数据模型由以下类组成：  
  
- <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
- <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
- <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
- <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
- <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
 这些类严格地映射到 Atom 1.0 规范中定义的构造，但有一些名称会不相同。  
  
 在 Windows Communication Foundation (WCF) ，联合源将建模为另一种类型的服务操作，其中，返回类型是的派生类之一 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> 。 源的检索建模为请求-响应消息交换。 客户端向服务发送请求，而服务进行响应。 请求消息是通过基础结构协议（例如，原始 HTTP）设置的，而响应消息包含由通常可以理解的联合格式（RSS 2.0 或 Atom 1.0）组成的负载。 实现这些消息交换的服务称为联合服务。  
  
 联合服务的协定包含一组操作，这些操作返回 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> 类的实例。 下面的示例演示联合服务的接口声明。  
  
 [!code-csharp[S_UE_SyndicationBoth#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_syndicationboth/cs/service.cs#0)]  
  
 联合支持是基于 WCF REST 编程模型生成的，该模型定义了 <xref:System.ServiceModel.WebHttpBinding> 绑定，该绑定与一起用于 <xref:System.ServiceModel.Description.WebHttpBehavior> 使源可用作服务。 有关 WCF REST 编程模型的详细信息，请参阅 [Wcf WEB HTTP 编程模型概述](wcf-web-http-programming-model-overview.md)。  
  
> [!NOTE]
> Atom 1.0 规范允许在其任何日期构造中指定秒的小数部分。 序列化和反序列化 WCF 实现会忽略秒的小数部分。  
  
## <a name="object-model"></a>对象模型  

 联合的对象模型由下表中多个组中的类组成。  
  
 格式设置类：  
  
|实例|描述|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Syndication.Atom10FeedFormatter>|用于将 <xref:System.ServiceModel.Syndication.SyndicationFeed> 实例序列化为 Atom 1.0 格式的类。|  
|<xref:System.ServiceModel.Syndication.Atom10FeedFormatter%601>|用于将 <xref:System.ServiceModel.Syndication.SyndicationFeed> 派生类序列化为 Atom 1.0 格式的类。|  
|<xref:System.ServiceModel.Syndication.Atom10ItemFormatter>|用于将 <xref:System.ServiceModel.Syndication.SyndicationItem> 实例序列化为 Atom 1.0 格式的类。|  
|<xref:System.ServiceModel.Syndication.Atom10ItemFormatter%601>|用于将 <xref:System.ServiceModel.Syndication.SyndicationItem> 派生类序列化为 Atom 1.0 格式的类。|  
|<xref:System.ServiceModel.Syndication.Rss20FeedFormatter>|用于将 <xref:System.ServiceModel.Syndication.SyndicationFeed> 实例序列化为 RSS 2.0 格式的类。|  
|<xref:System.ServiceModel.Syndication.Rss20FeedFormatter%601>|用于将 <xref:System.ServiceModel.Syndication.SyndicationFeed> 派生类序列化为 RSS 2.0 格式的类。|  
|<xref:System.ServiceModel.Syndication.Rss20ItemFormatter>|用于将 <xref:System.ServiceModel.Syndication.SyndicationItem> 实例序列化为 RSS 2.0 格式的类。|  
|<xref:System.ServiceModel.Syndication.Rss20ItemFormatter%601>|用于将 <xref:System.ServiceModel.Syndication.SyndicationItem> 派生类序列化为 RSS 2.0 格式的类。|  
  
 对象模型类：  
  
|实例|描述|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Syndication.SyndicationCategory>|一个表示联合源类别的类。|  
|<xref:System.ServiceModel.Syndication.SyndicationContent>|一个表示联合内容的基类。|  
|<xref:System.ServiceModel.Syndication.SyndicationElementExtension>|一个表示联合元素扩展的类。|  
|<xref:System.ServiceModel.Syndication.SyndicationElementExtensionCollection>|<xref:System.ServiceModel.Syndication.SyndicationElementExtension> 对象的集合。|  
|<xref:System.ServiceModel.Syndication.SyndicationFeed>|一个表示顶级源对象的类。|  
|<xref:System.ServiceModel.Syndication.SyndicationItem>|一个表示源项的类。|  
|<xref:System.ServiceModel.Syndication.SyndicationLink>|一个表示联合源或联合项中的链接的类。|  
|<xref:System.ServiceModel.Syndication.SyndicationPerson>|一个表示 Atom Person 构造的类。|  
|<xref:System.ServiceModel.Syndication.SyndicationVersions>|一个表示所支持的联合协议版本的类。|  
|<xref:System.ServiceModel.Syndication.TextSyndicationContent>|一个表示要显示给最终用户的任何 <xref:System.ServiceModel.Syndication.SyndicationItem> 内容的类。|  
|<xref:System.ServiceModel.Syndication.TextSyndicationContentKind>|一个表示所支持的不同文本联合内容类型的枚举。|  
|<xref:System.ServiceModel.Syndication.UrlSyndicationContent>|一个表示包含指向另一资源的 URL 的联合内容的类。|  
|<xref:System.ServiceModel.Syndication.XmlSyndicationContent>|一个表示不显示在浏览器中的联合内容的类。|  
  
 对象模型中的核心数据抽象是源和项，它们分别对应于 <xref:System.ServiceModel.Syndication.SyndicationFeed> 和 <xref:System.ServiceModel.Syndication.SyndicationItem> 类。 源会公开一些源级别的元数据（例如，标题、说明和作者）、一个存储未知扩展的位置以及组成源的其余信息内容的一组项。 项可以提供一些项级别的元数据（例如，标题、摘要和发布日期）、一个存储未知扩展的位置以及一个包含项的其余信息内容的内容元素。 源和项这两个核心抽象由表示 Atom 1.0 和 RSS 规范中引用的常见数据构造的其他类提供支持。  
  
 源实例中携带的信息可以转换成各种 XML 格式。 与 XML 的来回转换过程由 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> 类管理。 此类是一个抽象类；为 Atom 1.0 和 RSS 2.0 提供的具体实现分别为 <xref:System.ServiceModel.Syndication.Atom10FeedFormatter> 和 <xref:System.ServiceModel.Syndication.Rss20FeedFormatter>。 若要使用派生源类，应使用 <xref:System.ServiceModel.Syndication.Atom10FeedFormatter%601> 或 <xref:System.ServiceModel.Syndication.Rss20FeedFormatter%601>，您可以通过它们指定派生源类。 若要使用派生项类，应使用 <xref:System.ServiceModel.Syndication.Atom10ItemFormatter%601> 或 <xref:System.ServiceModel.Syndication.Rss20ItemFormatter%601>，您可以通过它们指定派生项类。第三方可以派生各自的 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> 实现以支持不同的联合格式。  
  
## <a name="extensibility"></a>扩展性  
  
- 联合协议的一个主要功能是扩展性。 Atom 1.0 和 RSS 2.0 都允许您向联合源中添加规范中没有定义的属性和元素。 WCF 联合编程模型提供了两种使用自定义属性和扩展的方法：派生新类和松散类型访问。 有关详细信息，请参阅 [联合扩展性](syndication-extensibility.md)。  
  
## <a name="see-also"></a>另请参阅

- [WCF 联合概述](wcf-syndication-overview.md)
- [WCF 联合对象模型如何映射到 Atom 和 RSS](how-the-wcf-syndication-object-model-maps-to-atom-and-rss.md)
- [WCF Web HTTP 编程模型](wcf-web-http-programming-model.md)
