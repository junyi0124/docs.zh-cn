---
title: 未能从委托中推理类型参数
ms.date: 07/20/2015
f1_keywords:
- bc36564
- vbc36564
helpviewer_keywords:
- BC36564
ms.assetid: 21312807-e1cd-4ac1-ae1c-c28a9c25164d
ms.openlocfilehash: f7937a34ab425da684f892250884d21e020e4c57
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161239"
---
# <a name="bc36564-type-arguments-could-not-be-inferred-from-the-delegate"></a>BC36564：无法从委托推断类型参数

赋值语句使用 `AddressOf` 将泛型过程的地址赋给委托，但它不会为泛型过程提供任何类型参数。

 通常，在调用某个泛型类型时，要为该泛型类型定义的每个类型形参提供一个类型实参。 如果未提供任何类型实参，编译器将尝试推断要传递给类型形参的类型。 如果上下文未提供充足的信息供编译器推断类型，则将生成错误。

 **错误 ID：** BC36564

## <a name="to-correct-this-error"></a>更正此错误

- 为 `AddressOf` 表达式中的泛型过程指定类型参数。

## <a name="see-also"></a>另请参阅

- [Generic Types in Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [AddressOf 运算符](../operators/addressof-operator.md)
- [Generic Procedures in Visual Basic](../../programming-guide/language-features/data-types/generic-procedures.md)
- [Type List](../statements/type-list.md)
- [扩展方法](../../programming-guide/language-features/procedures/extension-methods.md)
