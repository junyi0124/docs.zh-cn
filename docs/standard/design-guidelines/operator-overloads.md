---
title: 运算符重载
ms.date: 10/22/2008
helpviewer_keywords:
- operators [.NET Framework], overloads
- names [.NET Framework], overloaded operators
- member design guidelines, operators
- overloaded operators
ms.assetid: 37585bf2-4c27-4dee-849a-af70e3338cc1
ms.openlocfilehash: 7757e6cfdb9ae31f6e94c26a2684f8176174499a
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734811"
---
# <a name="operator-overloads"></a>运算符重载

运算符重载允许框架类型看起来像是内置语言基元。

 尽管在某些情况下允许和有用，但应慎重使用运算符重载。 在许多情况下，运算符重载已经被滥用，例如，当框架设计器开始对应该为简单方法的操作使用运算符时。 以下准则可帮助您决定何时以及如何使用运算符重载。

 ❌ 避免定义运算符重载，但在应像基元 (内置) 类型的类型中除外。

 ✔️考虑在应感觉为基元类型的类型中定义运算符重载。

 例如， <xref:System.String?displayProperty=nameWithType> 具有 `operator==` 并定义了 `operator!=` 。

 ✔️在表示数字 (例如) 的结构中定义运算符重载 <xref:System.Decimal?displayProperty=nameWithType> 。

 ❌ 在定义运算符重载时不太刻意。

 运算符重载非常有用，在这种情况下，操作的结果将是什么。 例如，可以 <xref:System.DateTime> 从一个中减去另一个 `DateTime` 并获取 <xref:System.TimeSpan> 。 但是，不适合使用逻辑联合运算符来联合两个数据库查询，或使用 shift 运算符将写入到流中。

 ❌ 除非至少有一个操作数为定义重载的类型，否则不要提供运算符重载。

 ✔️以对称方式重载运算符。

 例如，如果你重载 `operator==` ，则还应重载 `operator!=` 。 同样，如果你重载 `operator<` ，则还应重载 `operator>` ，依此类推。

 ✔️考虑为具有对应于每个重载运算符的友好名称提供方法。

 许多语言不支持运算符重载。 出于此原因，建议重载运算符的类型包含辅助方法，该方法具有提供等效功能的适当的特定于域的名称。

 下表包含一个运算符列表和相应的友好方法名称。

|C # 运算符符号|元数据名称|友好名称|
|-------------------------|-------------------|-------------------|
|`N/A`|`op_Implicit`|`To<TypeName>/From<TypeName>`|
|`N/A`|`op_Explicit`|`To<TypeName>/From<TypeName>`|
|`+ (binary)`|`op_Addition`|`Add`|
|`- (binary)`|`op_Subtraction`|`Subtract`|
|`* (binary)`|`op_Multiply`|`Multiply`|
|`/`|`op_Division`|`Divide`|
|`%`|`op_Modulus`|`Mod or Remainder`|
|`^`|`op_ExclusiveOr`|`Xor`|
|`& (binary)`|`op_BitwiseAnd`|`BitwiseAnd`|
|<code>&#124;</code>|`op_BitwiseOr`|`BitwiseOr`|
|`&&`|`op_LogicalAnd`|`And`|
|<code>&#124;&#124;</code>|`op_LogicalOr`|`Or`|
|`=`|`op_Assign`|`Assign`|
|`<<`|`op_LeftShift`|`LeftShift`|
|`>>`|`op_RightShift`|`RightShift`|
|`N/A`|`op_SignedRightShift`|`SignedRightShift`|
|`N/A`|`op_UnsignedRightShift`|`UnsignedRightShift`|
|`==`|`op_Equality`|`Equals`|
|`!=`|`op_Inequality`|`Equals`|
|`>`|`op_GreaterThan`|`CompareTo`|
|`<`|`op_LessThan`|`CompareTo`|
|`>=`|`op_GreaterThanOrEqual`|`CompareTo`|
|`<=`|`op_LessThanOrEqual`|`CompareTo`|
|`*=`|`op_MultiplicationAssignment`|`Multiply`|
|`-=`|`op_SubtractionAssignment`|`Subtract`|
|`^=`|`op_ExclusiveOrAssignment`|`Xor`|
|`<<=`|`op_LeftShiftAssignment`|`LeftShift`|
|`%=`|`op_ModulusAssignment`|`Mod`|
|`+=`|`op_AdditionAssignment`|`Add`|
|`&=`|`op_BitwiseAndAssignment`|`BitwiseAnd`|
|<code>&#124;=</code>|`op_BitwiseOrAssignment`|`BitwiseOr`|
|`,`|`op_Comma`|`Comma`|
|`/=`|`op_DivisionAssignment`|`Divide`|
|`--`|`op_Decrement`|`Decrement`|
|`++`|`op_Increment`|`Increment`|
|`- (unary)`|`op_UnaryNegation`|`Negate`|
|`+ (unary)`|`op_UnaryPlus`|`Plus`|
|`~`|`op_OnesComplement`|`OnesComplement`|

### <a name="overloading-operator-"></a>重载运算符 = =

 重载 `operator ==` 非常复杂。 运算符的语义需要与其他一些成员（如）兼容 <xref:System.Object.Equals%2A?displayProperty=nameWithType> 。

### <a name="conversion-operators"></a>转换运算符

 转换运算符是允许从一种类型转换为另一种类型的一元运算符。 运算符必须在操作数或返回类型上定义为静态成员。 有两种类型的转换运算符：隐式和显式。

 ❌ 如果最终用户不需要此类转换，请不要提供转换运算符。

 ❌ 不要定义类型的域之外的转换运算符。

 例如，、 <xref:System.Int32> <xref:System.Double> 和 <xref:System.Decimal> 都是数值类型，而 <xref:System.DateTime> 不是。 因此，不应将转换运算符转换为 `Double(long)` `DateTime` 。 在这种情况下，构造函数是首选的。

 ❌ 如果转换可能有损，请不要提供隐式转换运算符。

 例如，不应从到的隐式转换， `Double` `Int32` 因为的 `Double` 范围超出了 `Int32` 。 即使转换可能会有损，也可以提供显式转换运算符。

 ❌ 不从隐式强制转换引发异常。

 最终用户很难了解发生的情况，因为他们可能不知道转换正在进行。

 <xref:System.InvalidCastException?displayProperty=nameWithType>如果调用强制转换运算符导致了有损转换，并且该运算符的协定不允许有损转换，✔️将引发。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [成员设计准则](member.md)
- [框架设计准则](index.md)
