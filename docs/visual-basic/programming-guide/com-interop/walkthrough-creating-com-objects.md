---
title: 演练：创建 COM 对象
ms.date: 07/20/2015
helpviewer_keywords:
- COM interop [Visual Basic], creating COM objects
- COM objects, creating
- COM interop [Visual Basic], walkthroughs
- object creation [Visual Basic], COM objects
- COM objects, walkthroughs
ms.assetid: 7b07a463-bc72-4392-9ba0-9dfcb697a44f
ms.openlocfilehash: 90a21b70b45902a9f4fd559a97e777f26043fffb
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075611"
---
# <a name="walkthrough-creating-com-objects-with-visual-basic"></a>演练：使用 Visual Basic 创建 COM 对象

创建新的应用程序或组件时，最好创建 .NET Framework 程序集。 不过，Visual Basic 还可以轻松地向 COM 公开 .NET Framework 组件。 这使你可以为需要 COM 组件的早期应用程序套件提供新组件。 本演练演示了如何使用 Visual Basic 将 .NET Framework 对象公开为 COM 对象，无论使用 COM 类模板还是不使用 COM 类模板。  
  
 公开 COM 对象的最简单方法是使用 COM 类模板。 此模板创建一个新类，然后将项目配置为生成具有互操作性层作为 COM 对象的类，并将其注册到操作系统。  
  
> [!NOTE]
> 尽管还可以公开在 Visual Basic 中创建的类作为要使用的非托管代码的 COM 对象，但它不是真正的 COM 对象，不能由 Visual Basic 使用。 有关详细信息，请参阅 [.NET Framework 应用程序中的 COM 互操作性](com-interoperability-in-net-framework-applications.md)。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-a-com-object-by-using-the-com-class-template"></a>使用 COM 类模板创建 COM 对象  
  
1. 单击 "**新建项目**"，从 "**文件**" 菜单打开新的 Windows 应用程序项目。  
  
2. 在 " **新建项目** " 对话框中的 " **项目类型** " 字段下，选中 "Windows" 处于选中状态。 从 "**模板**" 列表中选择 **"类库"** ，然后单击 **"确定"**。 将显示新项目。  
  
3. 从 "**项目**" 菜单中选择 "**添加新项**"。 随即出现“添加新项”对话框。  
  
4. 从 "**模板**" 列表中选择**COM 类**，然后单击 "**添加**"。 Visual Basic 添加一个新类，并为 COM 互操作配置新的项目。  
  
5. 向 COM 类中添加代码，如属性、方法和事件。  
  
6. 从 "**生成**" 菜单中选择 "**生成 classlibrary1.chainone** "。 Visual Basic 生成程序集并向操作系统注册 COM 对象。  
  
## <a name="creating-com-objects-without-the-com-class-template"></a>不带 COM 类模板创建 COM 对象  

 你还可以手动创建 COM 类，而不是使用 COM 类模板。 当你在命令行中工作或需要更好地控制如何定义 COM 对象时，此过程非常有用。  
  
#### <a name="to-set-up-your-project-to-generate-a-com-object"></a>设置项目以生成 COM 对象  
  
1. 单击 " **NewProject**"，从 "**文件**" 菜单打开新的 Windows 应用程序项目。  
  
2. 在 " **新建项目** " 对话框中的 " **项目类型** " 字段下，选中 "Windows" 处于选中状态。 从 "**模板**" 列表中选择 **"类库"** ，然后单击 **"确定"**。 将显示新项目。  
  
3. 在“解决方案资源管理器”中，右键单击项目，然后单击“属性” 。 随即显示 " **项目设计器** "。  
  
4. 单击“编译”选项卡。  
  
5. 选中 " **为 COM 互操作注册** " 复选框。  
  
#### <a name="to-set-up-the-code-in-your-class-to-create-a-com-object"></a>在类中设置代码以创建 COM 对象  
  
1. 在 **解决方案资源管理器**中，双击 " **Class1** " 以显示其代码。  
  
2. 将类重命名为 `ComClass1`。  
  
3. 将以下常量添加到 `ComClass1` 。 它们将存储全局唯一标识符 (GUID) 需要 COM 对象的常量。  
  
     [!code-vb[VbVbalrInterop#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#2)]  
  
4. 在“工具”**** 菜单上，单击“创建 Guid”****。 在“创建 GUID”**** 对话框中，单击“注册表格式”****，然后单击“复制”****。 单击 **“退出”**。  
  
5. 用 GUID 替换的空字符串 `ClassId` ，同时删除前导大括号和尾随大括号。 例如，如果 Guidgen.exe 提供的 GUID 为， `"{2C8B0AEE-02C9-486e-B809-C780A11530FE}"` 则代码应如下所示。  
  
     [!code-vb[VbVbalrInterop#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#3)]  
  
6. 对和常量重复前面的 `InterfaceId` 步骤 `EventsId` ，如以下示例中所示。  
  
     [!code-vb[VbVbalrInterop#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#4)]  
  
    > [!NOTE]
    > 请确保 Guid 是新的且唯一的;否则，COM 组件可能会与其他 COM 组件发生冲突。  
  
7. 将 `ComClass` 属性添加到 `ComClass1` ，并为类 Id、接口 ID 和事件 ID 指定 guid，如以下示例中所示：  
  
     [!code-vb[VbVbalrInterop#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#5)]  
  
8. COM 类必须具有无参数的 `Public Sub New()` 构造函数，否则类将不会正确注册。 向类添加无参数构造函数：  
  
     [!code-vb[VbVbalrInterop#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#6)]  
  
9. 向类添加属性、方法和事件，并将其以语句结束 `End Class` 。 从 "**生成**" 菜单中选择 "**生成解决方案**"。 Visual Basic 生成程序集并向操作系统注册 COM 对象。  
  
    > [!NOTE]
    > 使用 Visual Basic 生成的 COM 对象无法由其他 Visual Basic 应用程序使用，因为它们不是真正的 COM 对象。 尝试添加对此类 COM 对象的引用将引发错误。 有关详细信息，请参阅 [.NET Framework 应用程序中的 COM 互操作性](com-interoperability-in-net-framework-applications.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.ComClassAttribute>
- [COM 互操作](index.md)
- [演练：使用 COM 对象实现继承](walkthrough-implementing-inheritance-with-com-objects.md)
- [#Region 指令](../../language-reference/directives/region-directive.md)
- [.NET Framework 应用程序中的 COM 互操作性](com-interoperability-in-net-framework-applications.md)
- [互操作性疑难解答](troubleshooting-interoperability.md)
