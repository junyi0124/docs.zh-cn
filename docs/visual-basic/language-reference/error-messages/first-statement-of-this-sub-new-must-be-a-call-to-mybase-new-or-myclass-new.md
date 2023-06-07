---
title: 该“Sub New”的第一个语句必须是对“MyBase.New”或“MyClass.New”的调用（没有不带参数的可访问构造函数）
ms.date: 07/20/2015
f1_keywords:
- bc30148
- vbc30148
helpviewer_keywords:
- BC30148
ms.assetid: 4426e8fc-cb39-4eb8-ba95-503cd32fcc89
ms.openlocfilehash: bce8ad10bc201386f34d6623741c7d41a5dec27e
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163020"
---
# <a name="bc30148-first-statement-of-this-sub-new-must-be-a-call-to-mybasenew-or-myclassnew-no-accessible-constructor-without-parameters"></a>BC30148：此 "Sub New" 的第一条语句必须是对 "MyBase. New" 或 "MyClass" 的调用。 (没有无参数的可访问构造函数) 

此 "Sub New" 的第一条语句必须是对 "MyBase. New" 或 "MyClass" 的调用 \<basename> ，因为 "" 的基类 "" 没有 \<derivedname> 可在没有参数的情况下调用的可访问 "Sub New"。

 在派生类中，每个构造函数都必须调用基类构造函数 (`MyBase.New`) 。 如果基类的构造函数不具有可由派生类访问的参数，则 `MyBase.New` 可以自动调用。 如果不是，则必须使用参数调用基类构造函数，这不能自动完成。 在这种情况下，每个派生类构造函数的第一条语句必须调用基类上的参数化构造函数，或调用派生类中的另一个构造函数，该构造函数将调用基类构造函数。

 **错误 ID：** BC30148

## <a name="to-correct-this-error"></a>更正此错误

- 请调用 `MyBase.New` 提供所需的参数，或调用进行此类调用的对等构造函数。

     例如，如果基类的构造函数声明为 `Public Sub New(ByVal index as Integer)` ，则派生类构造函数中的第一条语句可能是 `MyBase.New(100)` 。

## <a name="see-also"></a>另请参阅

- [继承基础知识](../../programming-guide/language-features/objects-and-classes/inheritance-basics.md)
