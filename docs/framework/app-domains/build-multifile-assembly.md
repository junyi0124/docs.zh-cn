---
title: 如何：生成多文件程序集
description: 查看示例代码理解过程中的每个步骤，了解如何在 .NET 中生成（创建）多文件程序集。
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], multifile
- entry point for assembly
- compiling assemblies
- Al.exe
- command-line compilers
- assembly manifest, multifile assemblies
- assemblies [.NET Framework], compiling
- Assembly Linker
- code modules
- multifile assemblies
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 261c5583-8a76-412d-bda7-9b8ee3b131e5
ms.openlocfilehash: a4c298284950ba2989bb73e6d3383b3c4024e6e7
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104946"
---
# <a name="how-to-build-a-multifile-assembly"></a>如何：生成多文件程序集

本文章介绍如何创建多文件程序集，并提供用于说明过程中每个步骤的代码。

> [!NOTE]
> 适用于 C# 和 Visual Basic 的 Visual Studio IDE 只能用于创建单文件程序集。 如果要创建多文件程序集，则必须使用命令行编译器或带有 Visual C++ 的 Visual Studio。 多文件程序集仅由 .NET Framework 支持。

## <a name="create-a-multifile-assembly"></a>创建多文件程序集

1. 将包含程序集中其他模块引用的命名空间的所有文件编译成代码模块。 代码模块的默认扩展名为 .netmodule。

   例如，假定 `Stringer` 文件具有一个名为 `myStringer` 的命名空间，其中包括一个名为 `Stringer` 的类。 `Stringer` 类包含名为 `StringerMethod` 的方法，此方法将单独一行写入控制台。

   ```cpp
   // Assembly building example in the .NET Framework.
   using namespace System;

   namespace myStringer
   {
       public ref class Stringer
       {
       public:
           void StringerMethod()
           {
               System::Console::WriteLine("This is a line from StringerMethod.");
           }
       };
   }
   ```

   ```csharp
   // Assembly building example in the .NET Framework.
   using System;

   namespace myStringer
   {
       public class Stringer
       {
           public void StringerMethod()
           {
               System.Console.WriteLine("This is a line from StringerMethod.");
           }
       }
   }
   ```

   ```vb
   ' Assembly building example in the .NET Framework.
   Namespace myStringer
       Public Class Stringer
           Public Sub StringerMethod()
               System.Console.WriteLine("This is a line from StringerMethod.")
           End Sub
       End Class
   End Namespace
   ```

2. 使用下面的命令编译此代码：

   ```cpp
   cl /clr:pure /LN Stringer.cpp
   ```

   ```csharp
   csc /t:module Stringer.cs
   ```

   ```vb
   vbc /t:module Stringer.vb
   ```

   使用 **/t:** 编译器选项指定 *module* 参数，表明文件应作为模块（而不是作为程序集）编译。 编译器生成一个名为 Stringer.netmodule 的模块，可将其添加到程序集中。

3. 编译所有其他模块，使用必要的编译器选项来表明代码中引用的其他模块。 此步骤使用 /addmodule 编译器选项。

   在下面的示例中，名为 Client 的代码模块具有一个入口点 `Main` 方法，此方法引用创建于步骤 1 的 Stringer.dll 模块中的方法 。

   ```cpp
   #using "Stringer.netmodule"

   using namespace System;
   using namespace myStringer; //The namespace created in Stringer.netmodule.

   ref class MainClientApp
   {
       // Static method Main is the entry point method.
   public:
       static void Main()
       {
           Stringer^ myStringInstance = gcnew Stringer();
           Console::WriteLine("Client code executes");
           myStringInstance->StringerMethod();
       }
   };

   int main()
   {
       MainClientApp::Main();
   }
   ```

   ```csharp
   using System;
   using myStringer;

   class MainClientApp
   {
       // Static method Main is the entry point method.
       public static void Main()
       {
           Stringer myStringInstance = new Stringer();
           Console.WriteLine("Client code executes");
           myStringInstance.StringerMethod();
       }
   }
   ```

   ```vb
   Imports myStringer

   Class MainClientApp
       ' Static method Main is the entry point method.
       Public Shared Sub Main()
           Dim myStringInstance As New Stringer()
           Console.WriteLine("Client code executes")
           myStringInstance.StringerMethod()
       End Sub
   End Class
   ```

