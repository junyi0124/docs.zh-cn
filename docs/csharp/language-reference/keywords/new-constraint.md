---
description: new 约束 - C# 参考
title: new 约束 - C# 参考
ms.date: 07/20/2015
helpviewer_keywords:
- new constraint keyword [C#]
ms.assetid: 58850b64-cb97-4136-be50-1f3bc7fc1da9
ms.openlocfilehash: f7b097729fe973aba7b85c48e1b1033aade83080
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89139445"
---
# <a name="new-constraint-c-reference"></a>new 约束（C# 参考）

`new` 约束指定泛型类声明中的类型实参必须有公共的无参数构造函数。 若要使用 `new` 约束，则该类型不能为抽象类型。

当泛型类创建类型的新实例时，请将 `new` 约束应用于类型参数，如下面的示例所示：

[!code-csharp[csrefKeywordsOperator#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#5)]

当与其他约束一起使用时，`new()` 约束必须最后指定：

[!code-csharp[csrefKeywordsOperator#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#6)]

有关详细信息，请参阅[类型参数的约束](../../programming-guide/generics/constraints-on-type-parameters.md)。

`new` 关键字还可用于[创建类型的实例](../operators/new-operator.md)或用作[成员声明修饰符](new-modifier.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[类型参数约束](~/_csharplang/spec/classes.md#type-parameter-constraints)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [泛型](../../programming-guide/generics/index.md)
