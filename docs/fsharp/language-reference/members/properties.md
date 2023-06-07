---
title: 属性
description: '了解 F # 属性，这些属性是表示与对象关联的值的成员。'
ms.date: 05/16/2016
ms.openlocfilehash: a2a4fbfc88831dcb5cad7a2da701969b2e98b2e3
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740193"
---
# <a name="properties"></a>属性

*属性* 是表示与对象关联的值的成员。

## <a name="syntax"></a>语法

```fsharp
// Property that has both get and set defined.
[ attributes ]
[ static ] member [accessibility-modifier] [self-identifier.]PropertyName
with [accessibility-modifier] get() =
    get-function-body
and [accessibility-modifier] set parameter =
    set-function-body

// Alternative syntax for a property that has get and set.
[ attributes-for-get ]
[ static ] member [accessibility-modifier-for-get] [self-identifier.]PropertyName =
    get-function-body
[ attributes-for-set ]
[ static ] member [accessibility-modifier-for-set] [self-identifier.]PropertyName
with set parameter =
    set-function-body

// Property that has get only.
[ attributes ]
[ static ] member [accessibility-modifier] [self-identifier.]PropertyName =
    get-function-body

// Alternative syntax for property that has get only.
[ attributes ]
[ static ] member [accessibility-modifier] [self-identifier.]PropertyName
with get() =
    get-function-body

// Property that has set only.
[ attributes ]
[ static ] member [accessibility-modifier] [self-identifier.]PropertyName
with set parameter =
    set-function-body

// Automatically implemented properties.
[ attributes ]
[ static ] member val [accessibility-modifier] PropertyName = initialization-expression [ with get, set ]
```

## <a name="remarks"></a>备注

属性表示在面向对象的编程中的 "具有" 关系，它表示与对象实例相关联的数据，对于静态属性，则表示与类型相关联的数据。

可以通过两种方式声明属性，具体取决于是否要显式指定基础值 (也称为属性的后备存储) ，或者是否允许编译器自动生成后备存储。 通常，如果属性具有非普通实现并且属性只是值或变量的简单包装，则应使用更明确的方式。 若要显式声明属性，请使用 `member` 关键字。 此声明性语法后跟指定 `get` 和 `set` 方法（也称为 " *访问器*"）的语法。 "语法" 部分中显示的显式语法的各种形式用于读/写、只读和只写属性。 对于只读属性，只定义一个 `get` 方法; 对于只写属性，只定义一个 `set` 方法。 请注意，当属性具有 `get` 和 `set` 访问器时，可以使用替代语法来指定每个访问器不同的属性和可访问性修饰符，如以下代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3201.fs)]

对于具有和方法的读/写属性， `get` `set` 和的顺序 `get` `set` 可以反转。 或者，你可以提供仅为提供的语法 `get` 和仅显示的语法， `set` 而不是使用组合语法。 这样做可以更轻松地注释单个 `get` 或 `set` 方法，前提是您可能需要执行此操作。 下面的代码演示了使用组合语法的替代方法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3203.fs)]

保存属性的数据的私有值称为 " *后备存储*"。 若要让编译器自动创建后备存储，请使用关键字 `member val` ，省略自标识符，然后提供一个表达式来初始化属性。 如果该属性是可变的，则包括 `with get, set` 。 例如，下面的类类型包括两个自动实现的属性。 `Property1` 为只读，并初始化为提供给主构造函数的自变量，并且 `Property2` 是一个初始化为空字符串的可设置属性：

```fsharp
type MyClass(property1 : int) =
member val Property1 = property1
member val Property2 = "" with get, set
```

自动实现的属性是类型初始化的一部分，因此必须在任何其他成员定义之前包含它们，就像 `let` `do` 类型定义中的绑定和绑定一样。 请注意，仅在初始化时计算初始化自动实现的属性的表达式，而不会在每次访问该属性时进行计算。 此行为与显式实现的属性的行为相反。 这实际上意味着将用于初始化这些属性的代码添加到类的构造函数中。 请考虑以下显示这种差异的代码：

```fsharp
type MyClass() =
    let random  = new System.Random()
    member val AutoProperty = random.Next() with get, set
    member this.ExplicitProperty = random.Next()

let class1 = new MyClass()

printfn $"class1.AutoProperty = %d{class1.AutoProperty}"
printfn $"class1.ExplicitProperty = %d{class1.ExplicitProperty}"
```

**输出**

```console
class1.AutoProperty = 1853799794
class1.AutoProperty = 1853799794
class1.ExplicitProperty = 978922705
class1.ExplicitProperty = 1131210765
```

上述代码的输出显示，AutoProperty 的值在被重复调用时不会更改，而 ExplicitProperty 会在每次调用时更改。 这说明，每次都不会计算自动实现属性的表达式，这与显式属性的 getter 方法相同。

>[!WARNING]
>某些库（如实体框架 (`System.Data.Entity`) 在基类构造函数中执行自定义操作，这些操作在初始化自动实现的属性时无法正常工作。 在这些情况下，请尝试使用显式属性。

属性可以是类、结构、可区分联合、记录、接口和类型扩展的成员，也可以在对象表达式中定义。

特性可应用到属性。 若要将特性应用于属性，请在属性之前的单独行上写入特性。 有关更多信息，请参阅[特性](../attributes.md)。

默认情况下，属性是公共的。 辅助功能修饰符还可以应用于属性。 若要应用可访问性修饰符，请在属性名称之前立即添加它（如果它要应用于 `get` 和方法） `set` ; `get` `set` 如果每个访问器需要不同的可访问性，请将其添加到和关键字之前。 可 *访问性修饰符* 可以是下列其中一项： `public` 、 `private` 、 `internal` 。 有关详细信息，请参阅[访问控制](../access-control.md)。

每次访问属性时，都将执行属性实现。

## <a name="static-and-instance-properties"></a>静态属性和实例属性

属性可以为静态或实例属性。 可以在没有实例的情况下调用静态属性，并将其用于与类型相关联的值，而不是与单个对象一起使用。 对于静态属性，请省略自标识符。 自标识符对于实例属性是必需的。

下面的静态属性定义基于某个方案，在此方案中，你有一个作为 `myStaticValue` 属性的后备存储的静态字段。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3204.fs)]

属性也可以是类似数组，在这种情况下，它们被称为 *索引属性*。 有关详细信息，请参阅 [索引属性](indexed-properties.md)。

## <a name="type-annotation-for-properties"></a>属性的类型批注

在许多情况下，编译器提供的信息足以从后备存储的类型推断属性的类型，但您可以通过添加类型注释来显式设置类型。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3205.fs)]

## <a name="using-property-set-accessors"></a>使用属性集访问器

您可以使用运算符设置提供 `set` 访问器的属性 `<-` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3206.fs)]

输出为 **20**。

## <a name="abstract-properties"></a>抽象属性

属性可以是抽象的。 与方法一样， `abstract` 只是表示存在与属性关联的虚拟调度。 抽象属性可以是真正抽象的，也就是说，在同一类中不使用定义。 因此，包含此类属性的类是一个抽象类。 另外，抽象可以只是表示属性是虚拟的，在这种情况下，定义必须存在于同一个类中。 请注意，抽象属性不能是私有的，如果一个访问器是抽象的，另一个访问器也必须是抽象的。 有关抽象类的详细信息，请参阅 [抽象类](../abstract-classes.md)。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3207.fs)]

## <a name="see-also"></a>请参阅

- [成员](index.md)
- [方法](methods.md)
