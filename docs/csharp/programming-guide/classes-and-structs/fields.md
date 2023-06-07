---
title: 字段 - C# 编程指南
description: C# 中的字段是在类或结构中直接声明的任意类型的变量。 字段是其包含类型的成员。
ms.date: 07/20/2015
helpviewer_keywords:
- fields [C#]
ms.assetid: 3cbb2f61-75f8-4cce-b4ef-f5d1b3de0db7
ms.openlocfilehash: 9bd2e198cd623788a21d4da73e89851a6d77e3bb
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474782"
---
# <a name="fields-c-programming-guide"></a>字段（C# 编程指南）

字段是在[类](../../language-reference/keywords/class.md)或[结构](../../language-reference/builtin-types/struct.md)中直接声明的任意类型的变量。 字段是其包含类型的成员。

类或结构可能具有实例字段和/或静态字段。 实例字段特定于类型的实例。 如果你有包含实例字段 F 的类 T，则可以创建两个类型为 T 的对象并修改每个对象中 F 的值，而不会影响另一个对象中的值。 与此相比，静态字段属于类本身，并在该类的所有实例之间共享。 只能使用类名称访问静态字段。 如果按实例名称访问静态字段，将出现 [CS0176](../../misc/cs0176.md) 编译时错误。

通常情况下，应仅对具有 private 或 protected 可访问性的变量使用字段。 类向客户端代码公开的数据应通过[方法](./methods.md)、[属性](./properties.md)和[索引器](../indexers/index.md)提供。 通过使用这些构造间接访问内部字段，可以防止出现无效的输入值。 存储由公共属性公开的数据的私有字段称为后备存储或支持字段。

字段通常存储必须对多个类方法可访问且存储时间必须长于任何单个方法的生存期的数据。 例如，表示日历日期的类可能具有三个整数字段：一个用于月、一个用于日、一个用于年。 不在单个方法作用域外使用的变量应声明为方法主体本身中的局部变量。

字段是通过指定该字段的访问级别在类块中声明的，其后跟字段的类型，再跟字段的名称。 例如:

[!code-csharp[csProgGuideObjects#61](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#61)]

若要访问对象中的字段，请在对象名称后添加一个句点，后跟字段的名称，如 `objectname.fieldname` 中所示。 例如：

[!code-csharp[csProgGuideObjects#62](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#62)]

声明字段时，可以使用赋值运算符为字段指定一个初始值。 例如，若要为 `day` 字段自动赋值 `"Monday"`，则需要声明 `day`，如以下示例所示：

[!code-csharp[csProgGuideObjects#63](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideObjects/CS/Objects.cs#63)]

字段会在对象实例的构造函数被调用之前即刻初始化。 如果构造函数分配了字段的值，则它将覆盖在字段声明期间给定的任何值。 有关详细信息，请参阅[使用构造函数](./using-constructors.md)。

> [!NOTE]
> 字段初始化表达式不能引用其他实例字段。

可以将字段标记为 [public](../../language-reference/keywords/public.md)、[private](../../language-reference/keywords/private.md)、[protected](../../language-reference/keywords/protected.md)、[internal](../../language-reference/keywords/internal.md)、[protected internal](../../language-reference/keywords/protected-internal.md) 或 [private protected](../../language-reference/keywords/private-protected.md)。 这些访问修饰符定义该类的用户访问该字段的方式。 有关详细信息，请参阅[访问修饰符](./access-modifiers.md)。

可以选择性地将字段声明为[静态](../../language-reference/keywords/static.md)。 这可使字段可供调用方在任何时候进行调用，即使不存在任何类的实例。 有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。

可以将字段声明为[只读](../../language-reference/keywords/readonly.md)。 只能在初始化期间或在构造函数中为只读字段赋值。 `static readonly` 字段非常类似于常量，只不过 C# 编译器在编译时不具有对静态只读字段的值的访问权限，而只有在运行时才具有访问权限。 有关详细信息，请参阅[常量](./constants.md)。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类和结构](./index.md)
- [使用构造函数](./using-constructors.md)
- [继承](./inheritance.md)
- [访问修饰符](./access-modifiers.md)
- [抽象类、密封类及类成员](./abstract-and-sealed-classes-and-class-members.md)
