---
title: 定义类
ms.date: 07/20/2015
helpviewer_keywords:
- execution [Visual Basic], ending
- objects [Visual Basic], initializing
- Initialize event [Visual Basic]
- files [Visual Basic], closing
- programs [Visual Basic], quitting
- code, exiting
- objects [Visual Basic], creating
- program termination
- classes [Visual Basic], walkthroughs
- class modules, walkthroughs
- Terminate event [Visual Basic]
- execution [Visual Basic], stopping
ms.assetid: 07018828-2d49-4cf5-a44b-19fb15d9efea
ms.openlocfilehash: 646c6ce307131f3edba194af19920eade9c1753c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84389109"
---
# <a name="walkthrough-defining-classes-visual-basic"></a>演练：定义类 (Visual Basic)

本演练演示如何定义类，然后可以使用这些类来创建对象。 它还演示了如何将属性和方法添加到新类，并演示了如何初始化对象。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="to-define-a-class"></a>定义类
  
1. 通过单击 "**文件**" 菜单上的 "**新建项目**" 来创建项目。 将显示“新建项目”对话框。  
  
2. 从 Visual Basic 项目模板列表中选择 "Windows 应用程序" 以显示新项目。  
  
3. 通过单击 "**项目**" 菜单上的 "**添加类**"，将新类添加到项目。 此时会显示“添加新项”对话框。****  
  
4. 选择 "**类**" 模板。  
  
5. 将新类命名为 `UserNameInfo.vb` ，然后单击 "**添加**" 以显示新类的代码。  
  
     [!code-vb[VbVbalrOOP#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#5)]
  
    > [!NOTE]
    > 你可以使用 Visual Basic**代码编辑器**将类添加到启动窗体中，方法是键入 `Class` 关键字，然后键入新类的名称。 **代码编辑器**为您提供相应的 `End Class` 语句。  
  
6. 通过在和语句之间添加以下代码，为类定义私有字段 `Class` `End Class` ：  
  
     [!code-vb[VbVbalrOOP#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#7)]
  
     将字段声明为 `Private` 意味着它只能在类中使用。 可以通过使用访问修饰符（如） `Public` 提供更多访问权限，使字段从类的外部可用。 有关详细信息，请参阅[Visual Basic 中的访问级别](../declared-elements/access-levels.md)。  
  
7. 通过添加以下代码来定义类的属性：  
  
     [!code-vb[VbVbalrOOP#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#8)]
  
8. 通过添加以下代码为类定义方法：  
  
     [!code-vb[VbVbalrOOP#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#9)]
  
9. 通过添加名为的过程为新类定义参数化构造函数 `Sub New` ：  
  
     [!code-vb[VbVbalrOOP#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#10)]
  
     `Sub New`当创建基于此类的对象时，将自动调用构造函数。 此构造函数设置包含用户名的字段的值。  
  
## <a name="to-create-a-button-to-test-the-class"></a>创建用于测试类的按钮
  
1. 将启动窗体更改为设计模式，方法是在**解决方案资源管理器**中右键单击其名称，然后单击 "**视图设计器**"。 默认情况下，Windows 应用程序项目的启动窗体命名为 "Form1"。 然后，将显示主窗体。  
  
2. 向主窗体添加一个按钮，然后双击它显示 `Button1_Click` 事件处理程序的代码。 添加以下代码来调用测试过程：  
  
     [!code-vb[VbVbalrOOP#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#12)]
  
## <a name="to-run-your-application"></a>运行应用程序
  
1. 按 F5 运行应用程序。 单击窗体上的按钮以调用测试过程。 它将显示一条消息，指出原始 `UserName` 是 "尧，胡继"，因为过程调用了 `Capitalize` 对象的方法。  
  
2. 单击“确定”，关闭该消息框****。 此 `Button1 Click` 过程将更改属性的值 `UserName` ，并显示一条消息，指出的新值 `UserName` 为 "Worden，Joe"。  
  
## <a name="see-also"></a>另请参阅

- [面向对象的编程 (Visual Basic)](../../concepts/object-oriented-programming.md)
- [对象和类](index.md)
