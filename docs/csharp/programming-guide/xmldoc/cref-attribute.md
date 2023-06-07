---
title: cref 属性 - C# 编程指南
description: 了解 cref 属性。 cref 属性表示“代码引用”，指定标记的内部文本是一个代码元素。
ms.date: 07/20/2015
helpviewer_keywords:
- cref [C#]
ms.assetid: 66a6b0e5-b961-4504-a461-3a4cf481fc8b
ms.openlocfilehash: 31fa1a3f182d7b72a1dfbe1ce47386f87fbbff75
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381991"
---
# <a name="cref-attribute-c-programming-guide"></a>cref 属性（C# 编程指南）

XML 文档标记中的 `cref` 属性是指“代码引用”。 它指定标记的内部文本是一个代码元素，例如类型、方法或属性。 文档工具（例如 [DocFX](https://dotnet.github.io/docfx/) 和 [Sandcastle](https://github.com/EWSoftware/SHFB)）使用 `cref` 属性自动生成指向记录类型或成员的页面的超链接。

## <a name="example"></a>示例

下面的示例演示了在 [\<see>](./see.md) 标记中使用的 `cref` 属性。

[!code-csharp[csProgGuideDocComments#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#3)]

在编译时，该程序生成以下 XML 文件。 请注意，例如 `GetZero` 方法的 `cref` 属性已被编译器转换为 `"M:TestNamespace.TestClass.GetZero"`。 “M:”前缀表示“方法”，并且是一种由文档工具（例如 DocFX 和 Sandcastle）识别的约定。 有关前缀的完整列表，请参阅[处理 XML 文件](./processing-the-xml-file.md)。

```xml  
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>CRefTest</name>
    </assembly>
    <members>
        <member name="T:TestNamespace.TestClass">
            <summary>
            TestClass contains several cref examples.
            </summary>
        </member>
        <member name="M:TestNamespace.TestClass.#ctor">
            <summary>
            This sample shows how to specify the <see cref="T:TestNamespace.TestClass"/> constructor as a cref attribute.
            </summary>
        </member>
        <member name="M:TestNamespace.TestClass.#ctor(System.Int32)">
            <summary>
            This sample shows how to specify the <see cref="M:TestNamespace.TestClass.#ctor(System.Int32)"/> constructor as a cref attribute.
            </summary>
        </member>
        <member name="M:TestNamespace.TestClass.GetZero">
            <summary>
            The GetZero method.
            </summary>
            <example>
            This sample shows how to call the <see cref="M:TestNamespace.TestClass.GetZero"/> method.
            <code>
            class TestClass
            {
                static int Main()
                {
                    return GetZero();
                }
            }
            </code>
            </example>
        </member>
        <member name="M:TestNamespace.TestClass.GetGenericValue``1(``0)">
            <summary>
            The GetGenericValue method.
            </summary>
            <remarks>
            This sample shows how to specify the <see cref="M:TestNamespace.TestClass.GetGenericValue``1(``0)"/> method as a cref attribute.
            </remarks>
        </member>
        <member name="T:TestNamespace.GenericClass`1">
            <summary>
            GenericClass.
            </summary>
            <remarks>
            This example shows how to specify the <see cref="T:TestNamespace.GenericClass`1"/> type as a cref attribute.
            </remarks>
        </member>
    </members>
</doc>
```

## <a name="see-also"></a>请参阅

- [XML 文档注释](./index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
