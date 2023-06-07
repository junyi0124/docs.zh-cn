---
title: 如何：声明自定义事件以节省内存
ms.date: 07/20/2015
helpviewer_keywords:
- declaring events [Visual Basic], custom
- events [Visual Basic], custom
- custom events [Visual Basic]
ms.assetid: 87ebee87-260c-462f-979c-407874debd19
ms.openlocfilehash: 78934e3e5ae7d5a3f5867c99a9f1db760c65ecbf
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075117"
---
# <a name="how-to-declare-custom-events-to-conserve-memory-visual-basic"></a>如何：声明自定义事件以节省内存 (Visual Basic)

在以下几种情况下，应用程序很重要的是应用程序保持其内存使用率较低。 自定义事件允许应用程序仅对它处理的事件使用内存。  
  
 默认情况下，当类声明事件时，编译器会为字段分配内存以存储事件信息。 如果某个类有许多未使用的事件，则它们将不必要地占用内存。  
  
 您可以使用自定义事件来更仔细地管理内存使用情况，而不是使用 Visual Basic 提供的事件的默认实现。  
  
## <a name="example"></a>示例  

 在此示例中，类使用 <xref:System.ComponentModel.EventHandlerList> 存储在字段中的类的一个实例 `Events` 存储有关正在使用的事件的信息。 <xref:System.ComponentModel.EventHandlerList>类是用于保存委托的优化列表类。  
  
 类中的所有事件使用 `Events` 字段跟踪处理每个事件的方法。  
  
 [!code-vb[VbVbalrEvents#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#22)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.ComponentModel.EventHandlerList>
- [事件](index.md)
- [如何：声明自定义事件以避免阻止](how-to-declare-custom-events-to-avoid-blocking.md)