4. 使用下面的命令编译此代码：

   ```cpp
   cl /clr:pure /FUStringer.netmodule /LN Client.cpp
   ```

   ```csharp
   csc /addmodule:Stringer.netmodule /t:module Client.cs
   ```

   ```vb
   vbc /addmodule:Stringer.netmodule /t:module Client.vb
   ```

   指定 /t:module 选项，因为此模块将在以后的步骤中添加到程序集。 Client 中的代码会引用 Stringer.netmodule 中的代码创建的命名空间，因此请指定 /addmodule 选项 。 编译器生成一个名为 Client.netmodule 的模块，其中包含对另一个模块 Stringer.netmodule 的引用 。

   > [!NOTE]
   > C# 和 Visual Basic 编译器支持使用以下两种不同语法直接创建多文件程序集。
   >
   > 两次编译创建出一个双文件程序集：
   >
   >   ```cpp
   >   cl /clr:pure /LN Stringer.cpp
   >   cl /clr:pure Client.cpp /link /ASSEMBLYMODULE:Stringer.netmodule
   >   ```
   >
   >   ```csharp
   >   csc /t:module Stringer.cs
   >   csc Client.cs /addmodule:Stringer.netmodule
   >   ```
   >
   >   ```vb
   >   vbc /t:module Stringer.vb
   >   vbc Client.vb /addmodule:Stringer.netmodule
   >   ```
   >
   > 一次编译创建出一个双文件程序集：
   >
   >   ```cpp
   >   cl /clr:pure /LN Stringer.cpp
   >   cl /clr:pure Client.cpp /link /ASSEMBLYMODULE:Stringer.netmodule
   >   ```
   >
   >   ```csharp
   >   csc /out:Client.exe Client.cs /out:Stringer.netmodule Stringer.cs
   >   ```
   >
   >   ```vb
   >   vbc /out:Client.exe Client.vb /out:Stringer.netmodule Stringer.vb
   >   ```

5. 使用[程序集链接器 (Al.exe)](../tools/al-exe-assembly-linker.md) 来创建包含程序集清单的输出文件。 此文件包含作为程序集组成部分的所有模块或资源的参考信息。

    在命令提示符处，键入下列命令：

    al \<*module name*> \<*module name*> … /main:\<*method name*> /out:\<*file name*> /target:\<*assembly file type*>  

    在此命令中，“module name”参数指定程序集要包含的各模块的名称。 /main: 选项指定作为程序集入口点的方法名称。 /out: 选项指定输出文件的名称，它包含程序集元数据。 /target: 选项指定程序集是控制台应用程序可执行文件 (.exe)、Windows 可执行文件 (.win) 或库文件 (.lib)  。

    在下面的示例中，Al.exe 创建一个程序集，该程序集是一个名为 myAssembly.exe 的控制台应用程序可执行文件 。 该应用程序由名为 Client.netmodule 和 Stringer.netmodule 的两个模块以及名为 myAssembly.exe（其中只包含程序集元数据）的可执行文件组成  。 程序集的入口点是位于 Client.dll 的 `MainClientApp` 类中的 `Main` 方法。

    ```cmd
    al Client.netmodule Stringer.netmodule /main:MainClientApp.Main /out:myAssembly.exe /target:exe
    ```

    可以使用 [MSIL 反汇编程序 (Ildasm.exe)](../tools/ildasm-exe-il-disassembler.md) 来检查程序集的内容，或者确定文件是程序集还是模块。

## <a name="see-also"></a>请参阅

- [创建程序集](../../standard/assembly/create.md)
- [如何：查看程序集内容](../../standard/assembly/view-contents.md)
- [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)
- [多文件程序集](multifile-assemblies.md)
