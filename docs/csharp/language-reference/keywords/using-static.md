---
description: using 静态指令 - C# 参考
title: using 静态指令 - C# 参考
ms.date: 03/10/2017
f1_keywords:
- using-static_CSharpKeyword
helpviewer_keywords:
- using static directive [C#]
ms.assetid: 8b8f9e34-c75e-469b-ba85-6f2eb4090314
ms.openlocfilehash: d117d4423a2f7c782cd6365a73e6c18298d89abc
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162227"
---
# <a name="using-static-directive-c-reference"></a>using 静态指令（C# 参考）

`using static` 指令指定一种类型，无需指定类型名称即可访问其静态成员和嵌套类型。 语法为：

```csharp
using static <fully-qualified-type-name>;
```

其中，fully-qualified-type-name 是无需指定类型名称即可访问其静态成员和嵌套类型的类型名称。 如果你不提供完全限定的类型名称（完整的命名空间名称以及类型名称），C# 便会生成编译器错误 [CS0246](../compiler-messages/cs0246.md)：“找不到类型名称或命名空间名称 ’type/namespace’ (是否缺少 using 指令或程序集引用?)”。

`using static` 指令适用于任何具有静态成员（或嵌套类型）的类型，即使该类型还具有实例成员。 但是，只能通过类型实例来调用实例成员。

`using static` 指令是在 C# 6 中引入的。

## <a name="remarks"></a>备注

通常，调用某个静态成员时，即会提供类型名称以及成员名称。 重复输入相同的类型名称来调用该类型的成员将生成详细的晦涩代码。 例如，`Circle` 类的以下定义引用 <xref:System.Math> 类的成员数。

[!code-csharp[using-static#1](~/samples/snippets/csharp/language-reference/keywords/using/using-static1.cs#1)]

通过消除每次引用成员时，显式引用 <xref:System.Math> 类的需求，`using static` 指令将生成更简洁的代码：

[!code-csharp[using-static#2](~/samples/snippets/csharp/language-reference/keywords/using/using-static2.cs#1)]

`using static` 仅导入可访问的静态成员和指定类型中声明的嵌套类型。  不导入继承的成员。  可以从任何带 using static 指令的已命名类型导入，包括 Visual Basic 模块。  如果 F# 顶级函数在元数据中显示为一个已命名类型（其名称是有效的 C# 标识符）的静态成员，则可以导入该 F# 函数。

 `using static` 使指定类型中声明的扩展方法可用于扩展方法查找。  但是，扩展方法的名称不导入到代码中非限定引用的作用域中。

 同一编译单元或命名空间中通过不同 `using static` 命令从不同类型导入的具有相同名称的方法组成一个方法组。  这些方法组内的重载解决方法遵循一般 C# 规则。

## <a name="example"></a>示例

以下示例使用 `using static` 指令来提供 <xref:System.Console>、<xref:System.Math> 和 <xref:System.String> 类的静态成员，而无需指定其类型名称。

[!code-csharp[using-static#3](~/samples/snippets/csharp/language-reference/keywords/using/using-static3.cs)]

在此示例中，`using static` 指令也已经应用于 <xref:System.Double> 类型。 这使得在未指定类型名称情况下调用 <xref:System.Double.TryParse(System.String,System.Double@)> 方法成为可能。 但是，如此创建的代码可读性较差，因为这样有必要检查 `using static` 指令，以确定所调用的数值类型的 `TryParse` 方法。

## <a name="see-also"></a>请参阅

- [using 指令](using-directive.md)
- [C# 参考](../index.md)
- [C# 关键字](index.md)
- [使用命名空间](../../programming-guide/namespaces/using-namespaces.md)
- [命名空间](../../programming-guide/namespaces/index.md)
