---
title: 抽象类
description: 了解F#抽象类，这些类使部分或全部成员未实现，并表示一组不同的对象类型的常用功能。
ms.date: 05/16/2016
ms.openlocfilehash: d7fc87178cff7c5c824992c97198b49f87025f00
ms.sourcegitcommit: a2d0e1f66367367065bc8dc0dde488ab536da73f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082947"
---
# <a name="abstract-classes"></a>抽象类

*抽象类*是保留部分或全部成员未实现的类，以便实现可由派生类提供。

## <a name="syntax"></a>语法

```fsharp
// Abstract class syntax.
[<AbstractClass>]
type [ accessibility-modifier ] abstract-class-name =
[ inherit base-class-or-interface-name ]
[ abstract-member-declarations-and-member-definitions ]

// Abstract member syntax.
abstract member member-name : type-signature
```

## <a name="remarks"></a>备注

在面向对象的编程中，抽象类用作层次结构的基类，并表示一组不同的对象类型的常用功能。 顾名思义，抽象类通常不直接与问题域中的具体实体直接对应。 不过，它们确实代表了许多不同的具体实体。

抽象类必须具有`AbstractClass`特性。 它们可以具有实现和未实现的成员。 当应用于类时，术语 "*抽象*" 的使用与其他 .net 语言相同;但是，在应用于方法（和属性）时，术语 "*抽象*" 的用法与在其他F# .net 语言中使用的方法稍有不同。 在F#中，当使用`abstract`关键字标记方法时，这表示成员在该类型的虚函数的内部表中具有一个称为*虚拟调度槽*的条目。 换句话说，方法为虚方法，但`virtual`该F#语言中未使用关键字。 关键字`abstract`用于虚拟方法，而不考虑是否实现方法。 虚拟调度槽的声明独立于该调度槽的方法的定义。 因此，在F#其他 .net 语言中，虚拟方法声明和定义的等效项是抽象方法声明和单独定义的组合，其中包含`default`关键字或`override`关键字。 有关详细信息和示例，请参阅[方法](./members/methods.md)。

仅当存在已声明但未定义的抽象方法时，才会将类视为抽象方法。 因此，具有抽象方法的类不一定是抽象类。 除非类具有未定义的抽象方法，否则不使用**AbstractClass**特性。

在前面的语法中，可*访问性修饰符*可以`private`是`internal` `public`或。 有关详细信息，请参阅[访问控制](access-control.md)。

与其他类型一样，抽象类可以具有基类和一个或多个基接口。 每个基类或接口都显示在单独的行上， `inherit`并带有关键字。

抽象类的类型定义可包含完全定义的成员，但它还可以包含抽象成员。 在前面的语法中，将单独显示抽象成员的语法。 在此语法中，成员的*类型签名*是一个列表，其中包含按顺序排列的参数类型和返回类型（根据适用`->`于扩充和元`*`组参数的适当标记和/或标记分隔）。 抽象成员类型签名的语法与签名文件中使用的语法相同，并且在 Visual Studio Code 编辑器中由 IntelliSense 显示。

下面的代码演示一个抽象类形状，它具有两个非抽象的派生类（正方形和圆圈）。 该示例演示如何使用抽象类、方法和属性。 在此示例中，抽象类形状表示具体实体 circle 和正方形的公共元素。 所有形状（在二维坐标系统中）的常见功能都抽象到 Shape 类中：网格上的位置、旋转角度以及区域和外围属性。 它们可以重写，但位置除外，不能更改各个形状的行为。

旋转方法可以重写，就像在 Circle 类中一样，因为它是对称的，所以旋转固定。 因此在 Circle 类中，旋转方法替换为不执行任何操作的方法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2901.fs)]

**输出：**

```console
Perimeter of square with side length 10.000000 is 40.000000
Circumference of circle with radius 5.000000 is 31.415927
Area of Square: 100.000000
Area of Circle: 78.539816
```

## <a name="see-also"></a>请参阅

- [类](classes.md)
- [成员](./members/index.md)
- [方法](./members/methods.md)
- [属性](./members/Properties.md)
