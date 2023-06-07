---
title: 演练：在 Visual Studio 中嵌入托管程序集中的类型
description: 本演练演示如何使用 Visual Studio 在 .NET 中嵌入托管程序集中的类型。 嵌入类型可支持版本独立性。
ms.date: 08/19/2019
ms.assetid: 55ed13c9-c5bb-4bc2-bcd8-0587eb568864
dev_langs:
- csharp
- vb
ms.openlocfilehash: 636e5f8095b64cd0f445555c96d00945ccf7eaf8
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83378987"
---
# <a name="walkthrough-embed-types-from-managed-assemblies-in-visual-studio"></a>演练：在 Visual Studio 中嵌入托管程序集中的类型

如果嵌入强命名托管程序集中的类型信息，则可以对应用程序中的类型进行松耦合，以实现版本独立性。 也就是说，可以将程序编写为使用托管库任何版本中的类型，而不必对每个版本重新编译程序。

类型嵌入经常与 COM 互操作一起使用，例如使用 Microsoft Office 中的自动化对象的应用程序。 通过嵌入类型信息，程序的同一个版本可以处理不同计算机上的不同 Microsoft Office 版本。 但也可以在完全托管的解决方案中使用类型嵌入。

指定可嵌入的公共接口后，创建实现这些接口的运行时类。 客户端程序可以通过引用包含这些公共接口的程序集，并将该引用的 `Embed Interop Types` 属性设置为 `True`，即可在设计时嵌入这些接口的类型信息。 然后，客户端程序可以加载类型化为这些接口的运行时对象的实例。 这相当于使用命令行编译器和通过 [-link 编译器选项](../../csharp/language-reference/compiler-options/link-compiler-option.md)引用该程序集。

如果创建新版本的强命名运行时程序集，则不必重新编译客户端程序。 客户端程序继续使用任何可用的运行时程序集版本，使用公共接口的嵌入类型信息。

在此演练中，将：

1. 创建一个强命名程序集，使其中的公共接口包含可嵌入的类型信息。
1. 创建一个实现公共接口的强命名运行时程序集。
1. 创建一个客户端程序，该程序嵌入公共接口中的类型信息，并创建运行时程序集中的类的实例。
1. 修改并重新生成运行时程序集。
1. 运行客户端程序，查看是否使用运行时程序集的新版本，而不必进行重新编译。

[!INCLUDE[note_settings_general](../../../includes/note-settings-general-md.md)]

## <a name="conditions-and-limitations"></a>条件和限制

可以在以下条件下嵌入程序集中的类型信息：

- 程序集至少公开一个公共接口。
- 嵌入的接口使用具有唯一 GUID 的 `ComImport` 属性和 `Guid` 属性进行批注。
- 程序集使用 `ImportedFromTypeLib` 特性或 `PrimaryInteropAssembly` 特性和程序集级别的 `Guid` 特性进行批注。 默认情况下，Visual C# 和 Visual Basic 项目模板包含程序集级别的 `Guid` 属性。

由于类型嵌入的主要功能是支持 COM 互操作程序集，因此在完全托管解决方案中嵌入类型信息时，将应用以下限制：

- 仅嵌入特定于 COM 互操作的属性。 忽略其他属性。
- 如果一个类型使用泛型参数，且泛型参数的类型是嵌入类型，则不能跨程序集边界使用该类型。 跨程序集边界的示例包括从另一个程序集调用方法或从另一个程序集中定义的类型派生类型。
- 不嵌入常量。
- <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> 类不支持将嵌入类型作为密钥。 可以实现自己的字典类型以支持将嵌入类型作为密钥。

## <a name="create-an-interface"></a>创建接口

第一步是创建类型等效性接口程序集。

1. 在 Visual Studio 中，选择“文件” > “新建” > “项目”  。

1. 在“创建新项目”对话框中，在“搜索模板”框中键入“类库”。 从列表中选择 C# 或 Visual Basic“类库(.NET Framework)”模板，然后选择“下一步” 。

