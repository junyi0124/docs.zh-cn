---
title: 创建和实现接口
ms.date: 07/20/2015
helpviewer_keywords:
- interfaces [Visual Basic], walkthroughs
- interfaces [Visual Basic], testing
- interface implementation [Visual Basic], walkthrough
- interfaces [Visual Basic], creating
ms.assetid: ded82af2-9f52-4232-98ef-fe458180f112
ms.openlocfilehash: 89e8f91a04b25b84bc783d5c4f4b91cf12426f72
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405062"
---
# <a name="walkthrough-creating-and-implementing-interfaces-visual-basic"></a>演练：创建和实现接口 (Visual Basic)

接口描述属性、方法和事件的特征，但使实现详细信息保留在结构或类中。  
  
 本演练演示如何声明和实现接口。  
  
> [!NOTE]
> 本演练不提供有关如何创建用户界面的信息。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="to-define-an-interface"></a>定义接口
  
1. 打开一个新的 Visual Basic Windows 应用程序项目。  
  
2. 通过单击 "**项目**" 菜单上的 "**添加模块**"，将新模块添加到项目。  
  
3. 将新模块命名为 `Module1.vb` ，然后单击 "**添加**"。 新模块的代码随即显示。  
  
4. `TestInterface` `Module1` 通过在 `Interface TestInterface` `Module` 和语句之间键入，然后 `End Module` 按 enter，在中定义名为的接口。 **代码编辑器**会缩进 `Interface` 关键字并添加 `End Interface` 语句来形成代码块。  
  
5. 通过在和语句之间放置以下代码，为接口定义属性、方法和事件 `Interface` `End Interface` ：  
  
     [!code-vb[VbVbalrOOP#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#98)]
  
## <a name="implementation"></a>实现

 你可能会注意到，用于声明接口成员的语法不同于用于声明类成员的语法。 这种差异反映了接口不能包含实现代码这一事实。  
  
### <a name="to-implement-the-interface"></a>实现接口
  
1. 添加一个名为 `ImplementationClass` 的类，方法是将以下语句添加到 `Module1` ，在 `End Interface` 语句后面但在语句之前，然后 `End Module` 按 enter：  
  
     [!code-vb[VbVbalrOOP#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#99)]
  
     如果在集成开发环境中工作，则按 ENTER 时，**代码编辑器**将提供一个匹配的 `End Class` 语句。  
  
2. 将以下 `Implements` 语句添加到 `ImplementationClass` ，命名类实现的接口：  
  
     [!code-vb[VbVbalrOOP#100](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#100)]
  
     当与类或结构顶部的其他项分开列出时， `Implements` 语句指示类或结构实现接口。  
  
     如果你在集成开发环境中工作，则在按下 ENTER 键时，**代码编辑器**将实现所需的类成员， `TestInterface` 并且你可以跳过下一步。  
  
3. 如果未在集成开发环境中工作，则必须实现该接口的所有成员 `MyInterface` 。 将以下代码添加到以 `ImplementationClass` 实现 `Event1` 、 `Method1` 和 `Prop1` ：  
  
     [!code-vb[VbVbalrOOP#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#101)]
  
     `Implements`语句命名要实现的接口和接口成员。  
  
4. 完成的定义， `Prop1` 方法是将私有字段添加到存储属性值的类：  
  
     [!code-vb[VbVbalrOOP#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#102)]
  
     `pval`从属性 get 访问器返回的值。  
  
     [!code-vb[VbVbalrOOP#103](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#103)]
  
     `pval`在属性集访问器中设置的值。  
  
     [!code-vb[VbVbalrOOP#104](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#104)]
  
5. `Method1`通过添加以下代码完成的定义。  
  
     [!code-vb[VbVbalrOOP#105](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#105)]
  
### <a name="to-test-the-implementation-of-the-interface"></a>测试接口的实现
  
1. 右键单击 "**解决方案资源管理器**中项目的启动窗体，然后单击"**查看代码**"。 编辑器显示您的启动窗体的类。 默认情况下，将调用启动窗体 `Form1` 。  
  
2. 将以下 `testInstance` 字段添加到 `Form1` 类：  
  
     [!code-vb[VbVbalrOOP#120](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#120)]
  
     通过将声明 `testInstance` 为 `WithEvents` ， `Form1` 类可处理其事件。  
  
3. 向类添加以下事件处理程序 `Form1` ，以处理引发的事件 `testInstance` ：  
  
     [!code-vb[VbVbalrOOP#106](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#106)]
  
4. 将名 `Test` 为的子程序添加到 `Form1` 类，以测试实现类：  
  
     [!code-vb[VbVbalrOOP#107](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#107)]
  
     此 `Test` 过程创建实现的类的一个实例 `MyInterface` ，将该实例分配给该 `testInstance` 字段，设置一个属性，然后通过接口运行方法。  
  
5. 添加代码以 `Test` 从 `Form1 Load` 启动窗体的过程调用过程：  
  
     [!code-vb[VbVbalrOOP#108](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#108)]
  
6. `Test`按 F5 运行该过程。 将显示消息 "Prop1 已设置为 9"。 单击 "确定" 后，将显示消息 "Method1 的 X 参数是 5"。 单击 "确定"，将显示消息 "事件处理程序捕获到事件"。  
  
## <a name="see-also"></a>另请参阅

- [Implements 语句](../../../language-reference/statements/implements-statement.md)
- [接口](index.md)
- [Interface 语句](../../../language-reference/statements/interface-statement.md)
- [Event 语句](../../../language-reference/statements/event-statement.md)
