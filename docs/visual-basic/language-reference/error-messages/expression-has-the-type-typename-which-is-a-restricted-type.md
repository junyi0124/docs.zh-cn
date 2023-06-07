---
title: 表达式的类型为“<typename>”，这是受限类型，不能用于访问从“Object”或“ValueType”继承的成员
ms.date: 07/20/2015
f1_keywords:
- bc31393
- vbc31393
helpviewer_keywords:
- BC31393
ms.assetid: 2963cf3f-c527-4aa7-b67c-ee80b6d23186
ms.openlocfilehash: 3e19c0d71ee47ac61e9704f0fcd2637f01aa0896
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163046"
---
# <a name="bc31393-expression-has-the-type-typename-which-is-a-restricted-type-and-cannot-be-used-to-access-members-inherited-from-object-or-valuetype"></a>BC31393： Expression 的类型为 " \<typename> "，这是受限类型，不能用于访问从 "Object" 或 "ValueType" 继承的成员

表达式的计算结果为不能由公共语言运行时 (CLR) 但访问需要装箱的成员的类型。

 *装箱* 是指将一个类型转换为 `Object` ，或有时转换为 <xref:System.ValueType>所需的处理。 公共语言运行时无法对某些结构类型（例如、和）进行 box <xref:System.ArgIterator> <xref:System.RuntimeArgumentHandle> <xref:System.TypedReference> 。

 此表达式尝试使用受限类型调用从或继承的方法 <xref:System.Object> <xref:System.ValueType> ，例如 <xref:System.Object.GetHashCode%2A> 或 <xref:System.Object.ToString%2A> 。 若要访问此方法，Visual Basic 尝试了导致此错误的隐式装箱转换。

 **错误 ID：** BC31393

## <a name="to-correct-this-error"></a>更正此错误

1. 查找计算结果为引用类型的表达式。

2. 查找语句中尝试调用从或继承的方法的部分 <xref:System.Object> <xref:System.ValueType> 。

3. 重写语句以避免方法调用。

## <a name="see-also"></a>另请参阅

- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
