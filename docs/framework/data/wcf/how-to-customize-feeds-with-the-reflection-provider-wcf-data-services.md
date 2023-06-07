---
title: 如何：使用反射提供程序自定义源（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, customizing
- WCF Data Services, customizing feeds
ms.assetid: 00c23dcf-9bb8-459a-a012-6c4d9bcad7e9
ms.openlocfilehash: fb22e87569a8e243813b3186232b6989abeb2a5e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161303"
---
# <a name="how-to-customize-feeds-with-the-reflection-provider-wcf-data-services"></a>如何：使用反射提供程序自定义源（WCF 数据服务）

WCF 数据服务使你能够在数据服务响应中自定义 Atom 序列化，以便可以将实体的属性映射到在 AtomPub 协议中定义的未使用的元素。 本主题演示如何为使用反射提供程序定义的数据模型中的实体类型定义映射特性。 有关详细信息，请参阅 [源自定义](feed-customization-wcf-data-services.md)。  
  
 主题[如何：使用反射提供程序创建数据服务](create-a-data-service-using-rp-wcf-data-services.md)中定义了此示例的数据模型  
  
## <a name="example"></a>示例  

 在以下示例中，`Order` 类型的两个属性均映射到现有的 Atom 元素。 `Product` 类型的 `Item` 属性映射到单独的命名空间中的自定义源特性。  
  
 [!code-csharp[Astoria Custom Feeds#CustomIQueryableFeeds](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_custom_feeds/cs/orderitems.svc.cs#customiqueryablefeeds)]
 [!code-vb[Astoria Custom Feeds#CustomIQueryableFeeds](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_custom_feeds/vb/orderitems.svc.vb#customiqueryablefeeds)]  
  
## <a name="example"></a>示例  

 上面的示例为 URI `http://myservice/OrderItems.svc/Orders(0)?$expand=Items` 返回以下结果。  
  
 [!code-xml[Astoria Custom Feeds#IQueryableFeedResultInline](../../../../samples/snippets/xml/VS_Snippets_Misc/astoria_custom_feeds/xml/iqueryablefeedresultinline.xml#iqueryablefeedresultinline)]  
  
## <a name="see-also"></a>请参阅

- [反射提供程序](reflection-provider-wcf-data-services.md)
