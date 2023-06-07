---
title: 如何：为数据服务请求（WCF 数据服务）指定客户端凭据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, customizing requests
ms.assetid: 1632f9af-e45f-4363-9222-03823daa8e28
ms.openlocfilehash: fd69d5f7eddf713612000b0ad677e7ada378553e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180655"
---
# <a name="how-to-specify-client-credentials-for-a-data-service-request-wcf-data-services"></a>如何：为数据服务请求（WCF 数据服务）指定客户端凭据

默认情况下，客户端库在向 OData 服务发送请求时不提供凭据。 但是，可以通过为 <xref:System.Net.NetworkCredential> 的 <xref:System.Data.Services.Client.DataServiceContext.Credentials%2A> 属性提供 <xref:System.Data.Services.Client.DataServiceContext> 以指定发送凭据，对发送到数据服务的请求进行身份验证。 有关更多信息，请参见 [Securing WCF Data Services](securing-wcf-data-services.md)。 本主题中的示例演示如何显式提供在从数据服务请求数据时 WCF 数据服务客户端使用的凭据。  
  
 本主题中的示例使用罗斯文示例数据服务和自动生成的客户端数据服务类。 此服务和客户端数据类是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。 你还可以使用在 OData 网站上发布的 [Northwind 示例数据服务](https://services.odata.org/Northwind/Northwind.svc/) ;此示例数据服务是只读的，尝试保存更改将返回错误。 OData 网站上的示例数据服务允许匿名身份验证。  
  
## <a name="example"></a>示例  

 以下示例来自 Windows Presentation Framework 应用程序的可扩展应用程序标记语言 (XAML) 主页文件的代码隐藏页。 该示例显示了一个 `LoginWindow` 实例，用于收集用户的身份验证凭据，然后在向数据服务发出请求时使用了这些凭据。  
  
 [!code-csharp[Astoria Northwind Client#ClientCredentials](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentials.xaml.cs#clientcredentials)]  
 [!code-vb[Astoria Northwind Client#ClientCredentials](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/clientcredentials.xaml.vb#clientcredentials)]
  
## <a name="example"></a>示例  

 以下 XAML 定义了 WPF 应用程序的主页。  
  
 [!code-xaml[Astoria Northwind Client#ClientCredentialsXaml](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentials.xaml#clientcredentialsxaml)]  
  
## <a name="example"></a>示例  

 以下示例来自窗口的代码隐藏页，该窗口用于在向数据服务发出请求时收集用户的身份验证凭据。  
  
 [!code-csharp[Astoria Northwind Client#ClientCredentialsLogin](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentialslogin.xaml.cs#clientcredentialslogin)]  
 [!code-vb[Astoria Northwind Client#ClientCredentialsLogin](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/clientcredentialslogin.xaml.vb#clientcredentialslogin)]
  
## <a name="example"></a>示例  

 以下 XAML 定义了 WPF 应用程序的登录。  
  
 [!code-xaml[Astoria Northwind Client#ClientCredentialsLoginXaml](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/clientcredentialslogin.xaml#clientcredentialsloginxaml)]  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 本主题中的示例适用以下安全注意事项：  
  
- 为验证此示例中提供的凭据是否能够正常工作，罗斯文数据服务必须使用一种非匿名访问的身份验证方案。 否则，承载数据服务的网站将不会请求凭据。  
  
- 用户凭据应仅在执行过程中请求并且不应缓存。 必须始终安全地存储凭据。  
  
- 使用基本和摘要式身份验证发送的数据不会加密，因此攻击者会看到数据。 此外，基本身份验证凭据（用户名和密码）是以明文形式发送的，会被截取。  
  
 有关更多信息，请参见 [Securing WCF Data Services](securing-wcf-data-services.md)。  
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务的安全](securing-wcf-data-services.md)
- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
