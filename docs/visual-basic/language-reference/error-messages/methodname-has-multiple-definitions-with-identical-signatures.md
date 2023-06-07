---
title: “<methodname>”具有多个带有相同签名的定义
ms.date: 07/20/2015
f1_keywords:
- vbc30269
- bc30269
helpviewer_keywords:
- BC30269
ms.assetid: 39489621-6617-4e5c-9b24-c2faf8273891
ms.openlocfilehash: 663b22421d1a0e401cfb3c135c99bd097163a78b
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92160361"
---
# <a name="bc30269-methodname-has-multiple-definitions-with-identical-signatures"></a>BC30269： " \<methodname> " 包含多个具有相同签名的定义

`Function`或 `Sub` 过程声明使用相同的过程名称和参数列表作为前面的声明。 一个可能的原因是尝试重载原始过程。 重载过程必须具有不同的参数列表。

 **错误 ID：** BC30269

## <a name="to-correct-this-error"></a>更正此错误

- 更改过程名称或参数列表，或删除重复的声明。

## <a name="see-also"></a>另请参阅

- [References to Declared Elements](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [重载过程注意事项](../../programming-guide/language-features/procedures/considerations-in-overloading-procedures.md)
