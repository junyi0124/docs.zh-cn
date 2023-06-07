---
title: 联合扩展性
ms.date: 03/30/2017
ms.assetid: 4d941175-74a2-4b15-81b3-086e8a95d25f
ms.openlocfilehash: 39187f228e47ed4e82c8e737563b860ce5734416
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266527"
---
# <a name="syndication-extensibility"></a>联合扩展性

联合 API 是专为提供不限制格式的编程模型而设计的，该编程模型允许将联合内容以各种格式写入到网络中。 抽象数据模型由以下类组成：  
  
- <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
- <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
- <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
- <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
- <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
 这些类严格地映射到 Atom 1.0 规范中定义的构造，但有一些名称会不相同。  
  
 联合协议的一个主要功能是扩展性。 Atom 1.0 和 RSS 2.0 均向规范中未定义的联合源中添加属性和元素。 Windows Communication Foundation (WCF) 联合编程模型提供了以下方式来使用自定义属性和扩展、松散类型访问和派生新类。  
  
## <a name="loosely-typed-access"></a>松散类型访问  

 通过派生新类添加扩展的方式需要编写附加代码。 另一种选择是以松散类型方式访问扩展。 联合抽象数据模型中定义的所有类型都包含名为 `AttributeExtensions` 和 `ElementExtensions` 的属性（但有一个例外，<xref:System.ServiceModel.Syndication.SyndicationContent> 具有 `AttributeExtensions` 属性，而不具有 `ElementExtensions` 属性）。 这些属性是未经 `TryParseAttribute` 和 `TryParseElement` 方法分别处理的扩展的集合。 通过对 <xref:System.ServiceModel.Syndication.SyndicationElementExtensionCollection.ReadElementExtensions%2A?displayProperty=nameWithType>、`ElementExtensions`、<xref:System.ServiceModel.Syndication.SyndicationFeed>、<xref:System.ServiceModel.Syndication.SyndicationItem> 和 <xref:System.ServiceModel.Syndication.SyndicationLink> 的 <xref:System.ServiceModel.Syndication.SyndicationPerson> 属性调用 <xref:System.ServiceModel.Syndication.SyndicationCategory>，可以访问这些未处理的扩展。 这组方法将使用指定的名称和命名空间查找所有扩展，将其分别反序列化到 `TExtension` 的实例中，并将其作为 `TExtension` 对象的集合返回。  
  
## <a name="deriving-a-new-class"></a>派生新类  

 可以从任何现有抽象数据模型类派生新类。 当实现一个应用程序并且您在其中使用的大多数源具有特殊扩展时，执行此操作。 在本主题中，程序使用的大多数源都包含一个 `MyExtension` 扩展。 若要提供改善的编程体验，请执行以下步骤：  
  
- 创建一个类以保留扩展数据。 在此例中，创建一个名为 MyExtension 的类。  
  
- 从 <xref:System.ServiceModel.Syndication.SyndicationItem> 派生一个名为 MyExtensionItem 的类，公开一个 MyExtension 类型的属性以便编程。  
  
- 重写 MyExtensionItem 类中的 <xref:System.ServiceModel.Syndication.SyndicationItem.TryParseElement%28System.Xml.XmlReader%2CSystem.String%29>，以便在读入 MyExtension 时实例化新的 MyExtension 实例。  
  
- 重写 MyExtensionItem 类中的 <xref:System.ServiceModel.Syndication.SyndicationItem.WriteElementExtensions%28System.Xml.XmlWriter%2CSystem.String%29>，以便将 MyExtension 属性的内容写入到 XML 编写器。  
  
- 从 <xref:System.ServiceModel.Syndication.SyndicationFeed> 派生一个名为 MyExtensionFeed 的类。  
  
- 重写 MyExtensionFeed 类中的 <xref:System.ServiceModel.Syndication.SyndicationFeed.CreateItem>，以便实例化 MyExtensionItem 而非默认的 <xref:System.ServiceModel.Syndication.SyndicationItem>。 在可以创建 <xref:System.ServiceModel.Syndication.SyndicationFeed>、<xref:System.ServiceModel.Syndication.SyndicationItem> 和 <xref:System.ServiceModel.Syndication.SyndicationLink> 对象（例如，<xref:System.ServiceModel.Syndication.SyndicationCategory>、<xref:System.ServiceModel.Syndication.SyndicationPerson> 和 <xref:System.ServiceModel.Syndication.SyndicationFeed.CreateLink>）的 <xref:System.ServiceModel.Syndication.SyndicationFeed.CreateCategory> 和 <xref:System.ServiceModel.Syndication.SyndicationFeed.CreatePerson> 中定义了一系列方法。 所有这些方法都可以重写以创建自定义派生类。  
  
## <a name="see-also"></a>另请参阅

- [WCF 联合概述](wcf-syndication-overview.md)
- [联合体系结构](architecture-of-syndication.md)
