---
title: DebugView 属性 (C#) 使用的语法
description: 描述 DebugView 属性使用的特殊语法以生成表达式数的字符串表示形式
author: zspitz
ms.author: wiwagn
ms.date: 05/22/2019
ms.topic: reference
helpviewer_keywords:
- expression trees
- debugview
ms.openlocfilehash: ba695fc808108c49a4eee3c70a305b24c91769d8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "67661718"
---
# <a name="debugview-syntax"></a>`DebugView` 语法

`DebugView` 属性（仅在调试时可用）提供表达式树的字符串呈现。 大部分语法都相当容易理解；特殊情况将在以下部分中介绍。

每个示例都后跟块注释，其中包含 `DebugView`。

## <a name="parameterexpression"></a>ParameterExpression

<xref:System.Linq.Expressions.ParameterExpression?displayProperty=nameWithType> 变量名称的开头显示有 `$` 符号。

如果参数没有名称，则会为其分配一个自动生成的名称，例如 `$var1` 或 `$var2`。

### <a name="examples"></a>示例

```csharp
ParameterExpression numParam =  Expression.Parameter(typeof(int), "num");
/*
    $num
*/

ParameterExpression numParam =  Expression.Parameter(typeof(int));
/*
    $var1
*/
```

## <a name="constantexpression"></a>ConstantExpression

对于表示整数值、字符串和 `null` 的 <xref:System.Linq.Expressions.ConstantExpression?displayProperty=nameWithType> 对象，将显示常数的值。

对于使用标准后缀作为 C# 原义字符的数值类型，将后缀添加到值。 下表显示与各种数值类型关联的后缀。

| 类型 | 关键字 | Suffix |
|--|--|--|
| <xref:System.UInt32?displayProperty=nameWithType> | [uint](../../../language-reference/builtin-types/integral-numeric-types.md) | U |
| <xref:System.Int64?displayProperty=nameWithType> | [long](../../../language-reference/builtin-types/integral-numeric-types.md) | L |
| <xref:System.UInt64?displayProperty=nameWithType> | [ulong](../../../language-reference/builtin-types/integral-numeric-types.md) | UL |
| <xref:System.Double?displayProperty=nameWithType> | [双精度](../../../language-reference/builtin-types/floating-point-numeric-types.md) | D |
| <xref:System.Single?displayProperty=nameWithType> | [float](../../../language-reference/builtin-types/floating-point-numeric-types.md) | F |
| <xref:System.Decimal?displayProperty=nameWithType> | [decimal](../../../language-reference/builtin-types/floating-point-numeric-types.md) | M |

### <a name="examples"></a>示例

```csharp
int num = 10;
ConstantExpression expr = Expression.Constant(num);
/*
    10
*/

double num = 10;
ConstantExpression expr = Expression.Constant(num);
/*
    10D
*/
```

## <a name="blockexpression"></a>BlockExpression

如果 <xref:System.Linq.Expressions.BlockExpression?displayProperty=nameWithType> 对象的类型与块中最后一个表达式的类型不同，则该类型将显示在尖括号（`<` 和 `>`）内。 否则，将不显示 <xref:System.Linq.Expressions.BlockExpression> 对象的类型。

### <a name="examples"></a>示例

```csharp
BlockExpression block = Expression.Block(Expression.Constant("test"));
/*
    .Block() {
        "test"
    }
*/

BlockExpression block =  Expression.Block(typeof(Object), Expression.Constant("test"));
/*
    .Block<System.Object>() {
        "test"
    }
*/
```

## <a name="lambdaexpression"></a>LambdaExpression

显示 <xref:System.Linq.Expressions.LambdaExpression?displayProperty=nameWithType> 对象及其委托类型。

如果 lambda 表达式没有名称，则会为其分配一个自动生成的名称，例如 `#Lambda1` 或 `#Lambda2`。

### <a name="examples"></a>示例

```csharp
LambdaExpression lambda =  Expression.Lambda<Func<int>>(Expression.Constant(1));
/*
    .Lambda #Lambda1<System.Func'1[System.Int32]>() {
        1
    }
*/

LambdaExpression lambda =  Expression.Lambda<Func<int>>(Expression.Constant(1), "SampleLambda", null);
/*
    .Lambda #SampleLambda<System.Func'1[System.Int32]>() {
        1
    }
*/
```

## <a name="labelexpression"></a>LabelExpression

如果指定 <xref:System.Linq.Expressions.LabelExpression?displayProperty=nameWithType> 对象的默认值，则在 <xref:System.Linq.Expressions.LabelTarget?displayProperty=nameWithType> 对象之前显示此值。

`.Label` 令牌指示标签的开头。 `.LabelTarget` 令牌指示要跳转到的目标的目的地。

如果标签没有名称，则会为其分配一个自动生成的名称，例如 `#Label1` 或 `#Label2`。

### <a name="examples"></a>示例

```csharp
LabelTarget target = Expression.Label(typeof(int), "SampleLabel");
BlockExpression block = Expression.Block(
    Expression.Goto(target, Expression.Constant(0)),
    Expression.Label(target, Expression.Constant(-1))
);
/*
    .Block() {
        .Goto SampleLabel { 0 };
        .Label
            -1
        .LabelTarget SampleLabel:
    }
*/

LabelTarget target = Expression.Label();
BlockExpression block = Expression.Block(
    Expression.Goto(target),
    Expression.Label(target)
);
/*
    .Block() {
        .Goto #Label1 { };
        .Label
        .LabelTarget #Label1:
    }
*/
```

## <a name="checked-operators"></a>Checked 运算符

Checked 运算符在运算符前面显示 `#` 符号。 例如，checked 加号显示为 `#+`。

### <a name="examples"></a>示例

```csharp
Expression expr = Expression.AddChecked( Expression.Constant(1), Expression.Constant(2));
/*
    1 #+ 2
*/

Expression expr = Expression.ConvertChecked( Expression.Constant(10.0), typeof(int));
/*
    #(System.Int32)10D
*/
```
