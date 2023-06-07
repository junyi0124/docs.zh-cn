---
description: class 关键字 - C# 参考
title: class 关键字 - C# 参考
ms.date: 07/18/2017
f1_keywords:
- class_CSharpKeyword
- class
helpviewer_keywords:
- class keyword [C#]
ms.assetid: b95d8815-de18-4c3f-a8cc-a0a53bdf8690
ms.openlocfilehash: 67c9c4be55cce25edf9ecb84b257a8523f193bec
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89142110"
---
# <a name="class-c-reference"></a>class（C# 参考）

使用 `class` 关键字声明类，如下例中所示：

```csharp
class TestClass
{
    // Methods, properties, fields, events, delegates
    // and nested classes go here.
}
```

## <a name="remarks"></a>备注

在 C# 中仅允许单一继承。 也就是说，一个类仅能从一个基类继承实现。 但是，一个类可实现多个接口。 下表显示类继承和接口实现的一些示例：

|继承|示例|
|-----------------|-------------|
|无|`class ClassA { }`|
|单向|`class DerivedClass : BaseClass { }`|
|无，实现两个接口|`class ImplClass : IFace1, IFace2 { }`|
|单一，实现一个接口|`class ImplDerivedClass : BaseClass, IFace1 { }`|

直接在命名空间中声明的、未嵌套在其它类中的类，可以是[公共](./public.md)或[内部](./internal.md)。 默认情况下类为 `internal`。

类成员（包括嵌套的类）可以是 [public](public.md)、[protected internal](protected-internal.md)、[protected](protected.md)、[internal](internal.md)、[private](private.md) 或 [private protected](private-protected.md)。 默认情况下成员为 `private`。

有关详细信息，请参阅[访问修饰符](../../programming-guide/classes-and-structs/access-modifiers.md)。

可以声明具有类型参数的泛型类。 有关更多信息，请参见[泛型类](../../programming-guide/generics/generic-classes.md)。

一个类可包含下列成员的声明：

- [构造函数](../../programming-guide/classes-and-structs/constructors.md)

- [常量](../../programming-guide/classes-and-structs/constants.md)

- [字段](../../programming-guide/classes-and-structs/fields.md)

- [终结器](../../programming-guide/classes-and-structs/destructors.md)

- [方法](../../programming-guide/classes-and-structs/methods.md)

- [属性](../../programming-guide/classes-and-structs/properties.md)

- [索引器](../../programming-guide/indexers/index.md)

- [运算符](../operators/index.md)

- [事件](../../programming-guide/events/index.md)

- [委托](../../programming-guide/delegates/index.md)

- [类](../../programming-guide/classes-and-structs/classes.md)

- [接口](../../programming-guide/interfaces/index.md)

- [结构类型](../builtin-types/struct.md)

- [枚举类型](../builtin-types/enum.md)

## <a name="example"></a>示例

下面的示例说明如何声明类字段、构造函数和方法。 该示例还说明如何实例化对象及如何打印实例数据。 本例声明了两个类。 第一个类 `Child` 包含两个私有字段（`name` 和 `age`）、两个公共构造函数和一个公共方法。 第二个类 `StringTest` 用于包含 `Main`。

[!code-csharp[csrefKeywordsTypes#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#5)]

## <a name="comments"></a>注释

注意：在上例中，私有字段（`name` 和 `age`）只能通过 `Child` 类的公共方法访问。 例如，不能在 `Main` 方法中使用如下语句打印 Child 的名称：

```csharp
Console.Write(child1.name);   // Error
```

只有当 `Main` 是类的成员时，才能从 `Main` 访问 `Child` 的私有成员。

类中不具有访问修饰符的已声明类型默认为 `private`，因此如果已删除关键字，则此示例中的数据成员将仍为 `private`。

最后要注意的是，默认情况下，对于使用无参数构造函数 (`child3`) 创建的对象，`age` 字段初始化为零。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](./index.md)
- [引用类型](./reference-types.md)
