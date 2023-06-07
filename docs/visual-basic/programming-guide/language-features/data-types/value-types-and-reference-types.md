---
title: Value Types and Reference Types
ms.date: 07/20/2015
helpviewer_keywords:
- reference data types [Visual Basic]
- reference types [Visual Basic]
- value types [Visual Basic]
- value data types [Visual Basic]
- types [Visual Basic]
- data types [Visual Basic], value types
- data types [Visual Basic], reference types
ms.assetid: fc82ce15-5a40-4c5c-a1e1-a556830e7391
ms.openlocfilehash: 72cb1455300e1ff00d9d558aa5a9df95f32aa7b0
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91090113"
---
# <a name="value-types-and-reference-types"></a>Value Types and Reference Types

Visual Basic 中有两种类型：引用类型和值类型。 引用类型的变量存储对其数据（对象）的引用，而值类型的变量直接包含其数据。 对于引用类型，两种变量可引用同一对象；因此，对一个变量执行的操作会影响另一个变量所引用的对象。 对于值类型，每个变量都有自己的数据副本，对一个变量执行的操作不可能影响另一个 (但对于参数) 的 [ByRef 修饰符](../../../language-reference/modifiers/byref.md) 除外。
  
## <a name="value-types"></a>值类型  

 如果数据类型在其自身的内存分配中包含数据，则该数据类型为 *值类型* 。 值类型包括：  
  
- 所有数值数据类型  
  
- `Boolean`、`Char` 和 `Date`  
  
- 所有结构（即使它们的成员是引用类型）  
  
- 枚举，因为它们的基础类型始终为、、、 `SByte` `Short` `Integer` `Long` 、 `Byte` 、、 `UShort` `UInteger` 或 `ULong`  
  
 每个结构都是值类型，即使它包含引用类型成员。 出于此原因，值类型（如 `Char` 和） `Integer` 是由 .NET Framework 结构实现的。  
  
 您可以使用保留的关键字（例如）声明值类型 `Decimal` 。 还可以使用 `New` 关键字初始化值类型。 如果类型具有采用参数的构造函数，则此方法特别有用。 这是构造函数的一个示例 <xref:System.Decimal.%23ctor%28System.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Boolean%2CSystem.Byte%29> ，它 `Decimal` 从提供的部分生成一个新值。  
  
## <a name="reference-types"></a>引用类型  

 *引用类型*存储对其数据的引用。 引用类型包括：  
  
- `String`  
  
- 所有数组（即使它们的元素为值类型）  
  
- 类类型，如 <xref:System.Windows.Forms.Form>  
  
- 委托  
  
 类是 *引用类型*。 请注意，每个数组都是引用类型，即使其成员是值类型也是如此。  
  
 由于每个引用类型都表示一个基础 .NET Framework 类，因此必须在初始化时使用 [New 运算符](../../../language-reference/operators/new-operator.md) 关键字。 下面的语句初始化一个数组。  
  
```vb  
Dim totals() As Single = New Single(8) {}  
```  
  
## <a name="elements-that-are-not-types"></a>不是类型的元素  

 下面的编程元素不限定为类型，因为您不能将任何这些元素指定为已声明元素的数据类型：  
  
- 命名空间  
  
- 模块  
  
- 事件  
  
- 属性和过程  
  
- 变量、常量和字段  
  
## <a name="working-with-the-object-data-type"></a>使用 Object 数据类型  

 可以将引用类型或值类型分配给 `Object` 数据类型的变量。 `Object`变量始终保存对数据的引用，而不是数据本身。 但是，如果将值类型分配给 `Object` 变量，则其行为就像它包含自己的数据一样。 有关详细信息，请参阅 [Object Data Type](../../../language-reference/data-types/object-data-type.md)。  
  
 可以 `Object` 通过将变量传递给 <xref:Microsoft.VisualBasic.Information.IsReference%2A> <xref:Microsoft.VisualBasic.Information> 命名空间类中的方法来确定变量是作为引用类型还是值类型 <xref:Microsoft.VisualBasic?displayProperty=nameWithType> 。 <xref:Microsoft.VisualBasic.Information.IsReference%2A?displayProperty=nameWithType>`True`如果该变量的内容 `Object` 表示一个引用类型，则返回。  
  
## <a name="see-also"></a>请参阅

- [可以为 null 的值类型](nullable-value-types.md)
- [Visual Basic 中的类型转换](type-conversions.md)
- [Structure 语句](../../../language-reference/statements/structure-statement.md)
- [有效使用数据类型](efficient-use-of-data-types.md)
- [Object Data Type](../../../language-reference/data-types/object-data-type.md)
- [数据类型](index.md)