1. 在“配置新项目”对话框的“项目名称”下，键入“TypeEquivalenceInterface”，然后选择“创建” 。 新项目创建完成。

1. 在“解决方案资源管理器”中，右键单击“Class1.cs”或“Class1.vb”文件，选择“重命名”，并将该文件从“Class1”重命名为“ISampleInterface”  。 出现提示时选择“是”，同时将类重命名为 `ISampleInterface`。 此类表示类的公共接口。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceInterface 项目，然后选择“属性”  。

1. 在“属性”屏幕的左窗格中选择“生成”，并将“输出路径”设置为计算机上的某个位置（例如 C:\TypeEquivalenceSample）  。 本演练中使用相同的位置。

1. 在“属性”屏幕的左窗格中选择“签名”，然后选择“对程序集签名”复选框  。 在“选择强名称密钥文件”下拉列表中，选择“新建” 。

1. 在“创建强名称密钥”对话框的“密钥文件名称”下，键入“key.snk” 。 取消选中“使用密码保护密钥文件”复选框，然后选择“确定” 。

1. 在代码编辑器中打开 ISampleInterface 类文件，将其内容替换为以下代码，以便创建 `ISampleInterface` 接口：

   ```csharp
   using System;
   using System.Runtime.InteropServices;

   namespace TypeEquivalenceInterface
   {
       [ComImport]
       [Guid("8DA56996-A151-4136-B474-32784559F6DF")]
       public interface ISampleInterface
       {
           void GetUserInput();
           string UserInput { get; }
       }
   }
   ```

   ```vb
   Imports System.Runtime.InteropServices

   <ComImport()>
   <Guid("8DA56996-A151-4136-B474-32784559F6DF")>
   Public Interface ISampleInterface
       Sub GetUserInput()
       ReadOnly Property UserInput As String
   End Interface
   ```

1. 在“工具”菜单上，选择“创建 GUID”，然后在“创建 GUID”对话框中，选择“注册表格式”   。 选择“复制”，然后选择“退出” 。

1. 在代码的 `Guid` 属性中，将示例 GUID 替换为复制的 GUID，并删除大括号 ({ })。

1. 在“解决方案资源管理器”中，展开“属性”文件夹，并选择“AssemblyInfo.cs”或“AssemblyInfo.vb”文件  。 在代码编辑器中，将以下属性添加到文件：

   ```csharp
   [assembly: ImportedFromTypeLib("")]
   ```

   ```vb
   <Assembly: ImportedFromTypeLib("")>
   ```

1. 选择“文件” > “保存全部”，或按 Ctrl+Shift+S 来保存文件和项目    。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceInterface 项目，然后选择“生成”  。 编译类库 DLL 文件，并保存到指定的生成输出路径中（如 C:\TypeEquivalenceSample）。

## <a name="create-a-runtime-class"></a>创建运行时类

接下来，创建类型等效性运行时类。

1. 在 Visual Studio 中，选择“文件” > “新建” > “项目”  。

1. 在“创建新项目”对话框中，在“搜索模板”框中键入“类库”。 从列表中选择 C# 或 Visual Basic“类库(.NET Framework)”模板，然后选择“下一步” 。

1. 在“配置新项目”对话框的“项目名称”下，键入“TypeEquivalenceRuntime”，然后选择“创建” 。 新项目创建完成。

1. 在“解决方案资源管理器”中，右键单击“Class1.cs”或“Class1.vb”文件，选择“重命名”，并将该文件从“Class1”重命名为“SampleClass”  。 出现提示时选择“是”，同时将类重命名为 `SampleClass`。 此类实现 `ISampleInterface` 接口。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceInterface 项目，并选择“属性”  。

1. 在“属性”屏幕的左窗格中选择“生成”，然后将“输出路径”设置为用于 TypeEquivalenceInterface 项目的同一位置（例如 C:\TypeEquivalenceSample）  。

1. 在“属性”屏幕的左窗格中选择“签名”，然后选择“对程序集签名”复选框  。 在“选择强名称密钥文件”下拉列表中，选择“新建” 。

