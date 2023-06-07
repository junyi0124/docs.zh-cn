---
title: 显式接口实现 - C# 编程指南
description: 类可以在 C# 中实现包含具有相同签名的成员的接口。 显式实现创建特定于一个接口的类成员。
ms.date: 01/24/2020
helpviewer_keywords:
- explicit interfaces [C#]
- interfaces [C#], explicit
ms.assetid: 181c901f-0d4c-4f29-97fc-895079617bf2
ms.openlocfilehash: a6ec328c08d1da84a11431d9400a094df8c72223
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303082"
---
# <a name="explicit-interface-implementation-c-programming-guide"></a>显式接口实现（C# 编程指南）

如果一个[类](../../language-reference/keywords/class.md)实现的两个接口包含签名相同的成员，则在该类上实现此成员会导致这两个接口将此成员用作其实现。 如下示例中，所有对 `Paint` 的调用皆调用同一方法。 第一个示例定义类型：

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefineTypes)]

下面的示例调用方法：

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallMethods)]

当两个接口成员不执行相同的功能时，它会导致其中一个或两个接口的实现不正确。 创建仅通过接口调用且特定于该接口的类成员，则有可能显式实现接口成员。 请使用接口名称和句点命名类成员。 例如：

[!code-csharp[DefineExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#ExplicitImplementation)]

类成员 `IControl.Paint` 仅通过 `IControl` 接口可用，`ISurface.Paint` 仅通过 `ISurface` 可用。 这两个方法实现相互独立，两者均不可直接在类上使用。 例如：

[!code-csharp[CallExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallExplicitImplementation)]

显式实现还用于处理两个接口分别声明名称相同的不同成员（例如属性和方法）的情况。 若要实现两个接口，类必须对属性 `P` 或方法 `P` 使用显式实现，或对二者同时使用，从而避免编译器错误。 例如：

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#NameCollision)]

从 [C# 8.0](../../whats-new/csharp-8.md#default-interface-methods) 开始，你可以为在接口中声明的成员定义一个实现。 如果类从接口继承方法实现，则只能通过接口类型的引用访问该方法。 继承的成员不会显示为公共接口的一部分。 下面的示例定义接口方法的默认实现：

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefaultImplementation)]

下面的示例调用默认实现：

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallDefaultImplementation)]

任何实现 `IControl` 接口的类都可以重写默认的 `Paint` 方法，作为公共方法或作为显式接口实现。

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [类和结构](../classes-and-structs/index.md)
- [接口](./index.md)
- [继承](../classes-and-structs/inheritance.md)
