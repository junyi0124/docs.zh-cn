---
title: 从应用程序域中检索安装信息
description: 使用 System.AppDomain 类或 AppDomainSetup 对象从 .NET 中的应用程序域检索安装信息。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- AppDomainSetup object
- retrieving setup information
- application domains, retrieving setup information
ms.assetid: 5cdb12ae-1e37-4a62-8ec7-93d6dcc6e8d9
ms.openlocfilehash: 3b7fdd302ac11caa423815483a4add38264f0910
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325660"
---
# <a name="retrieve-setup-information-from-an-application-domain"></a>从应用程序域中检索安装信息

应用程序域的每个实例由属性和 <xref:System.AppDomainSetup> 信息组成。 可使用 <xref:System.AppDomain?displayProperty=nameWithType> 类从应用程序域中检索安装信息。 此类提供多个成员，可用于检索应用程序域的相关配置信息。  
  
 还可以查询应用程序域的 AppDomainSetup 对象，获取创建域时传递到该域的安装信息。  
  
 下面的示例创建新的应用程序域，然后将几个成员值打印到控制台。  
  
 [!code-cpp[AppDomain_Setup#2](../../../samples/snippets/cpp/VS_Snippets_CLR/AppDomain_Setup/CPP/source2.cpp#2)]
 [!code-csharp[AppDomain_Setup#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AppDomain_Setup/CS/source2.cs#2)]
 [!code-vb[AppDomain_Setup#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AppDomain_Setup/VB/source2.vb#2)]  
  
 下面的示例设置并检索某一应用程序域的安装信息。 使用 `AppDomain.SetupInformation.ApplicationBase` 可获取配置信息。  
  
 [!code-cpp[AppDomain_Setup#3](../../../samples/snippets/cpp/VS_Snippets_CLR/AppDomain_Setup/CPP/source3.cpp#3)]
 [!code-csharp[AppDomain_Setup#3](../../../samples/snippets/csharp/VS_Snippets_CLR/AppDomain_Setup/CS/source3.cs#3)]
 [!code-vb[AppDomain_Setup#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AppDomain_Setup/VB/source3.vb#3)]  
  
## <a name="see-also"></a>请参阅

- [对应用程序域进行编程](application-domains.md#programming-with-application-domains)
- [使用应用程序域](use.md)
