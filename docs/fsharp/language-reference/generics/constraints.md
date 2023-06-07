---
title: 约束
description: 了解适用F#于泛型类型参数的约束，以指定泛型类型或函数中类型参数的要求。
ms.date: 05/16/2016
ms.openlocfilehash: 70a8bec1ad67d7e814cb7a96b1876bb22399c5e7
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425017"
---
# <a name="constraints"></a>约束

本主题介绍可应用于泛型类型参数以指定泛型类型或函数中类型参数的要求的约束。

## <a name="syntax"></a>语法

```fsharp
type-parameter-list when constraint1 [ and constraint2]
```

## <a name="remarks"></a>备注

可以应用多个不同的约束，以限制可在泛型类型中使用的类型。 下表列出并描述了这些约束。

|约束|语法|描述|
|----------|------|-----------|
|类型约束|*类型参数*：&gt;*类型*|提供的类型必须等于或派生自指定的类型，或者，如果类型是接口，则提供的类型必须实现接口。|
|Null 约束|*类型-参数*： null|提供的类型必须支持 null 文本。 这包括所有 .NET 对象类型，但F#不包括列表、元组、函数、类、记录或联合类型。|
|显式成员约束|[（]*类型-参数*[或 .。。或*类型参数*）]：（*成员签名*）|提供的类型自变量中至少有一个必须具有具有指定签名的成员;不用于常见用途。 成员必须在类型或隐式类型扩展的一部分上显式定义为显式成员约束的有效目标。|
|构造函数约束|*类型-参数*：（新的：单元&gt; "a"）|提供的类型必须具有无参数的构造函数。|
|值类型约束|： struct|提供的类型必须是 .NET 值类型。|
|引用类型约束|： not struct|提供的类型必须是 .NET 引用类型。|
|枚举类型约束|：枚举&lt;*基础类型*&gt;|提供的类型必须是具有指定基础类型的枚举类型;不用于常见用途。|
|委托约束|：委托&lt;*元组-参数类型*、*返回类型*&gt;|提供的类型必须是具有指定参数和返回值的委托类型;不用于常见用途。|
|比较约束|：比较|提供的类型必须支持比较。|
|相等约束|：相等|提供的类型必须支持相等性。|
|非托管约束|：非托管|提供的类型必须是非托管类型。 非托管类型是特定的基元类型（`sbyte`、`byte`、`char`、`nativeint`、`unativeint`、`float32`、`float`、`int16`、`uint16`、`int32`、`uint32`、`int64`、`uint64`或 `decimal`）、枚举类型、`nativeptr<_>`或非泛型结构，其字段为所有非托管类型。|

当你的代码必须使用在约束类型上可用的功能，而不是常规类型时，你必须添加约束。 例如，如果使用类型约束指定类类型，则可以在泛型函数或类型中使用该类的任意一个方法。

在显式编写类型参数时，有时需要指定约束，因为如果没有约束，编译器将无法验证你所使用的功能是否可用于在运行时为类型提供的任何类型参数.

在代码中F#使用的最常见约束是指定基类或接口的类型约束。 F#库使用其他约束来实现某些功能，如显式成员约束（用于实现算术运算符的运算符重载），主要是因为F#支持公共语言运行时支持的一组完整的约束。

在类型推理过程中，编译器会自动推断某些约束。 例如，如果在函数中使用 `+` 运算符，则编译器将对表达式中使用的变量类型推断显式成员约束。

下面的代码演示了一些约束声明：

```fsharp
// Base Type Constraint
type Class1<'T when 'T :> System.Exception> =
class end

// Interface Type Constraint
type Class2<'T when 'T :> System.IComparable> =
class end

// Null constraint
type Class3<'T when 'T : null> =
class end

// Member constraint with instance member
type Class5<'T when 'T : (member Method1 : 'T -> int)> =
class end

// Member constraint with property
type Class6<'T when 'T : (member Property1 : int)> =
class end

// Constructor constraint
type Class7<'T when 'T : (new : unit -> 'T)>() =
member val Field = new 'T()

// Reference type constraint
type Class8<'T when 'T : not struct> =
class end

// Enumeration constraint with underlying value specified
type Class9<'T when 'T : enum<uint32>> =
class end

// 'T must implement IComparable, or be an array type with comparable
// elements, or be System.IntPtr or System.UIntPtr. Also, 'T must not have
// the NoComparison attribute.
type Class10<'T when 'T : comparison> =
class end

// 'T must support equality. This is true for any type that does not
// have the NoEquality attribute.
type Class11<'T when 'T : equality> =
class end

type Class12<'T when 'T : delegate<obj * System.EventArgs, unit>> =
class end

type Class13<'T when 'T : unmanaged> =
class end

// Member constraints with two type parameters
// Most often used with static type parameters in inline functions
let inline add(value1 : ^T when ^T : (static member (+) : ^T * ^T -> ^T), value2: ^T) =
value1 + value2

// ^T and ^U must support operator +
let inline heterogenousAdd(value1 : ^T when (^T or ^U) : (static member (+) : ^T * ^U -> ^T), value2 : ^U) =
value1 + value2

// If there are multiple constraints, use the and keyword to separate them.
type Class14<'T,'U when 'T : equality and 'U : equality> =
class end
```

## <a name="see-also"></a>请参阅

- [泛型](index.md)
- [约束](constraints.md)
