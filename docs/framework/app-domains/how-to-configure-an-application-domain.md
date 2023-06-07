---
title: 如何：配置应用程序域
description: 在 .NET 中配置应用程序域。 可以使用 AppDomainSetup 类为新应用程序域提供含配置信息的 CLR。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- application domains, configuring
- ApplicationBase property
ms.assetid: 07ea8438-7a34-49f0-a7e8-3d6ff7e4a482
ms.openlocfilehash: bbda1f75da6b994c54cd71c56f16615a3758859b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96271507"
---
# <a name="how-to-configure-an-application-domain"></a>如何：配置应用程序域

可以使用 <xref:System.AppDomainSetup> 类为新应用程序域提供含配置信息的公共语言运行时。 创建自己的应用程序域时，最重要的属性是 <xref:System.AppDomainSetup.ApplicationBase%2A>。 其他 AppDomainSetup 属性主要由运行时宿主用于配置特殊的应用程序域。  
  
 ApplicationBase 属性定义应用程序的根目录。 当运行时需要满足类型请求时，它会在 ApplicationBase 属性指定的目录中探测包含该类型的程序集。  
  
> [!NOTE]
> 新的应用程序域只继承创建者的 ApplicationBase 属性。  
  
 以下示例创建 AppDomainSetup 类的实例，使用此类创建新的应用程序域，将信息写入控制台，然后卸载应用程序域。  
  
## <a name="example"></a>示例  

 [!code-cpp[ADApplicationBase#2](../../../samples/snippets/cpp/VS_Snippets_CLR/ADApplicationBase/CPP/source2.cpp#2)]
 [!code-csharp[ADApplicationBase#2](../../../samples/snippets/csharp/VS_Snippets_CLR/ADApplicationBase/CS/source2.cs#2)]
 [!code-vb[ADApplicationBase#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/ADApplicationBase/VB/source2.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [对应用程序域进行编程](application-domains.md#programming-with-application-domains)
- [使用应用程序域](use.md)
