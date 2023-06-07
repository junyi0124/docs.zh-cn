---
title: 使用 My 开发
ms.date: 07/20/2015
f1_keywords:
- My.MyWpfExtension.Windows
helpviewer_keywords:
- My object
- My namespace
- My feature
- Visual Basic, programming in
ms.assetid: f1d04509-5e46-4551-9f9f-94334a121fca
ms.openlocfilehash: 3befac591de8fbc7250777a8b87247ee395abf25
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "85840284"
---
# <a name="development-with-my-visual-basic"></a>使用 My 开发 (Visual Basic)

Visual Basic 提供了支持快速应用程序开发的新功能，不仅功能强大，还有助于提高工作效率和易用性。 其中一项功能称为 `My`，可提供对与应用程序及其运行时环境相关的信息和默认对象实例的访问权限。 此类信息按可通过 IntelliSense 搜索的格式进行编排，并根据用途进行了逻辑界定。  
  
 `My` 的顶级成员作为对象公开。 每个对象的行为类似于包含 `Shared` 成员的命名空间或类，可公开一组相关成员。  
  
 下表展示了顶级 `My` 对象及其相互之间的关系。  
  
 ![显示 My 的对象模型的示意图。](./media/index/my-object-model-relationships.gif)  
  
## <a name="in-this-section"></a>本节内容  

 [使用 My.Application、My.Computer 和 My.User 执行任务](performing-tasks-with-my-application-my-computer-and-my-user.md)  
 介绍了三个主要 `My` 对象（`My.Application`、`My.Computer` 和 `My.User`），这些对象提供对信息和功能的访问权限  
  
 [My.Forms 和 My.WebServices 提供的默认对象实例](default-object-instances-provided-by-my-forms-and-my-webservices.md)  
 介绍了 `My.Forms` 和 `My.WebServices` 对象，这些对象提供对应用程序使用的窗体、数据源和 XML Web Service 的访问权限。  
  
 [使用 My.Resources 和 My.Settings 快速开发应用程序](rapid-application-development-with-my-resources-and-my-settings.md)  
 介绍了 `My.Resources` 和 `My.Settings` 对象，这些对象提供对应用程序资源和设置的访问权限。  
  
 [Visual Basic 应用程序模型概述](overview-of-the-visual-basic-application-model.md)  
 介绍 Visual Basic 应用程序启动/关闭模型。  
  
 [My 对项目类型的依赖方式](how-my-depends-on-project-type.md)  
 详细介绍了不同项目类型中可用的 `My` 功能。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [My.Forms 对象](../../language-reference/objects/my-forms-object.md)
- [My.WebServices 对象](../../language-reference/objects/my-webservices-object.md)
- [My 对项目类型的依赖方式](how-my-depends-on-project-type.md)
