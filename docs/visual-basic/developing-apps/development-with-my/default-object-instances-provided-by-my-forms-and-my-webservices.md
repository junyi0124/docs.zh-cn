---
title: My.Forms 和 My.WebServices 提供的默认对象实例
ms.date: 07/20/2015
helpviewer_keywords:
- My.WebServices object [Visual Basic], developing applications
- My.Forms object [Visual Basic], developing applications
- rapid application development (RAD), My.Forms
- rapid application development (RAD), My.WebServices
ms.assetid: de930027-9108-4f0c-b97c-5e7db4d6ef79
ms.openlocfilehash: 141f2f5f98499498d3c6732f7ae8d0abe6259ed9
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990242"
---
# <a name="default-object-instances-provided-by-myforms-and-mywebservices-visual-basic"></a>My.Forms 和 My.WebServices 提供的默认对象实例 (Visual Basic)

[My.Forms](../../language-reference/objects/my-forms-object.md) 和 [My.WebServices](../../language-reference/objects/my-webservices-object.md) 对象提供对应用程序使用的窗体、数据源和 XML Web Service 的访问权限。 它们通过其中每个对象的默认实例的集合来提供访问权限。  
  
## <a name="default-instances"></a>默认实例  

 默认实例是由运行时提供的类的实例，无需使用 `Dim` 和 `New` 语句进行声明和实例化。 下面的示例演示如何声明和实例化名为 `Form1` 的 <xref:System.Windows.Forms.Form> 类的实例，以及现在如何能够通过 `My.Forms` 获取此 <xref:System.Windows.Forms.Form> 类的默认实例。  
  
 [!code-vb[VbVbcnMy#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#5)]  
  
 [!code-vb[VbVbcnMy#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#6)]  
  
 `My.Forms` 对象会为项目中存在的每个 `Form` 类返回默认实例的集合。 同样，`My.WebServices` 为在应用程序中创建了其引用的每个 Web 服务提供代理类的默认实例。  
  
## <a name="see-also"></a>请参阅

- [My.Forms 对象](../../language-reference/objects/my-forms-object.md)
- [My.WebServices 对象](../../language-reference/objects/my-webservices-object.md)
- [My 对项目类型的依赖方式](how-my-depends-on-project-type.md)
