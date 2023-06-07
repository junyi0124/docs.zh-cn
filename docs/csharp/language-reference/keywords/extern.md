---
description: extern 修饰符 - C# 参考
title: extern 修饰符 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- extern_CSharpKeyword
- extern
helpviewer_keywords:
- DllImport attribute
- extern keyword [C#]
ms.assetid: 9c3f02c4-51b8-4d80-9cb2-f2b6e1ae15c7
ms.openlocfilehash: 25eb5e6642d8b608bedcb4e9adadde4d84c2bae9
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89138964"
---
# <a name="extern-c-reference"></a>extern（C# 参考）

`extern` 修饰符用于声明在外部实现的方法。 `extern` 修饰符的常见用法是在使用 Interop 服务调入非托管代码时与 `DllImport` 特性一起使用。 在这种情况下，还必须将方法声明为 `static`，如下面的示例所示：

```csharp
[DllImport("avifil32.dll")]
private static extern void AVIFileInit();
```

`extern` 关键字还可以定义外部程序集别名，使得可以从单个程序集中引用同一组件的不同版本。 有关详细信息，请参阅[外部别名](extern-alias.md)。

将 [abstract](abstract.md) 和 `extern` 修饰符一起使用来修改同一成员是错误的做法。 使用 `extern` 修饰符意味着方法是在 C# 代码的外部实现的，而使用 `abstract` 修饰符意味着类中未提供方法实现。

extern 关键字用于 C# 中时会比用于 C++ 中时受到更多的限制。 若要比较 C# 关键字与 C++ 关键字，请参见 C++ 语言参考中的“使用 extern 指定链接”。

## <a name="example-1"></a>示例 1

在此示例中，程序接收来自用户的字符串并将该字符串显示在消息框中。 程序使用从 User32.dll 库导入的 `MessageBox` 方法。

[!code-csharp[csrefKeywordsModifiers#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsModifiers/CS/csrefKeywordsModifiers.cs#8)]

## <a name="example-2"></a>示例 2

此示例阐释了调入 C 库（本机 DLL）的 C# 程序。

1. 创建以下 C 文件并将其命名为 `cmdll.c`：

    ```c
    // cmdll.c
    // Compile with: -LD
    int __declspec(dllexport) SampleMethod(int i)
    {
      return i*10;
    }
    ```

2. 从 Visual Studio 安装目录打开 Visual Studio x64（或 x32）本机工具命令提示符窗口，并通过在命令提示符处键入“cl -LD cmdll.c”来编译 `cmdll.c` 文件。

3. 在相同的目录中，创建以下 C# 文件并将其命名为 `cm.cs`：

    ```csharp
    // cm.cs
    using System;
    using System.Runtime.InteropServices;
    public class MainClass
    {
        [DllImport("Cmdll.dll")]
          public static extern int SampleMethod(int x);

        static void Main()
        {
            Console.WriteLine("SampleMethod() returns {0}.", SampleMethod(5));
        }
    }
    ```

4. 从 Visual Studio 安装目录打开一个 Visual Studio x64（或 x32）本机工具命令提示符窗口，并通过键入以下内容来编译 `cm.cs` 文件：

    > “csc cm.cs”（针对 x64 命令提示符）或“csc -platform:x86 cm.cs”（针对 x32 命令提示符）

    这将创建可执行文件 `cm.exe`。

5. 运行 `cm.exe`。 `SampleMethod` 方法将值 5 传递到 DLL 文件，这将返回该值与 10 相乘后的结果。  该程序生成以下输出：

    ```output
    SampleMethod() returns 50.
    ```

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.DllImportAttribute?displayProperty=nameWithType>
- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [修饰符](index.md)
