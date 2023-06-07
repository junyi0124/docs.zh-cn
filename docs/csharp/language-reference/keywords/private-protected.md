---
description: private protected - C# 参考
title: private protected - C# 参考
ms.date: 11/15/2017
f1_keywords:
- privateprotected_CSharpKeyword
author: sputier
ms.openlocfilehash: e7ef6d691b43abd3d07321adfc0c166629ce9098
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90537296"
---
# <a name="private-protected-c-reference"></a>private protected（C# 参考）

`private protected` 关键字组合是一种成员访问修饰符。 仅派生自包含类的类型可访问私有受保护成员，但仅能在其包含程序集中访问。 有关 `private protected` 和其他访问修饰符的比较，请参阅[可访问性级别](accessibility-levels.md)。

> [!NOTE]
> `private protected` 访问修饰符在 C# 版本 7.2 及更高版本中有效。

## <a name="example"></a>示例

仅当变量的静态类型是派生类类型时，可从派生的类型访问基类的私有受保护成员（在其包含程序集中访问）。 以下面的代码段为例：

```csharp
public class BaseClass
{
    private protected int myValue = 0;
}

public class DerivedClass1 : BaseClass
{
    void Access()
    {
        var baseObject = new BaseClass();

        // Error CS1540, because myValue can only be accessed by
        // classes derived from BaseClass.
        // baseObject.myValue = 5;

        // OK, accessed through the current derived class instance
        myValue = 5;
    }
}
```

```csharp
// Assembly2.cs
// Compile with: /reference:Assembly1.dll
class DerivedClass2 : BaseClass
{
    void Access()
    {
        // Error CS0122, because myValue can only be
        // accessed by types in Assembly1
        // myValue = 10;
    }
}
```

此示例包含两个文件，即 `Assembly1.cs` 和 `Assembly2.cs`。
第一个文件包含公共基类 `BaseClass` 及其派生的类型 `DerivedClass1`。 `BaseClass` 拥有私有受保护成员 `myValue`，`DerivedClass1` 尝试以两种方式访问该成员。 通过 `BaseClass` 的实例第一次尝试访问 `myValue` 时会产生错误。 但是，如果尝试在 `DerivedClass1` 中将其用作继承的成员，则会成功。

在第二个文件中，如果尝试将 `myValue` 作为 `DerivedClass2` 的继承成员进行访问，会生成错误，因为仅 Assembly1 中的派生类型可以访问它。

如果 `Assembly1.cs` 包含命名为 `Assembly2` 的 <xref:System.Runtime.CompilerServices.InternalsVisibleToAttribute>，则派生类 `DerivedClass1` 将可以访问 `BaseClass` 中声明的 `private protected` 成员。 `InternalsVisibleTo` 使 `private protected` 成员对其他程序集中的派生类可见。

结构成员不能为 `private protected`，因为无法继承结构。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [访问修饰符](access-modifiers.md)
- [可访问性级别](accessibility-levels.md)
- [修饰符](index.md)
- [public](public.md)
- [private](private.md)
- [internal](internal.md)
- [Internal Virtual 关键字的安全问题](/previous-versions/dotnet/netframework-4.0/heyd8kky(v=vs.100))
