---
title: 派生类无法引发基类事件
ms.date: 07/20/2015
f1_keywords:
- vbc30029
- bc30029
helpviewer_keywords:
- BC30029
ms.assetid: 63afa1c6-2f93-4512-a2f0-372455979771
ms.openlocfilehash: 7b86420466d77a6aa45b1201a9375b4433e4b5ec
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163436"
---
# <a name="bc30029-derived-classes-cannot-raise-base-class-events"></a>BC30029：派生类无法引发基类事件

事件只能从声明它的声明空间引发。 因此，类无法从任何其他类（甚至是从中派生的类）引发事件。

 **错误 ID：** BC30029

## <a name="to-correct-this-error"></a>更正此错误

- 移动 `Event` 语句或 `RaiseEvent` 语句，使其位于同一个类中。

## <a name="see-also"></a>另请参阅

- [Event 语句](../statements/event-statement.md)
- [RaiseEvent 语句](../statements/raiseevent-statement.md)
