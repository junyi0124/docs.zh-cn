---
title: 如何为类型定义值相等性 - C# 编程指南
description: 了解如何为类型定义值相等性。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- overriding Equals method [C#]
- object equivalence [C#]
- Equals method [C#], overriding
- value equality [C#]
- equivalence [C#]
ms.assetid: 4084581e-b931-498b-9534-cf7ef5b68690
ms.openlocfilehash: 9523ba99f877fde7207042ecb8d28548168a68cb
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162721"
---
# <a name="how-to-define-value-equality-for-a-type-c-programming-guide"></a>如何为类型定义值相等性（C# 编程指南）

定义类或结构时，需确定为类型创建值相等性（或等效性）的自定义定义是否有意义。 通常，类型的对象预期要添加到某类集合时，或者这些对象主要用于存储一组字段或属性时，需实现值相等性。 可以基于类型中所有字段和属性的比较结果来定义值相等性，也可以基于子集进行定义。

在任何一种情况下，类和结构中的实现均应遵循 5 个等效性保证条件（对于以下规则，假设 `x`、`y` 和 `z` 都不为 null）：  
  
1. `x.Equals(x)` 返回 `true`。 这称为自反属性。  
  
2. `x.Equals(y)` 返回与 `y.Equals(x)` 相同的值。 这称为对称属性。  
  
3. 如果 `(x.Equals(y) && y.Equals(z))` 返回 `true`，则 `x.Equals(z)` 返回 `true`。 这称为可传递属性。  
  
4. 只要未修改 x 和 y 引用的对象，`x.Equals(y)` 的连续调用将返回相同的值。  
  
5. 任何非 null 值均不等于 null。 但是，CLR 会在所有方法调用上检查 null，如果 `this` 引用为 null，则会引发 `NullReferenceException`。 因此，当 `x` 为 null 时，`x.Equals(y)` 将引发异常。 这会违反规则 1 或 2，具体取决于 `Equals` 的参数。

定义的任何结构都已具有其从 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法的 <xref:System.ValueType?displayProperty=nameWithType> 替代中继承的值相等性的默认实现。 此实现使用反射来检查类型中的所有字段和属性。 尽管此实现可生成正确的结果，但与专门为类型编写的自定义实现相比，它的速度相对较慢。  
  
类和结构的值相等性的实现详细信息有所不同。 但是，类和结构都需要相同的基础步骤来实现相等性：  
  
1. 替代[虚拟](../../language-reference/keywords/virtual.md) <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法。 大多数情况下，`bool Equals( object obj )` 实现应只调入作为 <xref:System.IEquatable%601?displayProperty=nameWithType> 接口的实现的类型特定 `Equals` 方法。 （请参阅步骤 2。）  
  
2. 通过提供类型特定的 `Equals` 方法实现 <xref:System.IEquatable%601?displayProperty=nameWithType> 接口。 实际的等效性比较将在此接口中执行。 例如，可能决定通过仅比较类型中的一两个字段来定义相等性。 不会从 `Equals` 引发异常。 仅对于类：此方法应仅检查类中声明的字段。 它应调用 `base.Equals` 来检查基类中的字段。 （如果类型直接从 <xref:System.Object> 中继承，则不要这样做，因为 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 的 <xref:System.Object> 实现会执行引用相等性检查。）  
  
3. 可选但建议这样做：重载 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 和 [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 运算符。  
  
4. 替代 <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>，以便具有值相等性的两个对象生成相同的哈希代码。  
  
5. 可选：若要支持“大于”或“小于”定义，请为类型实现 <xref:System.IComparable%601> 接口，并同时重载 [<=](../../language-reference/operators/comparison-operators.md#less-than-or-equal-operator-) 和 [>=](../../language-reference/operators/comparison-operators.md#greater-than-or-equal-operator-) 运算符。  

> [!NOTE]
> 从 C# 9.0 开始，可以使用记录来获取值相等性语义，而不需要任何不必要的样板代码。

## <a name="class-example"></a>类示例

下面的示例演示如何在类（引用类型）中实现值相等性。

[!code-csharp[csProgGuideStatements#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#19)]

在类（引用类型）上，两种 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法的默认实现均执行引用相等性比较，而不是值相等性检查。 实施者替代虚方法时，目的是为其指定值相等性语义。

即使类不重载 `==` 和 `!=` 运算符，也可将这些运算符与类一起使用。 但是，默认行为是执行引用相等性检查。 在类中，如果重载 `Equals` 方法，则应重载 `==` 和 `!=` 运算符，但这并不是必需的。

## <a name="struct-example"></a>结构示例

下面的示例演示如何在结构（值类型）中实现值相等性：

[!code-csharp[csProgGuideStatements#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#20)]
  
对于结构，<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType>（<xref:System.ValueType?displayProperty=nameWithType> 中的替代版本）的默认实现通过使用反射来比较类型中每个字段的值，从而执行值相等性检查。 实施者替代结构中的 `Equals` 虚方法时，目的是提供更高效的方法来执行值相等性检查，并选择根据结构字段或属性的某个子集来进行比较。
  
除非结构显式重载了 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 和 [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 运算符，否则这些运算符无法对结构进行运算。

## <a name="see-also"></a>请参阅

- [相等性比较](equality-comparisons.md)
- [C# 编程指南](../index.md)
