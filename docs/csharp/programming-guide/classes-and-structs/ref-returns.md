---
title: ref 返回值和 ref 局部变量（C# 指南）
description: 了解如何定义和使用 ref 返回值和 ref 局部变量
ms.date: 04/04/2018
ms.openlocfilehash: 87a9538db60d69062f0fb48ed9683a9d4f972b91
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170068"
---
# <a name="ref-returns-and-ref-locals"></a>ref 返回值和局部变量

从 C# 7.0 开始，C# 支持引用返回值（ref 返回值）。 借助引用返回值，方法可以将对变量的引用（而不是值）返回给调用方。 然后，调用方可以选择将返回的变量视为按值返回或按引用返回。 调用方可以新建称为“引用本地”的变量，其本身就是对返回值的引用。

## <a name="what-is-a-reference-return-value"></a>什么是引用返回值？

绝大多数开发者都很熟悉将参数按引用传递给被调用的方法  。 已调用方法的参数列表包含以引用方式传递的变量。 调用方会监测已调用方法对其值做出的任何更改。 引用返回值  是指，方法返回对某变量的引用  （或别名）。 相应变量的作用域必须包括方法。 相应变量的生存期必须超过方法的返回值。 调用方对方法的返回值进行的修改应用于方法返回的变量。

如果声明方法返回引用返回值  ，表明方法返回变量别名。 这样做通常是为了让调用代码有权通过别名访问此变量（包括修改它）。 因此，方法的引用返回值不得包含返回类型 `void`。

对于方法能以引用返回值的形式返回的表达式，存在一些限制。 具体限制包括：

- 返回值的生存期必须长于方法执行时间。 换言之，它不能是返回自身的方法中的本地变量。 它可以是实例或类的静态字段，也可是传递给方法的参数。 尝试返回局部变量将生成编译器错误 CS8168：“无法按引用返回局部 "obj"，因为它不是 ref 局部变量”。

- 返回值不得为文本 `null`。 返回 `null` 会生成编译器错误 CS8156“无法在此上下文中使用表达式，因为它可能不是以引用方式返回”。

   使用引用返回值的方法可以返回值当前为 NULL（未实例化）或[可为空的值类型](../../language-reference/builtin-types/nullable-value-types.md)的变量别名。

- 返回值不得为常量、枚举成员、通过属性的按值返回值或 `class`/`struct` 方法。 违反此规则会生成编译器错误 CS8156“无法在此上下文中使用表达式，因为它可能不是以引用方式返回”。

此外，禁止对异步方法使用引用返回值。 异步方法可能会在执行尚未完成时就返回值，尽管返回值仍未知。

## <a name="defining-a-ref-return-value"></a>定义 ref 返回值

返回引用返回值  的方法必须满足以下两个条件：

- 方法签名在返回类型前面有 [ref](../../language-reference/keywords/ref.md) 关键字。
- 方法主体中的每个 [return](../../language-reference/keywords/return.md) 语句都在返回实例的名称前面有 [ref](../../language-reference/keywords/ref.md) 关键字。

下面的示例方法满足这些条件，且返回对名为 `p` 的 `Person` 对象的引用：

```csharp
public ref Person GetContactInformation(string fname, string lname)
{
    // ...method implementation...
    return ref p;
}
```

## <a name="consuming-a-ref-return-value"></a>使用 ref 返回值

引用返回值是被调用方法范围中另一个变量的别名。 可以将引用返回值的所有使用都解释为，使用它取别名的变量：

- 分配值时，就是将值分配到它取别名的变量。
- 读取值时，就是读取它取别名的变量的值。
- 如果以引用方式  返回它，就是返回对相同变量所取的别名。
- 如果以引用方式  将它传递到另一个方法，就是传递对它取别名的变量的引用。
- 如果返回[引用本地](#ref-locals)别名，就是返回相同变量的新别名。

## <a name="ref-locals"></a>ref 局部变量

假设 `GetContactInformation` 方法声明为引用返回：

```csharp
public ref Person GetContactInformation(string fname, string lname)
```

按值分配会读取变量值，并将它分配给新变量：

```csharp
Person p = contacts.GetContactInformation("Brandie", "Best");
```

上面的分配将 `p` 声明为本地变量。 它的初始值是通过读取 `GetContactInformation` 返回的值进行复制。 之后对 `p` 的任何分配都不会更改 `GetContactInformation` 返回的变量值。 变量 `p` 不再是返回的变量的别名。

声明引用本地  变量，复制原始值的别名。 在下面的分配中，`p` 是从 `GetContactInformation` 返回的变量的别名。

```csharp
ref Person p = ref contacts.GetContactInformation("Brandie", "Best");
```

后续使用 `p` 等同于使用 `GetContactInformation` 返回的变量，因为 `p` 是此变量的别名。 对 `p` 所做的更改也会更改从 `GetContactInformation` 返回的变量。

`ref` 关键字用于局部变量声明前面和  方法调用前面。

可通过相同方式按引用访问值。 在某些情况下，按引用访问值可避免潜在的高开销复制操作，从而提高性能。 例如，以下语句显示用户可如何定义一个用于引用值的 ref 局部变量值。

```csharp
ref VeryLargeStruct reflocal = ref veryLargeStruct;
```

`ref` 关键字用于局部变量声明前面和  第二个示例中的值前面。 在这两个示例中，如果无法同时将 `ref` 关键字包含在变量声明和赋值中，则会导致编译器错误 CS8172：“无法使用值对按引用变量进行初始化”。

在低于 C# 7.3 的版本中，无法将 ref 局部变量重新分配为，在初始化后引用其他存储。 此限制已取消。 下面的示例展示了如何重新分配：

```csharp
ref VeryLargeStruct reflocal = ref veryLargeStruct; // initialization
refLocal = ref anotherVeryLargeStruct; // reassigned, refLocal refers to different storage.
```

 Ref 局部变量仍必须在声明时进行初始化。

## <a name="ref-returns-and-ref-locals-an-example"></a>ref 返回结果和 ref 局部变量：示例

下列示例定义存储整数值数组的 `NumberStore` 类。 `FindNumber` 方法按引用返回第一个大于或等于作为参数传递的数字的数字。 如果没有大于或等于该参数的数字，则方法返回索引 0 中的数字。

[!code-csharp[ref-returns](../../../../samples/snippets/csharp/programming-guide/ref-returns/NumberStore.cs#1)]

下列示例调用 `NumberStore.FindNumber` 方法来检索大于或等于 16 的第一个值。 然后，调用方将该方法返回的值加倍。 示例输出表明，`NumberStore` 实例的数组元素值反映了更改。

[!code-csharp[ref-returns](../../../../samples/snippets/csharp/programming-guide/ref-returns/NumberStore.cs#2)]

如果引用返回值不受支持，需要通过返回数组元素及其值的索引来执行此类操作。 然后，调用方可使用此索引修改单个方法调用中的值。 但调用方也可修改要访问的索引，还可修改其他数组值。  

下面的示例展示了如何在高于 C# 7.3 的版本中将 `FindNumber` 方法重写为使用 ref 局部重新分配：

[!code-csharp[ref-returns](../../../../samples/snippets/csharp/programming-guide/ref-returns/NumberStoreUpdated.cs#1)]

如果查找的数字更接近数组末尾，这第二个版本就更高效且序列更长。

## <a name="see-also"></a>另请参阅

- [ref 关键字](../../language-reference/keywords/ref.md)
- [编写安全高效的代码](../../write-safe-efficient-code.md)
