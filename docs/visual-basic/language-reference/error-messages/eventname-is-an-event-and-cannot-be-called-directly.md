---
title: “<eventname>”是事件，不能直接调用
ms.date: 07/20/2015
f1_keywords:
- bc32022
- vbc32022
helpviewer_keywords:
- BC32022
ms.assetid: 4dcfcb8d-a9fa-46a7-a034-29d9ff3a59b3
ms.openlocfilehash: 246cb92daa2c838c95f695542f33cf02af42764d
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161980"
---
# <a name="bc32022-eventname-is-an-event-and-cannot-be-called-directly"></a>BC32022： " \<eventname> " 是事件，不能直接调用

"<`eventname`>" 是一个事件，因此不能直接调用。 使用 `RaiseEvent` 语句引发事件。

 过程调用指定过程名称的事件。 事件处理程序是一个过程，但事件本身就是信号设备，必须引发并处理。

 **错误 ID：** BC32022

## <a name="to-correct-this-error"></a>更正此错误

- 使用 `RaiseEvent` 语句对事件发出信号并调用处理该事件的过程。

## <a name="see-also"></a>另请参阅

- [RaiseEvent 语句](../statements/raiseevent-statement.md)
