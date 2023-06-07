---
title: 声明和引发事件
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], events
- events [Visual Basic], walkthroughs
- declaring events [Visual Basic], walkthroughs
- events [Visual Basic], declaring
- events [Visual Basic], raising
- raising events [Visual Basic], walkthroughs
ms.assetid: 8ffb3be8-097d-4d3c-b71e-04555ebda2a2
ms.openlocfilehash: 07ef611b50cfa13f77fa168d58dd3b43e97eeec6
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91057983"
---
# <a name="walkthrough-declaring-and-raising-events-visual-basic"></a>演练：声明和引发事件 (Visual Basic)

本演练演示如何声明和引发名为的类的事件 `Widget` 。 完成这些步骤后，你可能需要阅读相关主题 [演练：处理事件](walkthrough-handling-events.md)，其中演示了如何使用对象中的事件 `Widget` 在应用程序中提供状态信息。  
  
## <a name="the-widget-class"></a>小组件类  

 假设你有一个 `Widget` 类。 你的 `Widget` 类有一个可能需要很长时间才能执行的方法，并且你希望你的应用程序能够提供某种类型的完成指示器。  
  
 当然，您可以使 `Widget` 对象显示一个完成百分比对话框，但随后在您使用该类的每个项目中都将出现该对话框 `Widget` 。 对象设计的一个很好的原则是让使用对象的应用程序处理用户界面，除非对象的全部用途是管理窗体或对话框。  
  
 的用途 `Widget` 是执行其他任务，因此最好添加一个 `PercentDone` 事件并让调用 `Widget` 的方法的过程处理该事件并显示状态更新。 `PercentDone`事件还可以提供取消任务的机制。  
  
#### <a name="to-build-the-code-example-for-this-topic"></a>生成本主题的代码示例  
  
1. 打开一个新的 Visual Basic Windows 应用程序项目，并创建一个名为的窗体 `Form1` 。  
  
2. 向添加两个按钮和一个标签 `Form1` 。  
  
3. 按下表所示命名对象。  
  
    |Object|属性|设置|  
    |------------|--------------|-------------|  
    |`Button1`|`Text`|启动任务|  
    |`Button2`|`Text`|取消|  
    |`Label`|`(Name)`, `Text`|lblPercentDone，0|  
  
4. 在 " **项目** " 菜单上，选择 " **添加类** "，将名为的类添加 `Widget.vb` 到项目。  
  
#### <a name="to-declare-an-event-for-the-widget-class"></a>为小组件类声明事件  
  
- 使用 `Event` 关键字声明类中的事件 `Widget` 。 请注意，事件可以具有 `ByVal` 和 `ByRef` 参数，如 `Widget` 事件所示 `PercentDone` ：  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#1)]  
  
 调用对象收到 `PercentDone` 事件时，该 `Percent` 参数包含已完成的任务的百分比。 `Cancel`参数可以设置为 `True` ，以取消引发事件的方法。  
  
> [!NOTE]
> 可以声明事件自变量，就像处理过程的参数一样，但有以下例外：事件不能包含 `Optional` 或 `ParamArray` 参数，并且事件没有返回值。  
  
 此 `PercentDone` 事件由 `LongTask` 类的方法引发 `Widget` 。 `LongTask` 采用两个参数：方法在经过一段时间后进行工作，并在暂停之前的最小时间间隔内 `LongTask` 引发 `PercentDone` 事件。  
  
#### <a name="to-raise-the-percentdone-event"></a>引发 PercentDone 事件  
  
1. 若要简化对 `Timer` 此类使用的属性的访问，请将 `Imports` 语句添加到类模块的 "声明" 部分的顶部、语句的上方 `Class Widget` 。  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#2)]  
  
2. 将下面的代码添加到 `Widget` 类中:  
  
     [!code-vb[VbVbcnWalkthroughDeclaringAndRaisingEvents#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnWalkthroughDeclaringAndRaisingEvents/VB/Widget.vb#3)]  
  
 当应用程序调用 `LongTask` 方法时， `Widget` 类 `PercentDone` 每秒引发一次事件 `MinimumInterval` 。 当事件返回时，将 `LongTask` 检查 `Cancel` 参数是否设置为 `True` 。  
  
 此处有一些免责声明是必需的。 为简单起见，该 `LongTask` 过程假定您事先知道任务将花多长时间。 几乎不会出现这种情况。 将任务划分为偶大的块可能比较困难，通常，用户最重要的是，用户只需经过一段时间，就能看出发生了什么情况。  
  
 在此示例中，可能已发现另一个缺陷。 `Timer`属性返回从午夜开始经过的秒数; 因此，如果该应用程序在午夜之前启动，则会停滞。 更仔细地测量时间的方法会将这类边界情况（例如这种情况）考虑在内，或使用属性（例如）来避免这种情况 `Now` 。  
  
 由于 `Widget` 该类可以引发事件，因此你可以转到下一个演练。 [演练：处理事件](walkthrough-handling-events.md) 演示如何使用将 `WithEvents` 事件处理程序与 `PercentDone` 事件关联。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.DateAndTime.Timer%2A>
- <xref:Microsoft.VisualBasic.DateAndTime.Now%2A>
- [演练：处理事件](walkthrough-handling-events.md)
- [事件](index.md)
