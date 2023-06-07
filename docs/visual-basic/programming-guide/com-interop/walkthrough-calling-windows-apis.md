---
title: 演练：调用 Windows API
ms.date: 07/20/2015
helpviewer_keywords:
- DLLs, calling
- Windows API, walkthroughs
- platform invoke, walkthroughs
- API calls [Visual Basic], walkthroughs [Visual Basic]
- Windows API, calling
- walkthroughs [Visual Basic], API calls
- DllImport attribute, calling Windows API
- Declare statement [Visual Basic], declaring DLL functions
ms.assetid: 9280ca96-7a93-47a3-8d01-6d01be0657cb
ms.openlocfilehash: 88b3df2f18add6641d0355d2c605bc5f74dabbc7
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91098315"
---
# <a name="walkthrough-calling-windows-apis-visual-basic"></a>演练：调用 Windows API (Visual Basic)

Windows Api 是 (Dll) 的动态链接库，这是 Windows 操作系统的一部分。 当难以编写您自己的等效过程时，可以使用它们来执行任务。 例如，Windows 提供了一个名为 `FlashWindowEx` 的函数，使你可以使应用程序的标题栏在浅色和暗色之间交替。  
  
 在您的代码中使用 Windows Api 的优势在于它们可以节省开发时间，因为它们包含已经编写并等待使用的数十个有用函数。 缺点在于，当出现问题时，Windows Api 可能难以处理并铁面无私。  
  
 Windows Api 表示一种特殊的互操作性类别。 Windows Api 不使用托管代码，不具备内置类型库，而是使用与 Visual Studio 使用的数据类型不同的数据类型。 由于存在这些差异，并且 Windows Api 不是 COM 对象，因此，使用平台调用或 PInvoke 执行与 Windows Api 和 .NET Framework 的互操作性。 平台调用是一项服务，它使托管代码能够调用 Dll 中实现的非托管函数。 有关详细信息，请参阅 [使用非托管 DLL 函数](../../../framework/interop/consuming-unmanaged-dll-functions.md)。 可以通过使用 `Declare` 语句或将 `DllImport` 属性应用于空过程，在 Visual Basic 中使用 PInvoke。  
  
 Windows API 调用是过去 Visual Basic 编程的重要组成部分，但在 Visual Basic .NET 中很少需要。 应尽可能使用 .NET Framework 中的托管代码来执行任务，而不是 Windows API 调用。 本演练介绍了需要使用 Windows Api 的情况的信息。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="api-calls-using-declare"></a>使用 Declare 的 API 调用  

 调用 Windows Api 的最常见方法是使用 `Declare` 语句。  
  
### <a name="to-declare-a-dll-procedure"></a>声明 DLL 过程  
  
1. 确定要调用的函数的名称，以及该函数的参数、参数类型和返回值，以及包含该函数的 DLL 的名称和位置。  
  
    > [!NOTE]
    > 有关 Windows Api 的完整信息，请参阅 Platform sdk Windows API 中的 Win32 SDK 文档。 有关 Windows Api 使用的常量的详细信息，请检查在平台 SDK 中包含的头文件（如 Windows）。  
  
2. 在 "**文件**" 菜单上单击 "**新建**"，然后单击 "**项目**"，以打开新的 Windows 应用程序项目。 将显示“新建项目”对话框。  
  
3. 从 Visual Basic 项目模板列表中选择 " **Windows 应用程序** "。 将显示新项目。  
  