1. 在“创建强名称密钥”对话框的“密钥文件名称”下，键入“key.snk” 。 取消选中“使用密码保护密钥文件”复选框，然后选择“确定” 。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceRuntime 项目，然后选择“添加” > “引用”   。

1. 在“引用管理器”对话框中，选择“浏览”并浏览到输出路径文件夹 。 选择 TypeEquivalenceInterface.dll 文件，选择“添加”，然后选择“确定” 。

1. 在“解决方案资源管理器”中，展开“引用”文件夹，并选择“TypeEquivalenceInterface”引用  。 在“属性”窗格中，将“特定版本”设置为“False”（如果尚未设置）  。

1. 在代码编辑器中打开 SampleClass 类文件，将其内容替换为以下代码，以便创建 `SampleClass` 类：

   ```csharp
   using System;
   using TypeEquivalenceInterface;

   namespace TypeEquivalenceRuntime
   {
       public class SampleClass : ISampleInterface
       {
           private string p_UserInput;
           public string UserInput { get { return p_UserInput; } }

           public void GetUserInput()
           {
               Console.WriteLine("Please enter a value:");
               p_UserInput = Console.ReadLine();
           }
       }
   }
   ```

   ```vb
   Imports TypeEquivalenceInterface

   Public Class SampleClass
       Implements ISampleInterface

       Private p_UserInput As String
       Public ReadOnly Property UserInput() As String Implements ISampleInterface.UserInput
           Get
               Return p_UserInput
           End Get
       End Property

       Public Sub GetUserInput() Implements ISampleInterface.GetUserInput
           Console.WriteLine("Please enter a value:")
           p_UserInput = Console.ReadLine()
       End Sub
   End Class
   ```

1. 选择“文件” > “保存全部”，或按 Ctrl+Shift+S 来保存文件和项目    。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceRuntime 项目，并选择“生成”  。 编译类库 DLL 文件，并保存到指定的生成输出路径中。

## <a name="create-a-client-project"></a>创建客户端项目

最后，创建引用接口程序集的类型等效性客户端程序。

1. 在 Visual Studio 中，选择“文件” > “新建” > “项目”  。

1. 在“创建新项目”对话框中，在“搜索模板”框中键入“控制台”。 从列表中选择 C# 或 Visual Basic“控制台应用(.NET Framework)”模板，然后选择“下一步” 。

1. 在“配置新项目”对话框的“项目名称”下，键入“TypeEquivalenceClient”，然后选择“创建” 。 新项目创建完成。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceClient 项目，并选择“属性”  。

1. 在“属性”屏幕的左窗格中选择“生成”，然后将“输出路径”设置为用于 TypeEquivalenceInterface 项目的同一位置（例如 C:\TypeEquivalenceSample）  。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceClient 项目，并选择“添加” > “引用”   。

1. 如果 TypeEquivalenceInterface.dll 文件已在“引用管理器”对话框中列出，则选择该文件 。 如果没有，请选择“浏览”，浏览到输出路径文件夹，选择 TypeEquivalenceInterface.dll 文件（而不是 TypeEquivalenceRuntime.dll 文件），并选择“添加” 。 选择“确定”。

1. 在“解决方案资源管理器”中，展开“引用”文件夹，并选择“TypeEquivalenceInterface”引用  。 在“属性”窗格中，将“嵌入式互操作类型”设置为“True”  。

