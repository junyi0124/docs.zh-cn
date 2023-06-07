---
title: 类型“type1”的值无法转换为“type2”
ms.date: 07/20/2015
f1_keywords:
- vbc31194
- bc31194
helpviewer_keywords:
- BC31194
ms.assetid: 03d50c31-addd-4c90-9c53-725b84f9782e
ms.openlocfilehash: 107936aa969690d0cc9fd4a2605cfceea31eeca8
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161408"
---
# <a name="bc31194-value-of-type-type1-cannot-be-converted-to-type2"></a>BC31194：类型 "type1" 的值无法转换为 "type2"

类型 "type1" 的值无法转换为 "type2"。 您可以使用 "Value" 属性来获取 "" 的第一个元素的字符串值 \<parentElement> 。

 已尝试将 XML 文本隐式强制转换为特定类型。 XML 文本不能隐式强制转换为指定类型。

 **错误 ID：** BC31194

## <a name="to-correct-this-error"></a>更正此错误

- 使用 XML 文本的 `Value` 属性来将其值作为 `String`引用。 使用 `CType` 函数、另一个类型转换函数，或 <xref:System.Convert> 类将值强制转换为指定类型。

## <a name="see-also"></a>另请参阅

- <xref:System.Convert>
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [XML 文本](../xml-literals/index.md)
- [XML](../../programming-guide/language-features/xml/index.md)
