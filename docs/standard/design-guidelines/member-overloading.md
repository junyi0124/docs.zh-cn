---
title: 成员重载
ms.date: 10/22/2008
helpviewer_keywords:
- default arguments
- members [.NET Framework], overloaded
- member design guidelines [.NET Framework], overloading
- overloaded members
- signatures, members
ms.assetid: 964ba19e-8b94-4b5b-b1e3-5a0b531a0bb1
ms.openlocfilehash: fe8bf23a04e6684564d3d7e287c2a009e0817732
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706627"
---
# <a name="member-overloading"></a>成员重载

成员重载意味着在同一类型上创建两个或多个成员，这些成员仅在参数的数量或类型中不同，但具有相同的名称。 例如，在下面的中，将 `WriteLine` 重载方法：

```csharp
public static class Console {
    public void WriteLine();
    public void WriteLine(string value);
    public void WriteLine(bool value);
    ...
}
```

 由于只有方法、构造函数和索引属性可以有参数，因此只能重载这些成员。

 重载是提高可重用库的可用性、生产力和可读性的最重要方法之一。 参数数量重载使您可以提供更简单的构造函数和方法版本。 使用参数类型的重载，可以为在所选的不同类型集上执行相同操作的成员使用相同的成员名称。

 ✔️尝试使用描述性参数名称来指示较短重载使用的默认值。

 ❌ 避免在重载中随意改变参数名称。 如果一个重载中的参数表示与另一个重载中的参数相同的输入，则这些参数应具有相同的名称。

 ❌ 避免在重载成员的参数顺序中不一致。 具有相同名称的参数应出现在所有重载的同一位置。

 ✔️如果需要可扩展性) ，则仅设置最长重载虚拟 (。 更短的重载只需调用更长的重载。

 ❌ 不要使用 `ref` 或 `out` 修饰符重载成员。

 某些语言无法解析对重载的调用。 此外，此类重载通常具有完全不同的语义，但可能不应作为重载，而是使用两种不同的方法。

 ❌ 不要让重载的参数的位置和类型与语义不同。

 `null`对于可选参数，✔️允许传递。

 ✔️确实使用成员重载，而不是使用默认参数定义成员。

 默认参数不符合 CLS。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [成员设计准则](member.md)
- [框架设计准则](index.md)
