---
description: 详细了解 C# 中的 void 关键字
title: void - C# 参考
ms.date: 02/11/2020
f1_keywords:
- void_CSharpKeyword
- void
- (void)
helpviewer_keywords:
- void keyword [C#]
ms.assetid: 0d2d8a95-fe20-4fbd-bf5d-c1e54bce71d4
ms.openlocfilehash: fb22fffadeff4db9fcd8e1c8753d44455186aa5a
ms.sourcegitcommit: 870bc4b4087510f6fba3c7b1c0d391f02bcc1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92471632"
---
# <a name="void-c-reference"></a>void（C# 参考）

可以将 `void` 用作[方法](../../programming-guide/classes-and-structs/methods.md)（或[本地函数](../../programming-guide/classes-and-structs/local-functions.md)）的返回类型来指定该方法不返回值。

[!code-csharp[void method](snippets/shared/VoidType.cs#VoidExample)]

还可以将 `void` 用作引用类型来声明指向未知类型的指针。 有关详细信息，请参阅[指针类型](../../programming-guide/unsafe-code-pointers/pointer-types.md)。

不能将 `void` 用作变量的类型。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- <xref:System.Void?displayProperty=nameWithType>
