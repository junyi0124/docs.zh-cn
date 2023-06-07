---
title: WithEvents
ms.date: 07/20/2015
f1_keywords:
- vb.WithEvents
- WithEvents
helpviewer_keywords:
- WithEvents keyword [Visual Basic]
ms.assetid: 19d461f5-d72f-4de9-8c1d-0a6650316990
ms.openlocfilehash: c2dcb044d04099c51f57d82a8bc08f0932bf3542
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875419"
---
# <a name="withevents-visual-basic"></a>WithEvents (Visual Basic)

指定一个或多个已声明的成员变量引用可引发事件的类的实例。

## <a name="remarks"></a>备注

使用定义变量时 `WithEvents` ，可以通过声明方式指定方法使用关键字来处理变量的事件 `Handles` 。

只能 `WithEvents` 在类或模块级别使用。 这意味着变量的声明上下文 `WithEvents` 必须是类或模块，不能是源文件、命名空间、结构或过程。

不能 `WithEvents` 对结构成员使用。

只能声明单个变量（而不是数组） `WithEvents` 。

## <a name="rules"></a>规则

**元素类型。** 您必须 `WithEvents` 将变量声明为对象变量，以便它们可以接受类实例。 但是，不能将它们声明为 `Object` 。 您必须将它们声明为可引发事件的特定类。

`WithEvents`修饰符可用于以下上下文： [Dim 语句](../statements/dim-statement.md)

## <a name="example"></a>示例

```vb
Dim WithEvents app As Application
```

## <a name="see-also"></a>另请参阅

- [句柄数](../statements/handles-clause.md)
- [关键字](../keywords/index.md)
- [事件](../../programming-guide/language-features/events/index.md)
