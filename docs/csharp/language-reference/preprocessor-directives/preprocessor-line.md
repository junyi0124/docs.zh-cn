---
description: '#line - C# 参考'
title: '#line - C# 参考'
ms.date: 07/20/2015
f1_keywords:
- '#line'
helpviewer_keywords:
- '#line directive [C#]'
ms.assetid: 6439e525-5dd5-4acb-b8ea-efabb32ff95b
ms.openlocfilehash: e2a5ecb6c29184123b8a88ae1b12caf24ec7296a
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89137989"
---
# <a name="line-c-reference"></a>#line（C# 参考）

借助 `#line`，可修改编译器的行号及（可选）用于错误和警告的文件名输出。

以下示例演示如何报告与行号相关联的两个警告。 `#line 200` 指令将下一行的行号强制设为 200（尽管默认值为 #6）；在执行下一个 `#line` 指令前，文件名都会报告为“特殊”。 `#line default` 指令将行号恢复至默认行号，这会对上一指令重新编号的行进行计数。

```csharp
class MainClass
{
    static void Main()
    {
#line 200 "Special"
        int i;
        int j;
#line default
        char c;
        float f;
#line hidden // numbering not affected
        string s;
        double d;
    }
}
```

编译产生以下输出：

```console
Special(200,13): warning CS0168: The variable 'i' is declared but never used
Special(201,13): warning CS0168: The variable 'j' is declared but never used
MainClass.cs(9,14): warning CS0168: The variable 'c' is declared but never used
MainClass.cs(10,15): warning CS0168: The variable 'f' is declared but never used
MainClass.cs(12,16): warning CS0168: The variable 's' is declared but never used
MainClass.cs(13,16): warning CS0168: The variable 'd' is declared but never used
```

## <a name="remarks"></a>备注

可在生成过程的自动、中间步骤中使用 `#line` 指令。 例如，如果已从原始源代码文件中删除行，但仍希望编译器基于文件中的原始行号生成输出，可在删除行后，使用 `#line` 来模拟原始行号。

`#line hidden` 指令能对调试程序隐藏连续行，当开发者逐行执行代码时，介于 `#line hidden` 和下一 `#line` 指令（假设它不是其他 `#line hidden` 指令）间的任何行都将被跳过。 还可通过此选项允许 ASP.NET 区分用户定义和计算机生成的代码。 虽然此功能主要用于 ASP.NET，但可能更多的源生成器会利用此功能。

`#line hidden` 指令不影响错误报告中的文件名或行号。 也就是说，如果在隐藏块中遇到错误，编译器将报告错误的当前文件名和行号。

`#line filename` 指令可指定要在编译器输出中显示的文件名。 默认情况下，将使用源代码文件的实际名称。 该文件名必须以双引号 ("") 引起来，且必须位于行号之后。

源代码文件中可包含任意数目的 `#line` 指令。

## <a name="example-1"></a>示例 1

下列示例演示调试程序如何忽略代码中的隐藏行。 运行示例时，它将显示三行文本。 但是，如果按照示例所示设置断点、并按 F10 逐行执行代码，可观察到调试程序忽略隐藏行。 另请注意，即使在隐藏行设置断点，调试程序仍将忽略它。

```csharp
// preprocessor_linehidden.cs
using System;
class MainClass
{
    static void Main()
    {
        Console.WriteLine("Normal line #1."); // Set break point here.
#line hidden
        Console.WriteLine("Hidden line.");
#line default
        Console.WriteLine("Normal line #2.");
    }
}
```

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 预处理器指令](./index.md)
