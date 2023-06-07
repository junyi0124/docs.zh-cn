---
title: 版式和代码约定
ms.date: 07/20/2015
helpviewer_keywords:
- coding conventions [Visual Basic], Visual Basic
- best practices [Visual Basic], coding conventions
- conventions [Visual Basic], Visual Basic coding
- typographic conventions [Visual Basic]
- document conventions [Visual Basic]
- conventions [Visual Basic], documentation
- Visual Basic code, conventions
ms.assetid: 1916cd81-ea9d-4faa-81f7-4a0d864b60f4
ms.openlocfilehash: 0e36d9d61b0dd2701210ce614d15fd38f08f5401
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401350"
---
# <a name="typographic-and-code-conventions-visual-basic"></a>版式和代码约定 (Visual Basic)

Visual Basic 文档使用以下排字和代码约定。  
  
## <a name="typographic-conventions"></a>版式约定  
  
|示例|说明|  
|-------------|-----------------|  
|`Sub`, `If`, `ChDir`, `Print`, `True`, `Debug`|特定于语言的关键字和运行时成员具有词首大写字母，并设置格式，如本示例中所示。|  
|**SmallProject**、 **ButtonCollection**|将为您指示键入的单词和短语设置格式，如本示例中所示。|  
|[Module 语句](statements/module-statement.md)|可单击以切换到另一个帮助页的链接的格式如本示例中所示。|  
|*object*、 *variableName*、`argumentList`|您提供的信息的占位符格式如本示例中所示。|  
|[Shadows]，[ *expressionList* ]|在语法中，可选项括在括号中。|  
|{ `Public` &#124; `Friend` &#124; `Private` }|在语法中，当你必须在两个或多个项之间进行选择时，项括在大括号中，并用竖线分隔。<br /><br /> 您必须选择一个（且只有一个）项。|  
|[ `Protected` &#124; `Friend` ]|在语法中，当您有选择两个或多个项之间的选项时，项将括在方括号中，并用竖线分隔。<br /><br /> 您可以选择项的任意组合，或不选择任何项。|  
|[{ `ByVal` &#124; `ByRef` }]|在语法中，如果您可以选择不多个项，但您也可以完全省略项，则项括在括在大括号中的方括号内，并用竖线分隔。|  
|*成员*名称1，*成员名称*2，*成员名称*3|同一个占位符的多个实例通过下标来区分，如示例中所示。|  
|*1*<br /><br /> ...<br /><br /> *memberNameN*|在语法中，省略号（...）用于指示直接在省略号前面的类型的无限项。<br /><br /> 在代码中，省略号表示为清楚起见省略了代码。|  
|ESC，ENTER|键盘上的键名和键序列均以大写字母显示。|  
|Alt+F1|当键名称之间出现加号（+）时，必须在按下一个键的同时按下一个键。 例如，ALT + F1 表示按下 F1 键时按下 ALT 键。|  
  
## <a name="code-conventions"></a>代码约定  
  
|示例|说明|  
|-------------|-----------------|  
|`sampleString = "Hello, world!"`|代码示例以固定间距字体显示，并设置格式，如本示例中所示。|  
|上一语句将的值设置 `sampleString` 为 "Hello，world！"|解释性文本中的代码元素显示为固定间距字体，如本示例中所示。|  
|`' This is a comment.`<br /><br /> `REM This is also a comment.`|代码注释由撇号（'）或 REM 关键字引入。|  
|`sampleVar = "This is an " _`<br /><br /> `& "example" _`<br /><br /> `& " of how to continue code."`|位于行末尾的空格后跟下划线（_）指示该语句在下一行继续。|  
  
## <a name="see-also"></a>另请参阅

- [Visual Basic 语言参考](index.md)
- [关键字](keywords/index.md)
- [Visual Basic 运行库成员](runtime-library-members.md)
- [Visual Basic 命名约定](../programming-guide/program-structure/naming-conventions.md)
- [如何：在代码中拆分和合并语句](../programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
- [代码中的注释](../programming-guide/program-structure/comments-in-code.md)
