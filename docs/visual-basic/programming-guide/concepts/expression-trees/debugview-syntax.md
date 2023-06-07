---
title: DebugView 属性使用的语法
description: 描述 DebugView 属性使用的特殊语法以生成表达式数的字符串表示形式
author: zspitz
ms.author: wiwagn
ms.date: 05/22/2019
ms.topic: reference
helpviewer_keywords:
- expression trees
- debugview
ms.openlocfilehash: 98ceba37aa226fab68ae1c1028e2a1139b3b8e7e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346870"
---
# <a name="debugview-syntax"></a>`DebugView` 语法

`DebugView` 属性（仅在调试时可用）提供表达式树的字符串呈现。 大部分语法都相当容易理解；特殊情况将在以下部分中介绍。

每个示例后跟包含 `DebugView`的注释块。

## <a name="parameterexpression"></a>ParameterExpression

<xref:System.Linq.Expressions.ParameterExpression?displayProperty=nameWithType> 变量名称的开头显示有“$”符号。

如果参数没有名称，则会为其分配一个自动生成的名称，例如 `$var1` 或 `$var2`。

### <a name="examples"></a>示例

```vb
Dim numParam As ParameterExpression = Expression.Parameter(GetType(Integer), "num")
'
'    $num
'

Dim numParam As ParameterExpression = Expression.Parameter(GetType(Integer))
'
'    $var1
'
```

## <a name="constantexpressions"></a>ConstantExpressions

对于表示整数值、字符串和 <xref:System.Linq.Expressions.ConstantExpression?displayProperty=nameWithType> 的 `null` 对象，将显示常数的值。

对于某些数值类型，会将一个后缀添加到值：

| 类型 | 关键字 | Suffix |
|--|--|--|
| <xref:System.UInt32> | [UInteger](../../../language-reference/data-types/uinteger-data-type.md) | U |
| <xref:System.Int64> | [Long](../../../language-reference/data-types/long-data-type.md) | L |
| <xref:System.UInt64> | [ULong](../../../language-reference/data-types/ulong-data-type.md) | UL |
| <xref:System.Double> | [双精度](../../../language-reference/data-types/double-data-type.md) | D |
| <xref:System.Single> | [单精度](../../../language-reference/data-types/single-data-type.md) | F |
| <xref:System.Decimal> | [小数](../../../language-reference/data-types/decimal-data-type.md) | M |

### <a name="examples"></a>示例

```vb
Dim num as Integer = 10
Dim expr As ConstantExpression = Expression.Constant(num)
'
'    10
'

Dim num As Double = 10
Dim expr As ConstantExpression = Expression.Constant(num)
'
'    10D
'
```

## <a name="blockexpression"></a>BlockExpression

如果 <xref:System.Linq.Expressions.BlockExpression?displayProperty=nameWithType> 对象的类型与块中最后一个表达式的类型不同，则该类型将显示在尖括号（`<` 和 `>`）内。 否则，将不显示 <xref:System.Linq.Expressions.BlockExpression> 对象的类型。

### <a name="examples"></a>示例

```vb
Dim block As BlockExpression = Expression.Block(Expression.Constant("test"))
'
'    .Block() {
'        "test"
'    }
'

Dim block As BlockExpression = Expression.Block(
    GetType(Object),
    Expression.Constant("test")
)
'
'    .Block<System.Object>() {
'        "test"
'    }
'
```

## <a name="lambdaexpression"></a>LambdaExpression

显示 <xref:System.Linq.Expressions.LambdaExpression?displayProperty=nameWithType> 对象及其委托类型。

如果 lambda 表达式没有名称，则会为其分配一个自动生成的名称，例如 `#Lambda1` 或 `#Lambda2`。

### <a name="examples"></a>示例

```vb
Dim lambda As LambdaExpression = Expression.Lambda(Of Func(Of Integer))(
    Expression.Constant(1)
)
'
'    .Lambda #Lambda1<System.Func'1[System.Int32]>() {
'        1
'    }
'

Dim lambda As LambdaExpression = Expression.Lambda(Of Func(Of Integer))(
    Expression.Constant(1),
    "SampleLambda",
    Nothing
)
'
'    .Lambda #SampleLambda<System.Func'1[System.Int32]>() {
'        1
'    }
'
```

## <a name="labelexpression"></a>LabelExpression

如果指定 <xref:System.Linq.Expressions.LabelExpression?displayProperty=nameWithType> 对象的默认值，则在 <xref:System.Linq.Expressions.LabelTarget?displayProperty=nameWithType> 对象之前显示此值。

`.Label` 令牌指示标签的开头。 `.LabelTarget` 令牌指示要跳转到的目标的目的地。

如果标签没有名称，则会为其分配一个自动生成的名称，例如 `#Label1` 或 `#Label2`。

### <a name="examples"></a>示例

```vb
Dim target As LabelTarget = Expression.Label(GetType(Integer), "SampleLabel")
Dim label1 As BlockExpression = Expression.Block(
    Expression.Goto(target, Expression.Constant(0)),
    Expression.Label(target, Expression.Constant(-1))
)
'
'    .Block() {
'        .Goto SampleLabel { 0 };
'        .Label
'            -1
'        .LabelTarget SampleLabel:
'    }
'

Dim target As LabelTarget = Expression.Label()
Dim block As BlockExpression = Expression.Block(
    Expression.Goto(target),
    Expression.Label(target)
)
'
'    .Block() {
'        .Goto #Label1 { };
'        .Label
'        .LabelTarget #Label1:
'    }
'
```

## <a name="checked-operators"></a>Checked 运算符

Checked 运算符在运算符前面显示 `#` 符号。 例如，checked 加号显示为 `#+`。

### <a name="examples"></a>示例

```vb
Dim expr As Expression = Expression.AddChecked(
    Expression.Constant(1),
    Expression.Constant(2)
)
'
'     1 #+ 2
'

Dim expr As Expression = Expression.ConvertChecked(
    Expression.Constant(10.0),
    GetType(Integer)
)
'
'    #(System.Int32)10D
'
```
