---
title: WCF 联合对象模型如何映射到 Atom 和 RSS
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0365eb37-98cc-4b13-80fb-f1e78847a748
ms.openlocfilehash: 6fa390c8ae05035e589392c909ef340fb9c7948a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257810"
---
# <a name="how-the-wcf-syndication-object-model-maps-to-atom-and-rss"></a>WCF 联合对象模型如何映射到 Atom 和 RSS

 (WCF) 联合服务开发 Windows Communication Foundation 时，可以使用以下类创建源和项：  
  
- <xref:System.ServiceModel.Syndication.SyndicationFeed>  
  
- <xref:System.ServiceModel.Syndication.SyndicationItem>  
  
- <xref:System.ServiceModel.Syndication.SyndicationPerson>  
  
- <xref:System.ServiceModel.Syndication.SyndicationLink>  
  
- <xref:System.ServiceModel.Syndication.SyndicationCategory>  
  
- <xref:System.ServiceModel.Syndication.TextSyndicationContent>  
  
- <xref:System.ServiceModel.Syndication.UrlSyndicationContent>  
  
- <xref:System.ServiceModel.Syndication.XmlSyndicationContent>  
  
 可以按照为其定义格式化程序的任何联合格式序列化 <xref:System.ServiceModel.Syndication.SyndicationFeed>。 WCF 附带了两个格式化程序： <xref:System.ServiceModel.Syndication.Atom10FeedFormatter> 和 <xref:System.ServiceModel.Syndication.Rss20FeedFormatter> 。  
  
 与 RSS 2.0 规范相比，围绕 <xref:System.ServiceModel.Syndication.SyndicationFeed> 和 <xref:System.ServiceModel.Syndication.SyndicationItem> 的对象模型具有与 Atom 1.0 规范更密切的关系。 这是因为 Atom 1.0 是更为充分的规范，它定义了在 RSS 2.0 规范中不明确的或被忽略的元素。 因此，WCF 联合对象模型中的许多项在 RSS 2.0 规范中没有直接表示形式。 将 <xref:System.ServiceModel.Syndication.SyndicationFeed> 和对象序列化为 <xref:System.ServiceModel.Syndication.SyndicationItem> RSS 2.0 时，WCF 允许你将特定于 atom 的数据元素序列化为符合 Atom 规范的命名空间限定扩展元素。 可以通过传递到 <xref:System.ServiceModel.Syndication.Rss20FeedFormatter> 构造函数的参数对此进行控制。  
  
 本主题中的代码示例使用此处定义的两种方法之一进行实际的序列化。  
  
 `SerializeFeed` 序列化联合源。  
  
 [!code-csharp[SyndicationMapping#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#10)]
 [!code-vb[SyndicationMapping#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#10)]  
  
 `SerializeItem` 序列化联合项。  
  
 [!code-csharp[SyndicationMapping#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#11)]
 [!code-vb[SyndicationMapping#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#11)]  
  
## <a name="syndicationfeed"></a>SyndicationFeed  

 下面的代码示例演示如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationFeed> 类。  
  
 [!code-csharp[SyndicationMapping#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#0)]
 [!code-vb[SyndicationMapping#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#0)]  
  
 下面的 XML 演示如何将 <xref:System.ServiceModel.Syndication.SyndicationFeed> 序列化为 Atom 1.0。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<feed xml:lang="EN-US" xmlns="http://www.w3.org/2005/Atom">  
  <title type="text">My Feed Title</title>  
  <subtitle type="text">My Feed Description</subtitle>  
  <id>FeedID</id>  
  <rights type="text">Copyright 2007</rights>  
  <updated>2007-08-29T13:57:17-07:00</updated>  
  <category term="categoryName" label="categoryLabel" scheme="categoryScheme" />  
  <logo>http://server/image.jpg</logo>  
  <generator>Sample Code</generator>  
  <link rel="alternate" href="http://myfeeduri/" />  
  <entry>  
    <id>ItemID</id>  
    <title type="text">Item Title</title>  
    <summary type="text">Item Summary</summary>  
    <published>2007-08-29T00:00:00-07:00</published>  
    <updated>2007-08-29T13:57:17-07:00</updated>  
    <author>  
      <name>Jesper Aaberg</name>  
      <uri>http://Jesper/Aaberg</uri>  
      <email>Jesper@Aaberg.com</email>  
    </author>  
    <contributor>  
      <name>Lene Aaling</name>  
      <uri>http://Lene/Aaling</uri>  
      <email>Lene@Aaling.com</email>  
    </contributor>  
    <link rel="alternate" href="http://myitemuri/" />  
    <category term="categoryName" label="categoryLabel" scheme="categoryScheme" />  
    <content type="text">Item Content</content>  
    <rights type="text">Copyright 2007</rights>  
    <source>  
      <title type="text">My Feed Title</title>  
      <subtitle type="text">My Feed Description</subtitle>  
      <id>FeedID</id>  
      <rights type="text">Copyright 2007</rights>  
      <updated>2007-08-29T13:57:17-07:00</updated>  
      <category term="categoryName" label="categoryLabel" scheme="categoryScheme" />  
      <logo>http://server/image.jpg</logo>  
      <generator>Sample Code</generator>  
      <link rel="alternate" href="http://myfeeduri/" />  
    </source>  
  </entry>  
</feed>  
```  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationFeed>。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<rss xmlns:a10="http://www.w3.org/2005/Atom" version="2.0">  
  <channel>  
    <title>My Feed Title</title>  
    <link>http://myfeeduri/</link>  
    <description>My Feed Description</description>  
    <language>EN-US</language>  
    <copyright>Copyright 2007</copyright>  
    <lastBuildDate>Wed, 29 Aug 2007 13:57:17 -0700</lastBuildDate>  
    <category domain="categoryScheme">categoryName</category>  
    <generator>Sample Code</generator>  
    <image>  
      <url>http://server/image.jpg</url>  
      <title>My Feed Title</title>  
      <link>http://myfeeduri/</link>  
    </image>  
    <a10:id>FeedID</a10:id>  
    <item>  
      <guid isPermaLink="false">ItemID</guid>  
      <link>http://myitemuri/</link>  
      <author>Jesper@Aaberg.com</author>  
      <category domain="categoryScheme">categoryName</category>  
      <title>Item Title</title>  
      <description>Item Summary</description>  
      <source>My Feed Title</source>  
      <pubDate>Wed, 29 Aug 2007 00:00:00 -0700</pubDate>  
      <a10:updated>2007-08-29T13:57:17-07:00</a10:updated>  
      <a10:rights type="text">Copyright 2007</a10:rights>  
      <a10:content type="text">Item Content</a10:content>  
      <a10:contributor>  
        <a10:name>Lene Aaling</a10:name>  
        <a10:uri>http://Lene/Aaling</a10:uri>  
        <a10:email>Lene@Aaling.com</a10:email>  
      </a10:contributor>  
    </item>  
  </channel>  
</rss>  
```  
  
## <a name="syndicationitem"></a>SyndicationItem  

 下面的代码示例演示如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationItem> 类。  
  
 [!code-csharp[SyndicationMapping#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#1)]
 [!code-vb[SyndicationMapping#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#1)]  
  
 下面的 XML 演示如何将 <xref:System.ServiceModel.Syndication.SyndicationItem> 序列化为 Atom 1.0。  
  
```xml  
<entry xmlns="http://www.w3.org/2005/Atom">  
  <id>ItemID</id>  
  <title type="text">Item Title</title>  
  <summary type="text">Item Summary</summary>  
  <published>2007-08-29T00:00:00-07:00</published>  
  <updated>2007-08-29T14:07:09-07:00</updated>  
  <author>  
    <name>Jesper Aaberg</name>  
    <uri>http://Contoso/Aaberg</uri>  
    <email>Jesper.Aaberg@contoso.com</email>  
  </author>  
  <author>  
    <name>Syed Abbas</name>  
    <uri>http://Contoso/Abbas</uri>  
    <email>Syed.Abbas@contoso.com</email>  
  </author>  
  <contributor>  
    <name>Lene Aaling</name>  
    <uri>http://Contoso/Aaling</uri>  
    <email>Lene.Aaling@contoso.com</email>  
  </contributor>  
  <contributor>  
    <name>Kim Abercrombie</name>  
    <uri>http://Contoso/Abercrombie</uri>  
    <email>Kim.Abercrombie@contoso.com</email>  
  </contributor>  
  <link rel="alternate" href="http://myitemuri/" />  
  <category term="categoryName" label="categoryLabel" scheme="categoryScheme" />  
  <category term="categoryName" label="categoryLabel" scheme="categoryScheme" />  
  <content type="text">Item Content</content>  
  <rights type="text">Copyright 2007</rights>  
  <source>  
    <title type="text">My Feed Title</title>  
    <subtitle type="text">My Feed Description</subtitle>  
    <link rel="alternate" href="http://myfeeduri/" />  
  </source>  
</entry>  
```  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationItem>。  
  
```xml  
<item>  
  <guid isPermaLink="false">ItemID</guid>  
  <link>http://myitemuri/</link>  
  <author xmlns="http://www.w3.org/2005/Atom">  
    <name>Jesper Aaberg</name>  
    <uri>http://Jesper/Aaberg</uri>  
    <email>Jesper@Aaberg.com</email>  
  </author>  
  <author xmlns="http://www.w3.org/2005/Atom">  
    <name>Syed Abbas</name>  
    <uri>http://Contoso/Abbas</uri>  
    <email>Syed.Abbas@contoso.com</email>  
  </author>  
  <category domain="categoryScheme">categoryName</category>  
  <category domain="categoryScheme">categoryName</category>  
  <title>Item Title</title>  
  <description>Item Summary</description>  
  <source>My Feed Title</source>  
  <pubDate>Wed, 29 Aug 2007 00:00:00 -0700</pubDate>  
  <updated xmlns="http://www.w3.org/2005/Atom">2007-08-29T14:07:09-07:00</updated>  
  <rights type="text" xmlns="http://www.w3.org/2005/Atom">Copyright 2007</rights>  
  <content type="text" xmlns="http://www.w3.org/2005/Atom">Item Content</content>  
  <contributor xmlns="http://www.w3.org/2005/Atom">  
    <name>Lene Aaling</name>  
    <uri>http://Contoso/Aaling</uri>  
    <email>Lene.Aaling@contoso.com</email>  
  </contributor>  
  <contributor xmlns="http://www.w3.org/2005/Atom">  
    <name>Kim Abercrombie</name>  
    <uri>http://Contoso/Abercrombie</uri>  
    <email>Kim.Abercrombie@contoso.com</email>  
  </contributor>  
</item>  
```  
  
## <a name="syndicationperson"></a>SyndicationPerson  

 下面的代码示例演示如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationPerson> 类。  
  
 [!code-csharp[SyndicationMapping#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#2)]
 [!code-vb[SyndicationMapping#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#2)]  
  
 下面的 XML 演示如何将 <xref:System.ServiceModel.Syndication.SyndicationPerson> 序列化为 Atom 1.0。  
  
```xml  
  <author>  
    <name>Jesper Aaberg</name>  
    <uri>http://Contoso/Aaberg</uri>  
    <email>Jesper.Aaberg@contoso.com</email>  
  </author>  
<contributor>  
    <name>Lene Aaling</name>  
    <uri>http://Contoso/Aaling</uri>  
    <email>Lene.Aaling@contoso.com</email>  
  </contributor>  
```  
  
 下面的 XML 演示在 <xref:System.ServiceModel.Syndication.SyndicationPerson> 或 <xref:System.ServiceModel.Syndication.SyndicationPerson> 集合中分别仅存在一个 `Authors` 时，如何按照 RSS 2.0 序列化 `Contributors` 类。  
  
```xml  
<author>Jesper.Aaberg@contoso.com</author>  
<a10:contributor>  
    <a10:name>Lene Aaling</a10:name>  
    <a10:uri>http://Contoso/Aaling</a10:uri>  
    <a10:email>Lene.Aaling@contoso.com</a10:email>  
</a10:contributor>  
```  
  
 下面的 XML 演示在 <xref:System.ServiceModel.Syndication.SyndicationPerson> 或 <xref:System.ServiceModel.Syndication.SyndicationPerson> 集合中分别存在多个 `Authors` 时，如何按照 RSS 2.0 序列化 `Contributors` 类。  
  
```xml  
<a10:author>  
    <a10:name>Jesper Aaberg</a10:name>  
    <a10:uri>http://Contoso/Aaberg</a10:uri>  
    <a10:email>Jesper.Aaberg@contoso.com</a10:email>  
</a10:author>  
<a10:author>  
    <a10:name>Syed Abbas</a10:name>  
    <a10:uri>http://Contoso/Abbas</a10:uri>  
    <a10:email>Syed.Abbas@contoso.com</a10:email>  
</a10:author>  
<a10:contributor>  
    <a10:name>Lene Aaling</a10:name>  
    <a10:uri>http://Contoso/Aaling</a10:uri>  
    <a10:email>Lene.Aaling@contoso.com</a10:email>  
</a10:contributor>  
<a10:contributor>  
    <a10:name>Kim Abercrombie</a10:name>  
    <a10:uri>http://Contoso/Abercrombie</a10:uri>  
    <a10:email>Kim.Abercrombie@contoso.com</a10:email>  
</a10:contributor>  
```  
  
## <a name="syndicationlink"></a>SyndicationLink  

 下面的代码示例演示如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationLink> 类。  
  
 [!code-csharp[SyndicationMapping#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#3)]
 [!code-vb[SyndicationMapping#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#3)]  
  
 下面的 XML 演示如何将 <xref:System.ServiceModel.Syndication.SyndicationLink> 序列化为 Atom 1.0。  
  
 `<link rel="alternate" type="text/html" title="My Link Title" length="2048" href="http://contoso/MyLink" />`  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationLink>。  
  
 `<a10:link rel="alternate" type="text/html" title="My Link Title" length="2048" href="http://contoso/MyLink" />`  
  
## <a name="syndicationcategory"></a>SyndicationCategory  

 下面的代码示例演示如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationCategory> 类。  
  
 [!code-csharp[SyndicationMapping#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#4)]
 [!code-vb[SyndicationMapping#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#4)]  
  
 下面的 XML 演示如何将 <xref:System.ServiceModel.Syndication.SyndicationCategory> 序列化为 Atom 1.0。  
  
 `<category term="categoryName" label="categoryLabel" scheme="categoryScheme" />`  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.SyndicationCategory>。  
  
 `<category domain="categoryScheme">categoryName</category>`  
  
## <a name="textsyndicationcontent"></a>TextSyndicationContent  

 下面的代码示例演示使用 HTML 内容创建 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 时，如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 [!code-csharp[SyndicationMapping#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#5)]
 [!code-vb[SyndicationMapping#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#5)]  
  
 下面的 XML 演示如何按照 Atom 1.0 序列化具有 HTML 内容的 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 `<content type="html"><html> some html </html></content>`  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化具有 HTML 内容的 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 `<description><html> some html </html></description>`  
  
 下面的代码示例演示使用纯文本内容创建 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 时，如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 [!code-csharp[SyndicationMapping#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#6)]
 [!code-vb[SyndicationMapping#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#6)]  
  
 下面的 XML 演示如何按照 Atom 1.0 序列化具有纯文本内容的 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 `<content type="text">Some Plain Text</content>`  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化具有纯文本内容的 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 `<description>Some Plain Text</description>`  
  
 下面的代码示例演示使用 XHTML 内容创建 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 时，如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 [!code-csharp[SyndicationMapping#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#7)]
 [!code-vb[SyndicationMapping#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#7)]  
  
 下面的 XML 演示如何按照 Atom 1.0 序列化具有 XHTML 内容的 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 `<content type="xhtml">`  
  
 `<html> some xhtml </html>`  
  
 `</content>`  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化具有 XHTML 内容的 <xref:System.ServiceModel.Syndication.TextSyndicationContent> 类。  
  
 `<description><html> some xhtml </html></description>`  
  
## <a name="urlsyndicationcontent"></a>UrlSyndicationContent  

 下面的代码示例演示如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.UrlSyndicationContent> 类。  
  
 [!code-csharp[SyndicationMapping#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#8)]
 [!code-vb[SyndicationMapping#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#8)]  
  
 下面的 XML 演示如何按照 Atom 1.0 序列化 <xref:System.ServiceModel.Syndication.UrlSyndicationContent> 类。  
  
 `<content type="audio" src="http://someurl/" />`  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化具有 XHTML 内容的 <xref:System.ServiceModel.Syndication.UrlSyndicationContent> 类。  
  
 `<description />`  
  
 `<content type="audio" src="http://Contoso/someurl/" xmlns="http://www.w3.org/2005/Atom" />`  
  
## <a name="xmlsyndicationcontent"></a>XmlSyndicationContent  

 下面的代码示例演示如何按照 Atom 1.0 和 RSS 2.0 序列化 <xref:System.ServiceModel.Syndication.XmlSyndicationContent> 类。  
  
 [!code-csharp[SyndicationMapping#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/syndicationmapping/cs/snippets.cs#9)]
 [!code-vb[SyndicationMapping#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/syndicationmapping/vb/snippets.vb#9)]  
  
 下面的 XML 演示如何按照 Atom 1.0 序列化 <xref:System.ServiceModel.Syndication.XmlSyndicationContent> 类。  
  
 `<content type="mytype">`  
  
 `<SomeData xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.datacontract.org/2004/07/FeedMapping" />`  
  
 `</content>`  
  
 下面的 XML 演示如何按照 RSS 2.0 序列化具有 XHTML 内容的 <xref:System.ServiceModel.Syndication.XmlSyndicationContent> 类。  
  
 `<content type="mytype" xmlns="http://www.w3.org/2005/Atom">`  
  
 `<SomeData xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.datacontract.org/2004/07/FeedMapping" />`  
  
 `</content>`  
  
## <a name="see-also"></a>另请参阅

- [WCF 联合概述](wcf-syndication-overview.md)
- [联合体系结构](architecture-of-syndication.md)
- [如何：创建基本 RSS 源](how-to-create-a-basic-rss-feed.md)
- [如何：创建基本 Atom 源](how-to-create-a-basic-atom-feed.md)
- [如何：作为 Atom 和 RSS 公开源](how-to-expose-a-feed-as-both-atom-and-rss.md)
