---
description: '#error - C# 参考'
title: '#error - C# 参考'
ms.date: 08/24/2020
f1_keywords:
- '#error'
helpviewer_keywords:
- '#error directive [C#]'
ms.assetid: f2a7f3af-4cf9-4111-b369-70204d24b26b
ms.openlocfilehash: 76325901c5e39f340545b186a0aa9546cd853ff8
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89138093"
---
# <a name="error-c-reference"></a>#error（C# 参考）

`#error` 可从代码中的特定位置生成 [CS1029](../compiler-messages/cs1029.md) 用户定义的错误。 例如：

```csharp
#error Deprecated code in this method.
```

> [!NOTE]
> 编译器以特殊的方式处理 `#error version` 并报告编译器错误 CS8304，消息中包含使用的编译器和语言版本。

## <a name="remarks"></a>备注

`#error` 常用于条件指令中。

还可使用 [#warning](./preprocessor-warning.md) 生成用户定义警告。

## <a name="example"></a>示例

```csharp
// preprocessor_error.cs
// CS1029 expected
#define DEBUG
class MainClass
{
    static void Main()
    {
#if DEBUG
#error DEBUG is defined
#endif
    }
}
```

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 预处理器指令](./index.md)
