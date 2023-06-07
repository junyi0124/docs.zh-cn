---
title: 使用 My.Application、My.Computer 和 My.User 执行任务
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application object [Visual Basic], developing applications
- rapid application development (RAD), My.Application
- rapid application development (RAD), My.Computer
- rapid application development (RAD), My.User
- My.Computer object [Visual Basic], developing applications
- My.User object [Visual Basic], developing applications
ms.assetid: c8af61bd-4dd3-4a0f-9af5-795b594b240b
ms.openlocfilehash: 55961d6857b690fc2726f541df8a5497a3207928
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411689"
---
# <a name="performing-tasks-with-myapplication-mycomputer-and-myuser-visual-basic"></a>使用 My.Application、My.Computer 和 My.User 执行任务 (Visual Basic)

三个主要 `My` 对象为 `My.Application` (<xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>)、`My.Computer` (<xref:Microsoft.VisualBasic.Devices.Computer>) 和 `My.User` (<xref:Microsoft.VisualBasic.ApplicationServices.User>)，这些对象提供对信息和常用功能的访问权限。 借助这些对象，分别可以访问与当前应用程序、安装该应用程序的计算机或该应用程序的当前用户相关的信息。  
  
## <a name="myapplication-mycomputer-and-myuser"></a>My.Application、My.Computer 和 My.User  

 以下示例演示如何使用 `My` 检索信息。  
  
 [!code-vb[VbVbcnMy#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#1)]  
  
 [!code-vb[VbVbcnMy#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#2)]  
  
 通过这三个对象公开的成员，除了可以检索信息以外，你还可以执行与该对象相关的方法。 例如，可以通过 `My.Computer` 访问多种方法，来操控文件或更新注册表。  
  
 借助 `My`（其中包括用于操控文件、目录和驱动器的多种方法和属性），文件 I/O 会明显简化、加速。 借助 <xref:Microsoft.VisualBasic.FileIO.TextFieldParser> 对象，你可以读取大型结构化文件，其中包含带分隔符或固定宽度的字段。 此示例打开 `TextFieldParser` `reader`，并使用它读取 `C:\TestFolder1\test1.txt`。  
  
 [!code-vb[VbVbalrTextFieldParser#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#23)]  
  
 借助 `My.Application`，你可以应用程序的区域性。 以下示例演示如何调用此方法。  
  
 [!code-vb[VbVbcnMy#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMy/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ApplicationServices.ApplicationBase>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.ApplicationServices.User>
- [My 对项目类型的依赖方式](how-my-depends-on-project-type.md)
