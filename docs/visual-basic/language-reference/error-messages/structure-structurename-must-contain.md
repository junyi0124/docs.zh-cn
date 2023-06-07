---
title: 结构“<structurename>”至少必须包含一个实例成员变量或至少必须包含一个未标记为“Custom”的实例事件声明
ms.date: 07/20/2015
f1_keywords:
- bc30941
- vbc30941
helpviewer_keywords:
- BC30941
ms.assetid: 7054cc1e-bac3-4c3d-82f3-35772bd8dd3b
ms.openlocfilehash: 4e7ef82659c43be08ee444eaf3f4df663f7aaa53
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92159646"
---
# <a name="bc30941-structure-structurename-must-contain-at-least-one-instance-member-variable-or-at-least-one-instance-event-declaration-not-marked-custom"></a>BC30941：结构 " \<structurename> " 必须包含至少一个实例成员变量或至少包含一个未标记为 "Custom" 的实例事件声明

结构定义不包括任何非共享变量或非共享的非自定义事件。

 每个结构都必须有一个适用于每个特定实例的变量或事件， (非共享的) ，而不是共同 ([共享](../modifiers/shared.md)) 中的所有实例。 非共享常量、属性和过程不满足此要求。 此外，如果没有非共享变量并且只有一个非共享事件，则该事件不能是 `Custom` 事件。

 **错误 ID：** BC30941

## <a name="to-correct-this-error"></a>更正此错误

- 定义至少一个不是的变量或事件 `Shared` 。 如果只定义了一个事件，则它必须是非自定义以及非共享的。

## <a name="see-also"></a>另请参阅

- [结构](../../programming-guide/language-features/data-types/structures.md)
- [如何：声明结构](../../programming-guide/language-features/data-types/how-to-declare-a-structure.md)
- [Structure 语句](../statements/structure-statement.md)
