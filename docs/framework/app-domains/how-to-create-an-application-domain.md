---
title: 如何：创建应用程序域
description: 了解如何在 .NET 中创建应用程序域。 可以创建应用程序域来加载亲自管理的程序集，也可以创建应用程序域来执行代码。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- application domains, creating
ms.assetid: ba1fa43e-49f5-47d9-bd7f-3024af16f4ba
ms.openlocfilehash: fb022511ee395a9312e4dbaf7c0beee03c9b4569
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264083"
---
# <a name="how-to-create-an-application-domain"></a>如何：创建应用程序域

需要应用程序域时，公共语言运行时主机会自动创建它们。 但是，可以创建自己的应用程序域并将其加载到需要亲自管理的程序集中。 也可以创建从中执行代码的应用程序域。  
  
 使用 <xref:System.AppDomain?displayProperty=nameWithType> 类中某个重载的 CreateDomain 方法，创建新的应用程序域。 可以为应用程序域命名并按名称来引用应用程序域。  
  
 以下示例创建新的应用程序域，并为它指定名称 `MyDomain`，然后将主机域和新创建的子应用程序域的名称打印到控制台。  
  
## <a name="example"></a>示例  

 [!code-cpp[ADCreateDomain#2](../../../samples/snippets/cpp/VS_Snippets_CLR/ADCreateDomain/CPP/source2.cpp#2)]
 [!code-csharp[ADCreateDomain#2](../../../samples/snippets/csharp/VS_Snippets_CLR/ADCreateDomain/CS/source2.cs#2)]
 [!code-vb[ADCreateDomain#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/ADCreateDomain/VB/source2.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [对应用程序域进行编程](application-domains.md#programming-with-application-domains)
- [使用应用程序域](use.md)
