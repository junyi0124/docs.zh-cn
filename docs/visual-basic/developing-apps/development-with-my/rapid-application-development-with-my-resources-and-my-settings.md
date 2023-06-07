---
title: 使用 My.Resources 和 My.Settings 快速开发应用程序
ms.date: 07/20/2015
helpviewer_keywords:
- My.Settings object [Visual Basic], developing applications
- rapid application development (RAD), My.Resources
- rapid application development (RAD), My.Settings
- My.Resources object [Visual Basic], developing applications
ms.assetid: 68284ab1-b685-4814-a2a4-01ae40445ff8
ms.openlocfilehash: fd1ec25582e919b84235502f5921edfbc6e1dade
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990202"
---
# <a name="rapid-application-development-with-myresources-and-mysettings-visual-basic"></a>使用 My.Resources 和 My.Settings 快速开发应用程序 (Visual Basic)

`My.Resources` 对象提供对应用程序资源的访问权限，并允许你动态检索应用程序的资源。  
  
## <a name="retrieving-resources"></a>检索资源  

 某些资源（如音频文件、图标、图像和字符串）可通过 `My.Resources` 对象进行检索。 例如，你可以访问应用程序特定于区域性的资源文件。 下面的示例将窗体图标设置为存储在应用程序的资源文件中的名为 `Form1Icon` 的图标。  
  
 [!code-vb[VbVbcnMy#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#7)]  
  
 `My.Resources` 对象只公开全局资源。 它不提供对与窗体关联的资源文件的访问权限。 必须从窗体访问窗体资源。  
  
 同样，`My.Settings` 对象提供对应用程序设置的访问，并允许你动态地存储和检索应用程序的属性设置和其他信息。 有关详细信息，请参阅 [My.Resources 对象](../../language-reference/objects/my-resources-object.md)和 [My.Settings 对象](../../language-reference/objects/my-settings-object.md)。  
  
## <a name="see-also"></a>请参阅

- [My.Resources 对象](../../language-reference/objects/my-resources-object.md)
- [My.Settings 对象](../../language-reference/objects/my-settings-object.md)
- [访问应用程序设置](../programming/app-settings/index.md)
