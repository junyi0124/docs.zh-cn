---
ms.openlocfilehash: 9a2d6a25a8ab1b8bf65b947557802e0805a7f826
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617123"
---
### <a name="foreach-iterator-variable-is-now-scoped-within-the-iteration-so-closure-capturing-semantics-are-different-in-c5"></a>Foreach 迭代器变量现在已在迭代范围内，因此闭包捕获语义会不同（在 C#5 中）

#### <a name="details"></a>详细信息

从 C# 5 (Visual Studio 2012) 开始，`foreach` 迭代器变量在迭代范围内。 如果代码以前依靠变量而不在 `foreach` 闭包内，这可能会导致中断。 此更改的特征如下：传递到委托的迭代器变量将视为委托创建时它拥有的值，而非调用委托时它拥有的值。

#### <a name="suggestion"></a>建议

理想情况下，应更新代码以使用新的编译器行为。 如果需要旧语义，迭代器变量可替换为显式置于循环范围外的单独变量。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.5         |
| 类型    | 重定目标 |
