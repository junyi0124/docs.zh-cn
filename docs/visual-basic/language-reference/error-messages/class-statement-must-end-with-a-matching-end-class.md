---
title: “Class”语句必须以匹配的“End Class”结束
ms.date: 07/20/2015
f1_keywords:
- vbc30481
- bc30481
helpviewer_keywords:
- BC30481
ms.assetid: 583f3029-bc3a-4e06-866f-92dbecc46f19
ms.openlocfilehash: 6889e97aad913f6911ce438892752542de0d10f0
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163189"
---
# <a name="bc30481-class-statement-must-end-with-a-matching-end-class"></a>BC30481： "Class" 语句必须以匹配的 "End Class" 结束

`Class` 用于启动 `Class` 块; 因此它只能出现在块的开头，并以匹配的 `End Class` 语句结束块。 您有冗余的 `Class` 语句，或者您没有 `Class` 用结束块 `End Class` 。

 **错误 ID：** BC30481

## <a name="to-correct-this-error"></a>更正此错误

- 找到并删除不必要的 `Class` 语句。

- `Class`使用匹配的结束块 `End Class` 。

## <a name="see-also"></a>另请参阅

- [End \<keyword> 语句](../statements/end-keyword-statement.md)
- [Class 语句](../statements/class-statement.md)
