---
title: 分部类和方法 - C# 编程指南
description: C# 中的分部类和方法拆分一个类、一个结构、一个接口或一个方法的定义到两个或更多的源文件中。
ms.date: 07/20/2015
helpviewer_keywords:
- partial methods [C#]
- partial classes [C#]
- C# language, partial classes and methods
ms.assetid: 804cecb7-62db-4f97-a99f-60975bd59fa1
ms.openlocfilehash: 792159786131654d6ee0363f7ab7b87ac50d32bb
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864743"
---
# <a name="partial-classes-and-methods-c-programming-guide"></a>分部类和方法（C# 编程指南）

拆分一个[类](../../language-reference/keywords/class.md)、一个[结构](../../language-reference/builtin-types/struct.md)、一个[接口](../../language-reference/keywords/interface.md)或一个方法的定义到两个或更多的文件中是可能的。 每个源文件包含类型或方法定义的一部分，编译应用程序时将把所有部分组合起来。

## <a name="partial-classes"></a>分部类

在以下几种情况下需要拆分类定义：

- 处理大型项目时，使一个类分布于多个独立文件中可以让多位程序员同时对该类进行处理。

- 当使用自动生成的源文件时，你可以添加代码而不需要重新创建源文件。 Visual Studio 在创建Windows 窗体、Web 服务包装器代码等时会使用这种方法。 你可以创建使用这些类的代码，这样就不需要修改由Visual Studio生成的文件。

- 若要拆分类定义，请使用 [partial](../../language-reference/keywords/partial-type.md) 关键字修饰符，如下所示：

  [!code-csharp[csProgGuideObjects#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#26)]

`partial` 关键字指示可在命名空间中定义该类、结构或接口的其他部分。 所有部分都必须使用 `partial` 关键字。 在编译时，各个部分都必须可用来形成最终的类型。 各个部分必须具有相同的可访问性，如 `public`、`private` 等。

如果将任意部分声明为抽象的，则整个类型都被视为抽象的。 如果将任意部分声明为密封的，则整个类型都被视为密封的。 如果任意部分声明基类型，则整个类型都将继承该类。

指定基类的所有部分必须一致，但忽略基类的部分仍继承该基类型。 各个部分可以指定不同的基接口，最终类型将实现所有分部声明所列出的全部接口。 在某一分部定义中声明的任何类、结构或接口成员可供所有其他部分使用。 最终类型是所有部分在编译时的组合。

> [!NOTE]
> `partial` 修饰符不可用于委托或枚举声明中。

下面的示例演示嵌套类型可以是分部的，即使它们所嵌套于的类型本身并不是分部的也如此。

[!code-csharp[csProgGuideObjects#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#25)]

编译时会对分部类型定义的属性进行合并。 以下面的声明为例：

[!code-csharp[csProgGuideObjects#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#23)]

它们等效于以下声明：

[!code-csharp[csProgGuideObjects#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#24)]

将从所有分部类型定义中对以下内容进行合并：

- XML 注释

- 接口

- 泛型类型参数属性

- class 特性

- 成员

以下面的声明为例：

[!code-csharp[csProgGuideObjects#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#21)]

它们等效于以下声明：

[!code-csharp[csProgGuideObjects#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#22)]

### <a name="restrictions"></a>限制

处理分部类定义时需遵循下面的几个规则：

- 要作为同一类型的各个部分的所有分部类型定义都必须使用 `partial` 进行修饰。 例如，下面的类声明会生成错误：

  [!code-csharp[csProgGuideObjects#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#20)]

- `partial` 修饰符只能出现在紧靠关键字 `class`、`struct` 或 `interface` 前面的位置。

- 分部类型定义中允许使用嵌套的分部类型，如下面的示例中所示：

  [!code-csharp[csProgGuideObjects#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#19)]

- 要成为同一类型的各个部分的所有分部类型定义都必须在同一程序集和同一模块（.exe 或 .dll 文件）中进行定义。 分部定义不能跨越多个模块。

- 类名和泛型类型参数在所有的分部类型定义中都必须匹配。 泛型类型可以是分部的。 每个分部声明都必须以相同的顺序使用相同的参数名。

- 下面用于分部类型定义中的关键字是可选的，但是如果某关键字出现在一个分部类型定义中，则该关键字不能与在同一类型的其他分部定义中指定的关键字冲突：

  - [public](../../language-reference/keywords/public.md)

  - [private](../../language-reference/keywords/private.md)

  - [受保护](../../language-reference/keywords/protected.md)

  - [internal](../../language-reference/keywords/internal.md)

  - [abstract](../../language-reference/keywords/abstract.md)

  - [sealed](../../language-reference/keywords/sealed.md)

  - 基类

  - [new](../../language-reference/keywords/new-modifier.md) 修饰符（嵌套部分）

  - 泛型约束

有关详细信息，请参阅[类型参数的约束](../generics/constraints-on-type-parameters.md)。

## <a name="example-1"></a>示例 1

### <a name="description"></a>描述

下面的示例在一个分部类定义中声明 `Coords` 类的字段和构造函数，在另一个分部类定义中声明成员 `PrintCoords`。

### <a name="code"></a>代码

[!code-csharp[csProgGuideObjects#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#17)]

## <a name="example-2"></a>示例 2

### <a name="description"></a>描述

从下面的示例可以看出，你也可以开发分部结构和接口。

### <a name="code"></a>代码

[!code-csharp[csProgGuideObjects#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#18)]

## <a name="partial-methods"></a>分部方法

分部类或结构可以包含分部方法。 类的一个部分包含方法的签名。 可以在同一部分或另一个部分中定义可选实现。 如果未提供该实现，则会在编译时删除方法以及对方法的所有调用。

分部方法使类的某个部分的实施者能够定义方法（类似于事件）。 类的另一部分的实施者可以决定是否实现该方法。 如果未实现该方法，编译器会删除方法签名以及对该方法的所有调用。 调用该方法（包括调用中的任何参数计算结果）在运行时没有任何影响。 因此，分部类中的任何代码都可以随意地使用分部方法，即使未提供实现也是如此。 调用但不实现该方法不会导致编译时错误或运行时错误。

在自定义生成的代码时，分部方法特别有用。 这些方法允许保留方法名称和签名，因此生成的代码可以调用方法，而开发人员可以决定是否实现方法。 与分部类非常类似，分部方法使代码生成器创建的代码和开发人员创建的代码能够协同工作，而不会产生运行时开销。

分部方法声明由两个部分组成：定义和实现。 它们可以位于分部类的不同部分中，也可以位于同一部分中。 如果不存在实现声明，则编译器会优化定义声明和对方法的所有调用。

```csharp
// Definition in file1.cs
partial void onNameChanged();

// Implementation in file2.cs
partial void onNameChanged()
{
  // method body
}
```

- 分部方法声明必须以上下文关键字 [partial](../../language-reference/keywords/partial-type.md) 开头，并且方法必须返回 [void](../../language-reference/builtin-types/void.md)。

- 分部方法可以有 [in](../../language-reference/keywords/in-parameter-modifier.md) 或 [ref](../../language-reference/keywords/ref.md) 参数，但不能有 [out](../../language-reference/keywords/out-parameter-modifier.md) 参数。

- 分部方法为隐式 [private](../../language-reference/keywords/private.md) 方法，因此不能为 [virtual](../../language-reference/keywords/virtual.md) 方法。

- 分部方法不能为 [extern](../../language-reference/keywords/extern.md) 方法，因为主体的存在确定了方法是在定义还是在实现。

- 分部方法可以有 [static](../../language-reference/keywords/static.md) 和 [unsafe](../../language-reference/keywords/unsafe.md) 修饰符。

- 分部方法可以是泛型的。 约束将放在定义分部方法声明上，但也可以选择重复放在实现声明上。 参数和类型参数名称在实现声明和定义声明中不必相同。

- 你可以为已定义并实现的分部方法生成[委托](../../language-reference/builtin-types/reference-types.md)，但不能为已经定义但未实现的分部方法生成委托。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[分部类型](~/_csharplang/spec/classes.md#partial-types)。 该语言规范是 C# 语法和用法的权威资料。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类](./classes.md)
- [结构类型](../../language-reference/builtin-types/struct.md)
- [接口](../interfaces/index.md)
- [分部（类型）](../../language-reference/keywords/partial-type.md)
