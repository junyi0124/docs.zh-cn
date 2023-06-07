---
title: 标识符名称
description: 了解 C# 编程语言中有效标识符名称的规则。
ms.date: 08/21/2018
ms.openlocfilehash: bef6e2ea285b5391af3350ae42a4105d140c6d1b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "77673363"
---
# <a name="identifier-names"></a>标识符名称

标识符是分配给类型（类、接口、结构、委托或枚举）、成员、变量或命名空间的名称  。 有效标识符必须遵循以下规则：

- 标识符必须以字母或 `_` 开头。
- 标识符可以包含 Unicode 字母字符、十进制数字字符、Unicode 连接字符、Unicode 组合字符或 Unicode 格式字符。 有关 Unicode 类别的详细信息，请参阅 [Unicode 类别数据库](https://www.unicode.org/reports/tr44/)。
可以在标识符上使用 `@` 前缀来声明与 C# 关键字匹配的标识符。 `@` 不是标识符名称的一部分。 例如，`@if` 声明名为 `if` 的标识符。 这些[逐字标识符](../../language-reference/tokens/verbatim.md)主要用于与使用其他语言声明的标识符的互操作性。

有关有效标识符的完整定义，请参阅 [C# 语言规范中的标识符主题](../../../../_csharplang/spec/lexical-structure.md#identifiers)。

## <a name="naming-conventions"></a>命名约定

除了规则之外，在 .NET API 中还使用了许多标识符[命名约定](../../../standard/design-guidelines/naming-guidelines.md)。 按照约定，C# 程序对类型名称、命名空间和所有公共成员使用 `PascalCase`。 此外，以下约定也很常见：

- 接口名称以大写字母 `I` 开头。
- 属性类型以单词 `Attribute` 结尾。
- 枚举类型对非标记使用单数名词，对标记使用复数名词。
- 标识符不应包含两个连续的 `_` 字符。 这些名称保留给编译器生成的标识符。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [在 C# 程序内部](./index.md)
- [C# 参考](../../language-reference/index.md)
- [类](../classes-and-structs/classes.md)
- [结构类型](../../language-reference/builtin-types/struct.md)
- [命名空间](../namespaces/index.md)
- [接口](../interfaces/index.md)
- [委托](../delegates/index.md)
