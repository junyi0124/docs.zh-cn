---
title: 详细语法
description: '了解 F # 编程语言中的详细和轻型语法之间的差异。'
ms.date: 05/16/2016
ms.openlocfilehash: 4e1725b58c8cb67c074ba12fd4ca25ce0c000a1e
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97595172"
---
# <a name="verbose-syntax"></a>详细语法

F # 语言中有两种形式的语法可用于许多构造： *详细语法* 和 *轻量语法*。 详细语法并不常用，但优点在于缩进不太敏感。 轻型语法较短，并使用缩进来表示构造的开始和结束，而不是使用、、等其他关键字 `begin` `end` `in` 。 默认语法为轻型语法。 本主题介绍了未启用轻量语法时 F # 构造的语法。 详细语法始终处于启用状态，因此即使启用了轻型语法，仍可对某些构造使用详细语法。 您可以使用指令禁用轻型语法 `#light "off"` 。

## <a name="table-of-constructs"></a>构造表

下表显示了上下文中的 F # 语言构造的轻型和详细语法，这两种形式之间存在差异。 在此表中，尖括号 (&lt; &gt;) 将用户提供的语法元素括起来。 有关这些构造中使用的语法的更多详细信息，请参阅每个语言构造的文档。

<table>
<tr>
<th>语言构造</th>
<th>轻型语法</th>
<th>详细语法</th>
</tr>
<tr>
<td>
复合表达式
</td>
<td>

```fsharp
<expression1>
<expression2>
```

</td><td>

```fsharp
<expression1>; <expression2>
```

</td>
</tr>
<tr><td>

嵌套 `let` 绑定

</td><td>

```fsharp
let f x =
    let a = 1
    let b = 2
    x + a + b
```

</td><td>

```fsharp
let f x =
    let a = 1 in
    let b = 2 in
    x + a + b
```

</td>
</tr>
<tr><td>
代码块
</td><td>

```fsharp
(
    <expression1>
    <expression2>
)
```

</td><td>

```fsharp
begin
    <expression1>;
    <expression2>;
end
```

</td>
</tr>
<tr><td>
`for...do`
</td><td>

```fsharp
for counter = start to finish do
    ...
```

</td><td>

```fsharp
for counter = start to finish do
    ...
done
```

</td>
</tr>
<tr><td>
`while...do`
</td><td>

```fsharp
while <condition> do
    ...
```

</td><td>

```fsharp
while <condition> do
    ...
done
```

</td>
</tr>
<tr><td>
`for...in`
</td><td>

```fsharp
for var in start .. finish do
    ...
```

</td><td>

```fsharp
for var in start .. finish do
    ...
done
```

</td>
</tr>
<tr><td>
`do`
</td><td>

```fsharp
do
    ...
```

</td><td>

```fsharp
do
    ...
in
```

</td>
</tr>
<tr><td>记录 (record)
</td><td>

```fsharp
type <record-name> =
    {
        <field-declarations>
    }
    <value-or-member-definitions>
```

</td><td>

```fsharp
type <record-name> =
    {
        <field-declarations>
    }
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>class
</td><td>

```fsharp
type <class-name>(<params>) =
    ...
```

</td><td>

```fsharp
type <class-name>(<params>) =
    class
        ...
    end
```

</td>
</tr>
<tr><td>structure</td><td>

```fsharp
[<StructAttribute>]
type <structure-name> =
    ...
```

</td><td>

```fsharp
type <structure-name> =
    struct
        ...
    end
```

</td>
</tr>
<tr><td>可区分联合</td><td>

```fsharp
type <union-name> =
    | ...
    | ...
    ...
    <value-or-member definitions>
```

</td><td>

```fsharp
type <union-name> =
    | ...
    | ...
    ...
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>interface</td><td>

```fsharp
type <interface-name> =
    ...
```

</td><td>

```fsharp
type <interface-name> =
    interface
        ...
    end
```

</td>
</tr>
<tr><td>对象表达式</td><td>

```fsharp
{ new <type-name>
    with
        <value-or-member-definitions>
        <interface-implementations>
}
```

</td><td>

```fsharp
{ new <type-name>
    with
        <value-or-member-definitions>
    end
    <interface-implementations>
}
```

</td>
</tr>
<tr><td>接口实现</td><td>

```fsharp
interface <interface-name>
    with
        <value-or-member-definitions>
```

</td><td>

```fsharp
interface <interface-name>
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>类型扩展</td><td>

```fsharp
type <type-name>
    with
        <value-or-member-definitions>
```

</td><td>

```fsharp
type <type-name>
    with
        <value-or-member-definitions>
    end
```

</td>
</tr>
<tr><td>name</td><td>

```fsharp
module <module-name> =
    ...
```

</td><td>

```fsharp
module <module-name> =
    begin
        ...
    end
```

</td>
</tr>
</table>

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [编译器指令](compiler-directives.md)
- [代码格式设置准则](../style-guide/formatting.md)
