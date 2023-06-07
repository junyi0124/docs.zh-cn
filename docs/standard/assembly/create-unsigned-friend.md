---
title: 如何：创建未签名的友元程序集
description: 本文演示如何将友元程序集和未签名的程序集一起使用。 其中包含有关 .NET 安全性的信息。
ms.date: 08/19/2019
ms.assetid: 78cbc4f0-b021-4141-a4ff-eb4edbd814ca
dev_langs:
- csharp
- vb
ms.openlocfilehash: 8d3e13669c36048759fedeb3df1bfb59fd476317
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83378974"
---
# <a name="how-to-create-unsigned-friend-assemblies"></a>如何：创建未签名的友元程序集

本示例演示如何将友元程序集和未签名的程序集一起使用。

## <a name="create-an-assembly-and-a-friend-assembly"></a>创建程序集和友元程序集

1. 打开命令提示。

2. 创建名为 friend_unsigned_A 的 C# 或 Visual Basic 文件，其中包含以下代码。 该代码使用 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性将 friend_unsigned_B 声明为友元程序集。

   ```csharp
   // friend_unsigned_A.cs
   // Compile with:
   // csc /target:library friend_unsigned_A.cs
   using System.Runtime.CompilerServices;
   using System;

   [assembly: InternalsVisibleTo("friend_unsigned_B")]

   // Type is internal by default.
   class Class1
   {
       public void Test()
       {
           Console.WriteLine("Class1.Test");
       }
   }

   // Public type with internal member.
   public class Class2
   {
       internal void Test()
       {
           Console.WriteLine("Class2.Test");
       }
   }
   ```

   ```vb
   ' friend_unsigned_A.vb
   ' Compile with:
   ' vbc -target:library friend_unsigned_A.vb
   Imports System.Runtime.CompilerServices

   <Assembly: InternalsVisibleTo("friend_unsigned_B")>

   ' Friend type.
   Friend Class Class1
       Public Sub Test()
           Console.WriteLine("Class1.Test")
       End Sub
   End Class

   ' Public type with Friend member.
   Public Class Class2
       Friend Sub Test()
           Console.WriteLine("Class2.Test")
       End Sub
   End Class
   ```

3. 使用以下命令编译 friend_unsigned_A 并为其签名：

   ```csharp
   csc /target:library friend_unsigned_A.cs
   ```

   ```vb
   vbc -target:library friend_unsigned_A.vb
   ```

4. 创建名为 friend_unsigned_B 的 C# 或 Visual Basic 文件，其中包含以下代码。 由于 friend_unsigned_A 将 friend_unsigned_B 指定为友元程序集，因此 friend_unsigned_B 中的代码可以访问 `internal` (C#) 或 friend_unsigned_A 中的 `Friend` (Visual Basic) 类型和成员   。

   ```csharp
   // friend_unsigned_B.cs
   // Compile with:
   // csc /r:friend_unsigned_A.dll /out:friend_unsigned_B.exe friend_unsigned_B.cs
   public class Program
   {
       static void Main()
       {
           // Access an internal type.
           Class1 inst1 = new Class1();
           inst1.Test();

           Class2 inst2 = new Class2();
           // Access an internal member of a public type.
           inst2.Test();

           System.Console.ReadLine();
       }
   }
   ```

   ```vb
   ' friend_unsigned_B.vb
   ' Compile with:
   ' vbc -r:friend_unsigned_A.dll friend_unsigned_B.vb
   Module Module1
       Sub Main()
           ' Access a Friend type.
           Dim inst1 As New Class1()
           inst1.Test()

           Dim inst2 As New Class2()
           ' Access a Friend member of a public type.
           inst2.Test()

           System.Console.ReadLine()
       End Sub
   End Module
   ```

5. 使用以下命令编译 friend_unsigned_B。

   ```csharp
   csc /r:friend_unsigned_A.dll /out:friend_unsigned_B.exe friend_unsigned_B.cs
   ```

   ```vb
   vbc -r:friend_unsigned_A.dll friend_unsigned_B.vb
   ```

   编译器生成的程序集的名称必须与传递给 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性的友元程序集的名称匹配。 必须使用 `-out` 编译器选项显式指定输出程序集（.exe 或 .dll）的名称 。 有关详细信息，请参阅 [-out（C# 编译器选项）](../../csharp/language-reference/compiler-options/out-compiler-option.md)或 [-out (Visual Basic)](../../visual-basic/reference/command-line-compiler/out.md)。

6. 运行 friend_unsigned_B.exe 文件。

   该程序输出两个字符串：“Class1.Test”和“Class2.Test” 。

## <a name="net-security"></a>.NET 安全性

<xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性和 <xref:System.Security.Permissions.StrongNameIdentityPermission> 类之间具有相似之处。 主要区别是，<xref:System.Security.Permissions.StrongNameIdentityPermission> 可以要求安全权限来运行一段特定代码，而 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性控制 `internal` 和 `Friend` (Visual Basic) 类型和成员的可见性。

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- [.NET 中的程序集](index.md)
- [友元程序集](friend.md)
- [如何：创建已签名的友元程序集](create-signed-friend.md)
- [C# 编程指南](../../csharp/programming-guide/index.md)
- [编程概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
