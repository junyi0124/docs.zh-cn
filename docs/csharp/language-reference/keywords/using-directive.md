---
description: using 指令 - C# 参考
title: using 指令 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- using_CSharpKeyword
helpviewer_keywords:
- using directive [C#]
ms.assetid: b42b8e61-5e7e-439c-bb71-370094b44ae8
ms.openlocfilehash: 77d9c894dae9adc24343ce3a639a4afb904fb0a1
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599404"
---
# <a name="using-directive-c-reference"></a>using 指令（C# 参考）

`using` 指令有三种用途：

- 允许在命名空间中使用类型，这样无需在该命名空间中限定某个类型的使用：

    ```csharp
    using System.Text;
    ```

- 允许访问类型的静态成员和嵌套类型，而无需限定使用类型名称进行访问。

    ```csharp
    using static System.Math;
    ```

    有关详细信息，请参阅 [using static 指令](using-static.md)。

- 为命名空间或类型创建别名。 这称为 *using 别名指令*。

    ```csharp
    using Project = PC.MyCompany.Project;
    ```

`using` 关键字还用于创建 using 语句，此类语句有助于确保正确处理 <xref:System.IDisposable> 对象（如文件和字体）。 有关详细信息，请参阅 [using 语句](using-statement.md)。

## <a name="using-static-type"></a>Using 静态类型

你可以访问类型的静态成员，而无需限定使用类型名称进行访问：

```csharp
using static System.Console;
using static System.Math;
class Program
{
    static void Main()
    {
        WriteLine(Sqrt(3*3 + 4*4));
    }
}
```

## <a name="remarks"></a>备注

`using` 指令的范围限于显示它的文件。

可能出现 `using` 指令的位置：

- 源代码文件的开头，位于任何命名空间或类型定义之前。
- 在任何命名空间中，但位于此命名空间中声明的任何命名空间或类型之前。

否则，将生成编译器错误 [CS1529](../../misc/cs1529.md)。

创建 `using` 别名指令，以便更易于将标识符限定为命名空间或类型。 在任何 `using` 指令中，都必须使用完全限定的命名空间或类型，而无需考虑它之前的 `using` 指令。 `using` 指令的声明中不能使用 `using` 别名。 例如，以下代码生成一个编译器错误：

```csharp
using s = System.Text;
using s.RegularExpressions; // Generates a compiler error.
```

创建 `using` 指令，以便在命名空间中使用类型而不必指定命名空间。 `using` 指令不为你提供对嵌套在指定命名空间中的任何命名空间的访问权限。

命名空间分为两类：用户定义的命名空间和系统定义的命名空间。 用户定义的命名空间是在代码中定义的命名空间。 有关系统定义的命名空间的列表，请参阅 [.NET API 浏览器](../../../../api/index.md)。

## <a name="example-1"></a>示例 1

下面的示例显示如何为命名空间定义和使用 `using` 别名：

[!code-csharp[csrefKeywordsNamespace#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace2.cs#8)]

using 别名指令的右侧不能有开放式泛型类型。 例如，不能为 `List<T>` 创建 using 别名，但可以为 `List<int>` 创建 using 别名。

## <a name="example-2"></a>示例 2

下面的示例显示如何为类定义 `using` 指令和 `using` 别名：

[!code-csharp[csrefKeywordsNamespace#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace2.cs#9)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[ Using 指令](~/_csharplang/spec/namespaces.md#using-directives)。 该语言规范是 C# 语法和用法的权威资料。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [使用命名空间](../../programming-guide/namespaces/using-namespaces.md)
- [C# 关键字](index.md)
- [命名空间](../../programming-guide/namespaces/index.md)
- [using 语句](using-statement.md)
