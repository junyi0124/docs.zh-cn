---
title: 友元程序集
description: 友元程序集是可以访问其他程序集的内部 (C#) 或友元 (Visual Basic) 类型和成员的 .NET 程序集。
ms.date: 08/20/2019
ms.assetid: b65ea7de-0801-477a-a39c-e914c2cc107c
dev_langs:
- csharp
- vb
ms.openlocfilehash: 105621da2bd418c6294fa2bbec474809599cb6a5
ms.sourcegitcommit: d6bd7903d7d46698e9d89d3725f3bb4876891aa3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83378931"
---
# <a name="friend-assemblies"></a>友元程序集

友元程序集是可以访问其他程序集的[内部](../../csharp/language-reference/keywords/internal.md) (C#) 或[友元](../../visual-basic/language-reference/modifiers/friend.md) (Visual Basic) 类型和成员的程序集。 如果将一个程序集标识为友元程序集，则无需再将类型和成员标记为公共，其他程序集就能访问它们。 此举在下列情境中尤其方便：

- 在单元测试期间，当测试代码在单独的程序集上运行，但要求访问标记为 C# 中的 `internal` 或 Visual Basic 中的 `Friend` 的正在被测试的程序集中的成员时。

- 当你正在开发类库，库的添加包含在单独的程序集中，但要求访问标记为 C# 中的 `internal` 或 Visual Basic 中的 `Friend` 的现有程序集中的成员时。

## <a name="remarks"></a>备注

可使用 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性标识给定程序集的一个或多个友元程序集。 以下示例使用程序集 A 中的 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性，并将程序集 B 指定为友元程序集 。 这使程序集 B 能访问标记为 C# 中的 `internal` 或 Visual Basic 中的 `Friend` 的程序集 A 中的所有类型和成员 。

> [!NOTE]
> 如果编译的程序集（如程序集 B）将访问另一个程序集（如程序集 A）的内部类型或内部成员，则必须通过使用 -out 编译器选项显式指定输出文件（.exe 或 .dll）的名称   。 这是必需的，因为编译器尚未为它在绑定到外部引用时而正在构建的程序集生成名称。 有关详细信息，请参阅 [-out (C#)](../../csharp/language-reference/compiler-options/out-compiler-option.md) 或 [-out (Visual Basic)](../../visual-basic/reference/command-line-compiler/out.md)。

```csharp
using System.Runtime.CompilerServices;
using System;

[assembly: InternalsVisibleTo("AssemblyB")]

// The class is internal by default.
class FriendClass
{
    public void Test()
    {
        Console.WriteLine("Sample Class");
    }
}

// Public class that has an internal method.
public class ClassWithFriendMethod
{
    internal void Test()
    {
        Console.WriteLine("Sample Method");
    }

}
```

```vb
Imports System.Runtime.CompilerServices
<Assembly: InternalsVisibleTo("AssemblyB")>

' Friend class.
Friend Class FriendClass
    Public Sub Test()
        Console.WriteLine("Sample Class")
    End Sub
End Class

' Public class with a Friend method.
Public Class ClassWithFriendMethod
    Friend Sub Test()
        Console.WriteLine("Sample Method")
    End Sub
End Class
```

只有显式指定为友元的程序集才能访问`internal` (C#) 或 `Friend` (Visual Basic) 类型和成员。 例如，如果程序集 B 是程序集 A 的友元，且程序集 C 引用了程序集 B，则 C 无法访问程序集 A 中的 `internal` (C#) 或 `Friend` (Visual Basic) 类型     。

编译器对传递到 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性的友元程序集名称执行一些基本验证。 如果程序集 A 将程序集 B 声明为友元程序集，则验证规则如下 ：

- 如果程序集 A 是强名称的程序集，那么程序集 B 也必须是强名称的 。 传递到属性的友元程序集名称必须包含程序集名称和用于对程序集 B 签名的强名称密钥的公钥。

     传递到 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute> 属性的友元程序集名称不能是程序集 B 的强名称。 请勿包含程序集版本、区域性、体系结构或公钥令牌。

- 如果程序集 A 不是强名称，则友元程序集名称应仅由程序集名称构成。 有关详细信息，请参阅[如何：创建未签名的友元程序集](create-unsigned-friend.md)。

- 如果程序集 B 是强名称，则必须使用项目设置或命令行 `/keyfile` 编译器选项为程序集 B 指定强名称密钥 。 有关详细信息，请参阅[如何：创建已签名的友元程序集](create-signed-friend.md)。

 <xref:System.Security.Permissions.StrongNameIdentityPermission> 类还提供类型共享功能，但具有以下差异：

- <xref:System.Security.Permissions.StrongNameIdentityPermission> 应用到单个类型，而友元程序集应用到整个程序集。

- 如果要与程序集 B 共享程序集 A 中的数百个类型，则必须向它们全部添加 <xref:System.Security.Permissions.StrongNameIdentityPermission> 。 如果使用友元程序集，只需声明友元关系一次。

- 如果使用 <xref:System.Security.Permissions.StrongNameIdentityPermission>，必须将要共享的类型声明为公共类型。 如果使用友元程序集，共享的类型会声明为 `internal` (C#) 或 `Friend` (Visual Basic)。

如需了解如何从模块文件（扩展名为 .netmodule 的文件）访问程序集的 `internal` (C#) 或 `Friend` (Visual Basic) 类型和方法，请参阅 [-moduleassemblyname (C#)](../../csharp/language-reference/compiler-options/moduleassemblyname-compiler-option.md) 或 [-moduleassemblyname (Visual Basic)](../../visual-basic/reference/command-line-compiler/moduleassemblyname.md)。

## <a name="see-also"></a>请参阅

- <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>
- <xref:System.Security.Permissions.StrongNameIdentityPermission>
- [如何：创建未签名的友元程序集](create-unsigned-friend.md)
- [如何：创建已签名的友元程序集](create-signed-friend.md)
- [.NET 中的程序集](index.md)
- [C# 编程指南](../../csharp/programming-guide/index.md)
- [编程概念 (Visual Basic)](../../visual-basic/programming-guide/concepts/index.md)
