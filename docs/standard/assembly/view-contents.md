---
title: 如何：查看程序集内容
description: 可以使用 IL 反汇编程序来查看程序集的属性，以及对其他模块和程序集的引用。
ms.date: 08/20/2019
helpviewer_keywords:
- assembly manifest, viewing information
- Ildasm.exe
- MSIL Disassembler
- assemblies [.NET], viewing contents
- viewing assembly information
- MSIL
- viewing MSIL information
ms.assetid: fb7baaab-4c0d-47ad-8fd3-4591cf834709
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: be2311c601effbebd519e33b7a5e13d49f44bd05
ms.sourcegitcommit: 279fb6e8d515df51676528a7424a1df2f0917116
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92687494"
---
# <a name="how-to-view-assembly-contents"></a>如何：查看程序集内容

可使用 [Ildasm.exe（IL 反汇编程序）](../../framework/tools/ildasm-exe-il-disassembler.md)查看文件中的 Microsoft 中间语言 (MSIL) 信息。 如果要检查的文件是程序集，此信息可包括程序集的属性以及对其他模块和程序集的引用。 此信息有助于确定文件是程序集还是程序集的一部分，以及文件是否具有对其他模块或程序集的引用。

若要使用 Ildasm.exe 来显示程序集的内容，请在命令提示符下键入 ildasm \<assembly name>。 例如，以下命令反汇编 Hello.exe 程序集。

```cmd
ildasm Hello.exe
```

若要查看程序集清单信息，请在“MSIL 反汇编程序”窗口中双击“清单”图标。

## <a name="example"></a>示例

下例以基本的“Hello World”程序开始。 编译该程序后，使用 Ildasm.exe 反汇编 Hello.exe 程序集，并查看程序集清单 。

```cpp
using namespace System;

class MainApp
{
public:
    static void Main()
    {
        Console::WriteLine("Hello World using C++/CLI!");
    }
};

int main()
{
    MainApp::Main();
}
```

```csharp
using System;

class MainApp
{
    public static void Main()
    {
        Console.WriteLine("Hello World using C#!");
    }
}
```

```vb
Class MainApp
    Public Shared Sub Main()
        Console.WriteLine("Hello World using Visual Basic!")
    End Sub
End Class
```

在 Hello.exe 程序集上运行 ildasm.exe 命令，然后在“MSIL 反汇编程序”窗口中双击“清单”图标生成以下输出 ：

```output
// Metadata version: v4.0.30319
.assembly extern mscorlib
{
  .publickeytoken = (B7 7A 5C 56 19 34 E0 89 )                         // .z\V.4..
  .ver 4:0:0:0
}
.assembly Hello
{
  .custom instance void [mscorlib]System.Runtime.CompilerServices.CompilationRelaxationsAttribute::.ctor(int32) = ( 01 00 08 00 00 00 00 00 )
  .custom instance void [mscorlib]System.Runtime.CompilerServices.RuntimeCompatibilityAttribute::.ctor() = ( 01 00 01 00 54 02 16 57 72 61 70 4E 6F 6E 45 78   // ....T..WrapNonEx
                                                                                                             63 65 70 74 69 6F 6E 54 68 72 6F 77 73 01 )       // ceptionThrows.
  .hash algorithm 0x00008004
  .ver 0:0:0:0
}
.module Hello.exe
// MVID: {7C2770DB-1594-438D-BAE5-98764C39CCCA}
.imagebase 0x00400000
.file alignment 0x00000200
.stackreserve 0x00100000
.subsystem 0x0003       // WINDOWS_CUI
.corflags 0x00000001    //  ILONLY
// Image base: 0x00600000
```

下表描述了本例所使用 Hello.exe 程序集的程序集清单中的各项指令：

|指令|描述|
|---------------|-----------------|
|**.assembly extern \<assembly name>**|指定包含当前模块所引用项目的另一程序集（在此示例中为 `mscorlib`）。|
|**.publickeytoken \<token>**|指定引用程序集的实际密钥的标记。|
|**.ver \<version number>**|指定引用程序集的版本号。|
|**.assembly \<assembly name>**|指定程序集名称。|
|**.hash algorithm \<int32 value>**|指定使用的哈希算法。|
|**.ver \<version number>**|指定程序集的版本号。|
|**.module \<file name>**|指定组成程序集的模块名称。 在此示例中，程序集只包含一个文件。|
|**.subsystem \<value>**|指定程序要求的应用程序环境。 在此示例中，值 3 表示该可执行文件从控制台运行。|
|.corflags|当前是元数据中的一个保留字段。|

根据程序集的内容，程序集清单可包含许多不同的指令。 有关程序集清单中指令的详尽列表，请参阅 Ecma 文档，特别是“第 II 部分：Metadata Definition and Semantics”（第 2 部分：元数据定义和语义）和“Partition III:CIL 指令集”：

- [ECMA C# 和公共语言基础结构标准](../components.md#applicable-standards)
- [标准 ECMA-335 - 公共语言基础结构 (CLI)](http://www.ecma-international.org/publications/standards/Ecma-335.htm)

## <a name="see-also"></a>请参阅

- [应用程序域和程序集](../../framework/app-domains/application-domains.md#application-domains-and-assemblies)
- [应用程序域和程序集帮助主题](../../framework/app-domains/application-domains-and-assemblies-how-to-topics.md)
- [Ildasm.exe（IL 反汇编程序）](../../framework/tools/ildasm-exe-il-disassembler.md)
