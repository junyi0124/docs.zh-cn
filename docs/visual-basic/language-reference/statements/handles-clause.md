---
title: Handles 子句
ms.date: 07/20/2015
f1_keywords:
- Handles
- vb.Handles
helpviewer_keywords:
- Handles keyword [Visual Basic]
ms.assetid: 1b051c0e-f499-42f6-acb5-6f4f27824b40
ms.openlocfilehash: 347f521267d4fd954ac359ab25ed5810cfd71d34
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873250"
---
# <a name="handles-clause-visual-basic"></a>Handles 子句 (Visual Basic)

声明某一过程可处理指定的事件。  
  
## <a name="syntax"></a>语法  
  
```vb  
proceduredeclaration Handles eventlist  
```  
  
## <a name="parts"></a>组成部分  

 `proceduredeclaration`  
 将处理事件的过程的 `Sub` 过程声明。  
  
 `eventlist`  
 `proceduredeclaration` 要处理的事件的列表，以逗号分隔。 事件必须由当前类的基类引发，或由使用 `WithEvents` 关键字声明的对象引发。  
  
## <a name="remarks"></a>备注  

 在过程声明的结尾使用 `Handles` 关键字，以使其处理由使用 `WithEvents` 关键字声明的对象变量引发的事件。 `Handles` 关键字还可在派生类中用于处理来自基类的事件。  
  
 `Handles` 关键字和 `AddHandler` 语句都允许你指定特定过程处理特定事件，但存在差异。 定义过程时使用 `Handles` 关键字，以指定它处理特定事件。 `AddHandler` 语句在运行时将过程连接到事件。 有关详细信息，请参阅 [AddHandler 语句](addhandler-statement.md)。  
  
 对于自定义事件，应用程序会在添加过程作为事件处理程序时，调用事件的 `AddHandler` 访问器。 有关自定义事件的详细信息，请参阅 [Event 语句](event-statement.md)。  
  
## <a name="example"></a>示例  

 [!code-vb[VbVbalrEvents#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#2)]  
  
 下面的示例演示派生类可以如何使用 `Handles` 语句处理来自基类的事件。  
  
 [!code-vb[VbVbalrEvents#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#3)]  
  
## <a name="example"></a>示例  

 下面的示例包含 **WPF 应用程序** 项目的两个按钮事件处理程序。  
  
 [!code-vb[VbVbalrEvents#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/class3.vb#41)]  
  
## <a name="example"></a>示例  

 下面的示例与前面的示例等效。 `Handles` 子句中的 `eventlist` 包含两个按钮的事件。  
  
 [!code-vb[VbVbalrEvents#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/class3.vb#42)]  
  
## <a name="see-also"></a>另请参阅

- [WithEvents](../modifiers/withevents.md)
- [AddHandler 语句](addhandler-statement.md)
- [RemoveHandler 语句](removehandler-statement.md)
- [Event 语句](event-statement.md)
- [RaiseEvent 语句](raiseevent-statement.md)
- [事件](../../programming-guide/language-features/events/index.md)