1. 在代码编辑器中打开 Program.cs 或 Module1.vb 文件，将其内容替换为以下代码，以便创建客户端程序 ：

   ```csharp
   using System;
   using System.Reflection;
   using TypeEquivalenceInterface;

   namespace TypeEquivalenceClient
   {
       class Program
       {
           static void Main(string[] args)
           {
               Assembly sampleAssembly = Assembly.Load("TypeEquivalenceRuntime");
               ISampleInterface sampleClass =
                   (ISampleInterface)sampleAssembly.CreateInstance("TypeEquivalenceRuntime.SampleClass");
               sampleClass.GetUserInput();
               Console.WriteLine(sampleClass.UserInput);
               Console.WriteLine(sampleAssembly.GetName().Version.ToString());
               Console.ReadLine();
           }
       }
   }
   ```

   ```vb
   Imports System.Reflection
   Imports TypeEquivalenceInterface

   Module Module1

       Sub Main()
           Dim sampleAssembly = Assembly.Load("TypeEquivalenceRuntime")
           Dim sampleClass As ISampleInterface = CType( _
               sampleAssembly.CreateInstance("TypeEquivalenceRuntime.SampleClass"), ISampleInterface)
           sampleClass.GetUserInput()
           Console.WriteLine(sampleClass.UserInput)
           Console.WriteLine(sampleAssembly.GetName().Version)
           Console.ReadLine()
       End Sub

   End Module
   ```

1. 选择“文件” > “保存全部”，或按 Ctrl+Shift+S 来保存文件和项目    。

1. 按 Ctrl+F5 生成并运行程序 。 请注意，控制台输出返回程序集版本 1.0.0.0。

## <a name="modify-the-interface"></a>修改接口

现在，修改接口程序集，并更改其版本。

1. 在 Visual Studio 中，选择“文件” > “打开” > 项目/解决方案，并打开 TypeEquivalenceInterface 项目   。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceInterface 项目，并选择“属性”  。

1. 在“属性”屏幕的左窗格中选择“应用程序”，然后选择“程序集信息”  。

1. 在“程序集信息”对话框中，将“程序集版本”和“文件版本”值更改为 2.0.0.0，然后选择“确定”  。

1. 打开 SampleInterface.cs 或 SampleInterface.vb 文件，并将以下代码行添加到 `ISampleInterface` 接口 ：

   ```csharp
   DateTime GetDate();
   ```

   ```vb
   Function GetDate() As Date
   ```

1. 选择“文件” > “保存全部”，或按 Ctrl+Shift+S 来保存文件和项目    。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceInterface 项目，然后选择“生成”  。 编译类库 DLL 文件的新版本，并保存到生成输出路径中。

## <a name="modify-the-runtime-class"></a>修改运行时类

修改运行时类的同时更新其版本。

1. 在 Visual Studio 中，选择“文件” > “打开” > 项目/解决方案，并打开 TypeEquivalenceRuntime 项目   。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceRuntime 项目，并选择“属性”  。

1. 在“属性”屏幕的左窗格中选择“应用程序”，然后选择“程序集信息”  。

1. 在“程序集信息”对话框中，将“程序集版本”和“文件版本”值更改为 2.0.0.0，然后选择“确定”  。

1. 打开 SampleClass.cs 或 SampleClass.vb 文件，并将以下代码添加到 `SampleClass` 类 ：

   ```csharp
    public DateTime GetDate()
    {
        return DateTime.Now;
    }
   ```

   ```vb
   Public Function GetDate() As DateTime Implements ISampleInterface.GetDate
       Return Now
   End Function
   ```

1. 选择“文件” > “保存全部”，或按 Ctrl+Shift+S 来保存文件和项目    。

1. 在“解决方案资源管理器”中，右键单击 TypeEquivalenceRuntime 项目，并选择“生成”  。 编译类库 DLL 文件的新版本，并保存到生成输出路径中。

## <a name="run-the-updated-client-program"></a>运行更新的客户端程序

转到生成输出文件夹位置，并运行 TypeEquivalenceClient.exe。 请注意，控制台输出现在反映 `TypeEquivalenceRuntime` 程序集的新版本（即 2.0.0.0），而不会重新编译程序。

## <a name="see-also"></a>请参阅

- [-link（C# 编译器选项）](../../csharp/language-reference/compiler-options/link-compiler-option.md)
- [-link (Visual Basic)](../../visual-basic/reference/command-line-compiler/link.md)
- [C# 编程指南](../../csharp/programming-guide/index.md)
- [编程概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
- [.NET 中的程序集](index.md)
