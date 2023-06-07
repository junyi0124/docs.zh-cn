---
title: 需要“Optional”
ms.date: 07/20/2015
f1_keywords:
- bc30202
- vbc30202
helpviewer_keywords:
- BC30202
ms.assetid: 6f75060c-2db4-4a79-b5d1-5780c09a74cd
ms.openlocfilehash: 9c717cef2052722563e04595ef7a808ea103a75d
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92159815"
---
# <a name="bc30202-optional-expected"></a>BC30202：应为 "Optional"

过程声明中的可选参数后跟必需的参数。 可选参数后面的每个参数都必须是可选的。

 **错误 ID：** BC30202

## <a name="to-correct-this-error"></a>更正此错误

- 如果需要参数，请将其移动到自变量列表中的第一个可选参数之前。

- 如果参数旨在作为可选参数，请使用 `Optional` 关键字。

## <a name="see-also"></a>另请参阅

- [可选参数](../../programming-guide/language-features/procedures/optional-parameters.md)
