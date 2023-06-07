---
title: 如何：创建基本 Atom 源
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6e0cacc1-9b11-4665-adb7-577a62626fd6
ms.openlocfilehash: 76362684d289ca6aaac852158a05087a0a28f837
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254046"
---
# <a name="how-to-create-a-basic-atom-feed"></a>如何：创建基本 Atom 源

Windows Communication Foundation (WCF) 允许您创建公开联合源的服务。 本主题讨论如何创建公开 Atom 联合源的联合服务。  
  
### <a name="to-create-a-basic-syndication-service"></a>创建基本联合服务  
  
1. 使用通过 <xref:System.ServiceModel.Web.WebGetAttribute> 属性标记的接口定义服务协定。 作为联合源公开的每个操作应返回 <xref:System.ServiceModel.Syndication.Atom10FeedFormatter> 对象。  
  
     [!code-csharp[htAtomBasic#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#0)]
     [!code-vb[htAtomBasic#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#0)]  
  
    > [!NOTE]
    > 应用 <xref:System.ServiceModel.Web.WebGetAttribute> 的所有服务操作将映射到 HTTP GET 请求。 若要将操作映射到不同的 HTTP 方法，请改用 <xref:System.ServiceModel.Web.WebInvokeAttribute>。 有关详细信息，请参阅 [如何：创建基本 WCF WEB HTTP 服务](how-to-create-a-basic-wcf-web-http-service.md)。  
  
2. 实现服务协定。  
  
     [!code-csharp[htAtomBasic#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#1)]
     [!code-vb[htAtomBasic#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#1)]  
  
3. 创建 <xref:System.ServiceModel.Syndication.SyndicationFeed> 对象，并添加作者、类别和说明。  
  
     [!code-csharp[htAtomBasic#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#2)]
     [!code-vb[htAtomBasic#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#2)]  
  
4. 创建若干 <xref:System.ServiceModel.Syndication.SyndicationItem> 对象。  
  
     [!code-csharp[htAtomBasic#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#3)]
     [!code-vb[htAtomBasic#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#3)]  
  
5. 向源中添加 <xref:System.ServiceModel.Syndication.SyndicationItem> 对象。  
  
     [!code-csharp[htAtomBasic#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#4)]
     [!code-vb[htAtomBasic#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#4)]  
  
6. 返回源。  
  
     [!code-csharp[htAtomBasic#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#5)]
     [!code-vb[htAtomBasic#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#5)]  
  
### <a name="to-host-the-service"></a>承载服务  
  
1. 创建 <xref:System.ServiceModel.Web.WebServiceHost> 对象。  
  
     [!code-csharp[htAtomBasic#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#6)]
     [!code-vb[htAtomBasic#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#6)]  
  
2. 打开服务主机，从服务加载源，显示源，然后等待用户按 Enter。  
  
     [!code-csharp[htAtomBasic#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#8)]
     [!code-vb[htAtomBasic#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#8)]  
  
### <a name="to-call-getblog-with-an-http-get"></a>使用 HTTP GET 调用 GetBlog()  
  
1. 打开 Internet Explorer，键入以下 URL，然后按 ENTER： `http://localhost:8000/BlogService/GetBlog`  
  
     URL 包含服务的基址 (`http://localhost:8000/BlogService`) 、终结点的相对地址以及要调用的服务操作。  
  
### <a name="to-call-getblog-from-code"></a>从代码中调用 GetBlog()  
  
1. 使用基址和调用的方法创建 <xref:System.Xml.XmlReader>。  
  
     [!code-csharp[htAtomBasic#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/snippets.cs#9)]
     [!code-vb[htAtomBasic#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/snippets.vb#9)]  
  
2. 调用静态 <xref:System.ServiceModel.Syndication.SyndicationFeed.Load%28System.Xml.XmlReader%29> 方法，同时传入刚刚创建的 <xref:System.Xml.XmlReader>。  
  
     [!code-csharp[htAtomBasic#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/snippets.cs#10)]
     [!code-vb[htAtomBasic#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/snippets.vb#10)]  
  
     这将调用服务操作，并用从服务操作返回的格式化程序填充新的 <xref:System.ServiceModel.Syndication.SyndicationFeed>。  
  
3. 访问源对象。  
  
     [!code-csharp[htAtomBasic#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/snippets.cs#11)]
     [!code-vb[htAtomBasic#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/snippets.vb#11)]  
  
## <a name="example"></a>示例  

 下面列出了此示例的完整代码。  
  
 [!code-csharp[htAtomBasic#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/htatombasic/cs/program.cs#12)]
 [!code-vb[htAtomBasic#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htatombasic/vb/program.vb#12)]  
  
## <a name="compiling-the-code"></a>编译代码  

 编译前面的代码时，请引用 System.ServiceModel.dll 和 System.ServiceModel.Web.dll。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
