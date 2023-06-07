---
description: protected internal - C# 参考
title: protected internal - C# 参考
ms.date: 11/15/2017
f1_keywords:
- protectedinternal_CSharpKeyword
author: sputier
ms.openlocfilehash: f22de104154df74f02c048b1209eeac754a03e64
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90536792"
---
# <a name="protected-internal-c-reference"></a>受保护的内部成员（C# 参考）

`protected internal` 关键字组合是一种成员访问修饰符。 可从当前程序集或派生自包含类的类型访问受保护的内部成员。 有关 `protected internal` 和其他访问修饰符的比较，请参阅[可访问性级别](accessibility-levels.md)。

## <a name="example"></a>示例

可从包含程序集内的任何类型访问基类的受保护的内部成员。 也可在另一程序集中的派生类中访问它，前提是通过派生类类型的变量进行访问。 以下面的代码段为例：

```csharp
// Assembly1.cs
// Compile with: /target:library
public class BaseClass
{
   protected internal int myValue = 0;
}

class TestAccess
{
    void Access()
    {
        var baseObject = new BaseClass();
        baseObject.myValue = 5;
    }
}
```

```csharp
// Assembly2.cs
// Compile with: /reference:Assembly1.dll
class DerivedClass : BaseClass
{
    static void Main()
    {
        var baseObject = new BaseClass();
        var derivedObject = new DerivedClass();

        // Error CS1540, because myValue can only be accessed by
        // classes derived from BaseClass.
        // baseObject.myValue = 10;

        // OK, because this class derives from BaseClass.
        derivedObject.myValue = 10;
    }
}
```

此示例包含两个文件，即 `Assembly1.cs` 和 `Assembly2.cs`。
第一个文件包含公共基类 `BaseClass` 和另一个类 `TestAccess`。 `BaseClass` 拥有受保护的内部成员 `myValue`，由 `TestAccess` 类型访问。
在第二个文件中，如果尝试通过 `BaseClass` 的实例访问 `myValue` ，会生成错误，但如果尝试通过一个派生类的实例来访问此成员，`DerivedClass` 会成功。

结构成员不能为 `protected internal`，因为无法继承结构。

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
