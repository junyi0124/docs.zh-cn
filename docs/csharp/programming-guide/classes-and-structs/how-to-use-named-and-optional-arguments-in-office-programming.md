---
title: 如何在 Office 编程中使用命名实参和可选实参（C# 编程指南）
description: 了解如何使用命名参数和可选参数来帮助访问 COM 接口，如 Microsoft Office 自动化 API。
ms.date: 07/20/2015
helpviewer_keywords:
- named and optional arguments [C#], Office programming
- optional arguments [C#], Office programming
- named arguments [C#], Office programming
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: 65b8a222-bcd8-454c-845f-84adff5a356f
ms.openlocfilehash: de0545131385c13a79fd35df74e3a04da98ad8fb
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97512471"
---
# <a name="how-to-use-named-and-optional-arguments-in-office-programming-c-programming-guide"></a>如何在 Office 编程中使用命名实参和可选实参（C# 编程指南）

在 C# 4 中引入的命名参数和可选参数增强了 C# 编程中的便利性、灵活性和可读性。 另外，这些功能显著方便了对 COM 接口（如 Microsoft Office 自动化 API）的访问。

在下面的示例中，方法 [ConvertToTable](<xref:Microsoft.Office.Interop.Word.Range.ConvertToTable%2A>) 具有十六个参数，用于表示表的各种特性，例如列数和行数、格式设置、边框、字体以及颜色。 由于大多数时候都不需要为所有十六个参数指定特定值，因此所有这些参数都是可选的。 但是，如果没有命名实参和可选实参，则必须为每个形参提供值或占位符值。 有了命名实参和可选实参，则只需为项目所需的形参指定值。

必须在计算机上安装 Microsoft Office Word 才能完成这些过程。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-a-new-console-application"></a>创建新的控制台应用程序

1. 启动 Visual Studio。

2. 在 **“文件”** 菜单上，指向 **“新建”** ，然后单击 **“项目”** 。

3. 在“模板类别”窗格中，展开“Visual C#”，然后单击“Windows”。

4. 查看“模板”窗格的顶部，确保“.NET Framework 4”出现在“目标框架”框中。

5. 在“模板”窗格中，单击“控制台应用程序”。

6. 在“名称”字段中键入项目的名称。

7. 单击 **“确定”** 。

     新项目将出现在“解决方案资源管理器”中。

## <a name="to-add-a-reference"></a>添加引用

1. 在“解决方案资源管理器”中，右键单击你的项目名称，然后单击“添加引用”。 此时会显示“添加引用”对话框。

2. 在“.NET”页上的“组件名称”列表中，选择“Microsoft.Office.Interop.Word”  。

3. 单击 **“确定”** 。

## <a name="to-add-necessary-using-directives"></a>添加必要的 using 指令

1. 在“解决方案资源管理器”中，右键单击“Program.cs”文件，然后单击“查看代码”。

2. 将以下 `using` 指令添加到代码文件的顶部：

     [!code-csharp[csProgGuideNamedAndOptional#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#4)]

## <a name="to-display-text-in-a-word-document"></a>在 Word 文档中显示文本

1. 在“Program.cs”的 `Program` 类中，添加以下方法以创建 Word 应用程序和 Word 文档。 [Add](<xref:Microsoft.Office.Interop.Word.Documents.Add%2A>) 方法具有四个可选参数。 此示例使用这些参数的默认值。 因此，调用语句中不必有参数。

     [!code-csharp[csProgGuideNamedAndOptional#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#6)]

2. 将以下代码添加到方法的末尾，以定义要在文档何处显示文本，以及要显示什么文本：

     [!code-csharp[csProgGuideNamedAndOptional#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#7)]

## <a name="to-run-the-application"></a>要运行应用程序

1. 将以下语句添加到 Main：

     [!code-csharp[csProgGuideNamedAndOptional#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#8)]

2. 按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 运行项目。 此时会出现一个 Word 文档，其中包含指定的文本。

## <a name="to-change-the-text-to-a-table"></a>将文本更改为表
  
1. 使用 `ConvertToTable` 方法将文本放入表中。 该方法具有十六个可选参数。 IntelliSense 将可选参数放入括号中，如下图所示。

     ![ConvertToTable 方法的参数列表](./media/how-to-use-named-and-optional-arguments-in-office-programming/convert-table-parameters.png)

     通过使用命名实参和可选实参，可以只对要更改的形参指定值。 将以下代码添加到方法 `DisplayInWord` 的末尾以创建一个简单的表。 此参数指定 `range` 中文本字符串内的逗号分隔表的各个单元格。

     [!code-csharp[csProgGuideNamedAndOptional#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#9)]

     在 C# 早期版本中，对 `ConvertToTable` 的调用需要对每个形参使用引用实参，如以下代码所示：
  
     [!code-csharp[csProgGuideNamedAndOptional#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#14)]

2. 按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 运行项目。

## <a name="to-experiment-with-other-parameters"></a>试验其他参数

1. 若要更改表以使其具有一列三行，请将 `DisplayInWord` 中的最后一行替换为以下语句，然后按 <kbd>CTRL</kbd>+<kbd>F5</kbd>。  

     [!code-csharp[csProgGuideNamedAndOptional#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#10)]

2. 若要为表指定预定义的格式，请将 `DisplayInWord` 中的最后一行替换为以下语句，然后按 <kbd>CTRL</kbd>+<kbd>F5</kbd>。 格式可以为任何 [WdTableFormat](<xref:Microsoft.Office.Interop.Word.WdTableFormat>) 常量。

     [!code-csharp[csProgGuideNamedAndOptional#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#11)]

## <a name="example"></a>示例

下面的代码包括完整的示例：

 [!code-csharp[csProgGuideNamedAndOptional#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/wordprogram.cs#12)]

## <a name="see-also"></a>请参阅

- [命名参数和可选参数](./named-and-optional-arguments.md)