4. 将以下 `Declare` 函数添加到要在其中使用 DLL 的类或模块：  
  
     [!code-vb[VbVbalrInterop#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#9)]  
  
### <a name="parts-of-the-declare-statement"></a>Declare 语句的组成部分  

 `Declare`语句包括以下元素。  
  
#### <a name="auto-modifier"></a>Auto 修饰符  

 `Auto`修饰符指示运行时根据公共语言运行时规则 (或别名（如果指定) ），根据方法名称转换字符串。  
  
#### <a name="lib-and-alias-keywords"></a>Lib 和 Alias 关键字  

 关键字后面的名称 `Function` 是程序用来访问导入的函数的名称。 它可以与要调用的函数的实际名称相同，也可以使用任何有效的过程名称，然后使用 `Alias` 关键字来指定要调用的函数的真实名称。  
  
 指定 `Lib` 关键字，后跟包含要调用的函数的 DLL 的名称和位置。 不需要指定位于 Windows 系统目录中的文件的路径。  
  
 如果要 `Alias` 调用的函数的名称不是有效的 Visual Basic 过程名称，或者与应用程序中其他项的名称冲突，请使用关键字。 `Alias` 指示正在调用的函数的真实名称。  
  
#### <a name="argument-and-data-type-declarations"></a>参数和数据类型声明  

 声明参数及其数据类型。 此部分可能具有挑战性，因为 Windows 使用的数据类型与 Visual Studio 数据类型不对应。 Visual Basic 通过将参数转换为兼容的数据类型（称为 *封送*处理的进程）来执行大量的工作。 您可以通过使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 命名空间中定义的属性来显式控制如何对参数进行封送处理 <xref:System.Runtime.InteropServices> 。  
  
> [!NOTE]
> 以前版本的 Visual Basic 允许你声明参数 `As Any` ，这意味着可以使用任何数据类型的数据。 Visual Basic 要求对所有语句使用特定数据类型 `Declare` 。  
  
#### <a name="windows-api-constants"></a>Windows API 常量  

 一些参数是常量的组合。 例如， `MessageBox` 本演练中所示的 API 接受一个名为的整数参数 `Typ` ，用于控制消息框的显示方式。 可以通过检查 Winuser.h 文件中的语句来确定这些常量的数值 `#define` 。 数值通常以十六进制显示，因此，您可能希望使用计算器来添加它们并将其转换为 decimal。 例如，如果想要将感叹号样式的常量 `MB_ICONEXCLAMATION` 0x00000030 和 Yes/No 样式0x00000004 组合在一起 `MB_YESNO` ，则可以添加数字，并获得0x00000034 或 52 decimal 的结果。 虽然您可以直接使用 decimal 结果，但最好将这些值声明为应用程序中的常量，并使用运算符组合这些值 `Or` 。  
  
##### <a name="to-declare-constants-for-windows-api-calls"></a>声明用于 Windows API 调用的常量  
  
1. 请查阅您正在调用的 Windows 函数的文档。 确定其使用的常量的名称以及包含这些常量的数值的 .h 文件的名称。  
  
2. 使用文本编辑器（如记事本）查看标题 () 文件的内容，并查找与正在使用的常量相关联的值。 例如， `MessageBox` API 使用常量 `MB_ICONQUESTION` 在消息框中显示一个问号。 的定义位于 `MB_ICONQUESTION` winuser.h 中，如下所示：  
  
     `#define MB_ICONQUESTION             0x00000020L`  
  
3. 将等效 `Const` 语句添加到你的类或模块，以使这些常量可用于你的应用程序。 例如：  
  
     [!code-vb[VbVbalrInterop#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#11)]  
  
###### <a name="to-call-the-dll-procedure"></a>调用 DLL 过程  
  
1. 将名为的按钮添加 `Button1` 到项目的启动窗体，然后双击它以查看其代码。 显示按钮的事件处理程序。  
  
2. 向 `Click` 添加的按钮的事件处理程序中添加代码，以调用过程并提供相应的参数：  
  
     [!code-vb[VbVbalrInterop#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#12)]  
  
3. 按 F5 运行项目。 消息框显示 **"是" 和 "** **无** " 响应按钮。 单击任一项。  
  
#### <a name="data-marshaling"></a>数据封送  

 Visual Basic 会自动转换 Windows API 调用的参数和返回值的数据类型，但可以使用 `MarshalAs` 属性显式指定 API 预期的非托管数据类型。 有关互操作封送处理的详细信息，请参阅 [互操作封送处理](../../../framework/interop/interop-marshaling.md)。  
  
##### <a name="to-use-declare-and-marshalas-in-an-api-call"></a>在 API 调用中使用 Declare 和 MarshalAs  
  
1. 确定要调用的函数的名称，以及该函数的参数、数据类型和返回值。  
  
2. 若要简化对属性的访问 `MarshalAs` ，请向 `Imports` 类或模块的代码顶部添加一个语句，如以下示例中所示：  
  
     [!code-vb[VbVbalrInterop#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#13)]  
  
3. 将导入函数的函数原型添加到正在使用的类或模块，并将该特性应用于 `MarshalAs` 参数或返回值。 在下面的示例中，需要将类型 `void*` 封送为的 API 调用如下 `AsAny` ：  
  
     [!code-vb[VbVbalrInterop#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#14)]  
  
## <a name="api-calls-using-dllimport"></a>使用 DllImport 的 API 调用  

 `DllImport`特性提供了另一种方法来调用不带类型库的 dll 中的函数。 `DllImport` 大致等效于使用语句， `Declare` 但可以更好地控制调用函数的方式。  
  
 `DllImport`只要调用引用共享 (有时称为*静态*) 方法，就可以将用于大多数 Windows API 调用。 不能使用需要类的实例的方法。 与 `Declare` 语句不同， `DllImport` 调用不能使用 `MarshalAs` 属性。  
  
### <a name="to-call-a-windows-api-using-the-dllimport-attribute"></a>使用 DllImport 特性调用 Windows API  
  
1. 在 "**文件**" 菜单上单击 "**新建**"，然后单击 "**项目**"，以打开新的 Windows 应用程序项目。 将显示“新建项目”对话框。  
  
2. 从 Visual Basic 项目模板列表中选择 " **Windows 应用程序** "。 将显示新项目。  
  
3. 将名为的按钮添加 `Button2` 到启动窗体。  
  
4. 双击 `Button2` 打开窗体的代码视图。  
  
5. 若要简化对的访问 `DllImport` ，请将 `Imports` 语句添加到 startup 窗体类的代码顶部：  
  
     [!code-vb[VbVbalrInterop#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#13)]  
  
6. 在该窗体的语句之前声明一个空函数 `End Class` ，并将该函数命名为 `MoveFile` 。  
  
7. 将 `Public` 和修饰符应用于 `Shared` 函数声明，并 `MoveFile` 基于 Windows API 函数使用的参数设置参数：  
  
     [!code-vb[VbVbalrInterop#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#16)]  
  
     函数可以具有任何有效的过程名称; `DllImport` 特性指定 DLL 中的名称。 它还处理参数和返回值的互操作性封送处理，以便可以选择类似于 API 使用的数据类型的 Visual Studio 数据类型。  
  
8. 将特性应用于 `DllImport` 空函数。 第一个参数是包含所调用函数的 DLL 的名称和位置。 不需要指定位于 Windows 系统目录中的文件的路径。 第二个参数是命名参数，用于指定 Windows API 中函数的名称。 在此示例中， `DllImport` 特性强制将调用 `MoveFile` 转发到 `MoveFileW` KERNEL32.DLL 中的。 `MoveFileW`方法从路径 `src` 向路径复制文件 `dst` 。  
  
     [!code-vb[VbVbalrInterop#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#17)]  
  
9. 向 `Button2_Click` 事件处理程序添加代码以调用函数：  
  
     [!code-vb[VbVbalrInterop#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrInterop/VB/Class1.vb#18)]  
  
10. 创建一个名为 Test.txt 的文件，并将其放在硬盘驱动器上的 C:\Tmp 目录中。 如有必要，请创建 Tmp 目录。  
  
11. 按 F5 键启动该应用程序。 主窗体出现。  
  
12. 单击 " **Button2**"。 如果可以移动该文件，则会显示消息 "文件已成功移动"。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.DllImportAttribute>
- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Declare Statement](../../language-reference/statements/declare-statement.md)
- [Auto](../../language-reference/modifiers/auto.md)
- [Alias](../../language-reference/statements/alias-clause.md)
- [COM 互操作](index.md)
- [在托管代码中创建原型](../../../framework/interop/creating-prototypes-in-managed-code.md)
- [将委托作为回调方法进行封送](../../../framework/interop/marshaling-a-delegate-as-a-callback-method.md)
