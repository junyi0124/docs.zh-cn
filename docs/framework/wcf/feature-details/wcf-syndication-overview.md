---
title: WCF 联合概述
ms.date: 03/30/2017
ms.assetid: af6d4c39-e5e8-4099-aee6-5261feff9107
ms.openlocfilehash: 8ffc42987ff03b074e35f594491a0ca5a3adbe46
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239050"
---
# <a name="wcf-syndication-overview"></a>WCF 联合概述

Windows Communication Foundation (WCF) 提供对从 WCF 服务公开联合源的支持。 联合是一种应用程序集成机制，在这种机制中，服务器以一种可互操作的格式（称为源）公开某些应用程序数据。 源是应用程序数据的集合，其中包括一些源级别的元数据（标题、作者、URL 和其他元数据）以及一系列源项。 在源内，源项通常是按时间的倒序顺序排序的。 源项由一组标准的项级别元数据（标题、URL、创建日期、类别和其他项级别的元数据）以及任意数量的应用程序特定数据组成。 最常见的两种类型的联合源是 (RSS) 2.0 和 Atom 1.0 的简单联合，这两者都受 WCF 支持。  
  
## <a name="object-model"></a>对象模型  

 WCF 定义一组特定于联合的类，使你能够以与格式无关的方式使用源、源项和相关元数据： <xref:System.ServiceModel.Syndication.SyndicationFeed> 、、、 <xref:System.ServiceModel.Syndication.SyndicationItem> 以及 <xref:System.ServiceModel.Syndication.SyndicationPerson> <xref:System.ServiceModel.Syndication.SyndicationLink> 其他特定于联合的类。 WCF 还定义基础结构类，这些类在 WCF REST 编程模型上构建，以提供联合支持，包括： <xref:System.ServiceModel.Syndication.Atom10FeedFormatter> 和  <xref:System.ServiceModel.Syndication.Rss20FeedFormatter> 。 源格式化程序类支持在 RSS 2.0 和 Atom 1.0 之间序列化对象模型。  
  
## <a name="scenarios"></a>方案  

 如今，联合通常用于博客，其中博客作者会定期发布某类信息。 可以是文本、图像、音频或其他类型的信息。 许多报纸和杂志也使用联合来发布新闻故事或文章。 通过订阅这样的源，用户可以获得所有来自此类网站的最新信息。 虽然联合通常与博客和发行者关联，但联合也可以与任何公开信息集合的应用程序一起使用，例如，想要使用联合源公开的 bug 数据库。 你可以创建公开操作的 WCF 服务 `CodeDefects` 。 此操作可能会需要一个参数，该参数指定要检索其 bug 的人员的电子邮件地址。 客户端可以使用以下 URL 调用操作： `http://someserver/bugDatabase/CodeDefects?user=johndoe` 。  
  
## <a name="syndication-formats"></a>联合格式  

 WCF 联合平台支持 RSS 2.0 和 Atom 1.0。  
  
## <a name="see-also"></a>另请参阅

- [WCF Web HTTP 编程模型](wcf-web-http-programming-model.md)
