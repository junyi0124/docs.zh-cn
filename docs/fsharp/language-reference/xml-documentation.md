---
title: XML 文档
description: '了解有关 F # 的支持，以便从注释生成文档。'
ms.date: 09/15/2020
ms.openlocfilehash: 2f8788e79ea964143eecfaddbeb039474dd61f8e
ms.sourcegitcommit: 5d9cee27d9ffe8f5670e5f663434511e81b8ac38
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98025011"
---
# <a name="document-your-code-with-xml-comments"></a>使用 XML 注释记录代码

可以在 F # 中生成三斜杠 (///) 代码注释中的文档。 XML 注释可以在代码文件中的声明之前 () 或签名 ( fsi.exe) 文件中。

XML 文档注释是一种特殊注释，添加在任何用户定义的类型或成员的定义上方。
其特殊之处在于其可由编译器处理，由此在编译时生成 XML 文档文件。
编译器生成的 XML 文件可以与 .NET 程序集一起分发，以便 Ide 可以使用工具提示来显示关于类型或成员的快速信息。 此外，可以通过 [fsdocs](http://fsprojects.github.io/FSharp.Formatting/) 等工具运行 XML 文件，以生成 API 参考网站。

编译器将忽略 XML 文档注释，就像所有其他注释一样，除非启用下述选项，以在编译时检查注释的有效性和完整性。

可通过执行下列操作之一在编译时生成 XML 文件：

- 可以将元素添加 `GenerateDocumentationFile` 到 `<PropertyGroup>` 项目文件的部分中 `.fsproj` ，这会在项目目录中生成一个 XML 文件，该文件与程序集具有相同的根文件名。 例如：

   ```xml
   <GenerateDocumentationFile>true</GenerateDocumentationFile>
   ```

- 如果使用 Visual Studio 开发应用程序，右键单击项目并选择“属性”  。 在属性对话框中，选择“生成”  选项卡，然后选中“XML 文档文件”  。 还可以更改编译器写入文件的位置。

可以通过两种方式编写 XML 文档注释：带和不带 XML 标记。 两者都使用三斜杠注释。

## <a name="comments-without-xml-tags"></a>无 XML 标记的注释

如果 `///` 注释未以开头 `<` ，则会将整个注释文本作为紧随其后的代码构造的摘要文档。 如果只想编写每个构造的简短摘要，请使用此方法。

文档准备期间，注释将编码为 XML，因此 `<` ，、和等字符 `>` `&` 不需要进行转义。 如果未显式指定摘要标记，则不应指定其他标记，如 **param** 或 **返回** 标记。

下面的示例显示了不带 XML 标记的替代方法。 在此示例中，注释中的整个文本都被视为一个摘要。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7102.fs)]

## <a name="comments-with-xml-tags"></a>带有 XML 标记的注释

如果注释正文以 `<` (通常) 开头 `<summary>` ，则使用 xml 标记将其视为 xml 格式的注释正文。 使用此第二个参数，您可以为简短摘要、附加备注、每个参数和类型参数的文档和所引发的异常以及对返回值的说明单独指定备注。

下面是签名文件中的典型 XML 文档注释：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet7101.fs)]

## <a name="recommended-tags"></a>建议的标记

如果使用的是 XML 标记，下表描述了在 F # XML 代码注释中识别的外部标记。

| 标记语法                                  | 描述 |
|---------------------------------------------|-----------|
| `<summary>`**_全文_**`</summary>`           | 指定 *文本* 为程序元素的简短说明。 描述通常是一两个句子。|
| `<remarks>`**_全文_**`</remarks>`           | 指定 *文本* 包含有关程序元素的补充信息。|
| `<param name="`**_名称_** `">`**_描述_**`</param>` | 指定函数或方法参数的名称和说明。|
| `<typeparam name="`**_名称_** `">`**_描述_**`</typeparam>` | 指定类型参数的名称和说明。|
| `<returns>`**_全文_**`</returns>`           | 指定 *文本* 描述函数或方法的返回值。|
| `<exception cref="`**_类型_** `">`**_描述_**`</exception>` |指定可以生成的异常的类型以及引发此异常的情况。|
| `<seealso cref="`**_对_**`"/>`      | 指定另一种类型的文档的链接。 *引用* 是显示在 XML 文档文件中的名称。 另请参阅链接通常显示在文档页的底部。|

下表描述了 "说明" 部分中使用的标记：

| 标记语法                                | 描述 |
|-------------------------------------------|-------------|
| `<para>`**_全文_**`</para>`               | 指定文本段落。 用于分隔 **备注** 标记内的文本。|
| `<code>`**_全文_**`</code>`               | 指定 *文本* 为多行代码。 文档生成器可以使用此标记以适用于代码的字体显示文本。|
| `<paramref name="`**_路径名_**`"/>`         | 指定对同一文档注释中的参数的引用。|
| `<typeparamref name="`**_路径名_**`"/>`     | 指定对同一文档注释中类型参数的引用。|
| `<c>`**_全文_**`</c>`                     | 指定 *文本* 为内联代码。 文档生成器可以使用此标记以适用于代码的字体显示文本。|
| `<see cref="`**_参考_** `">`**_文本_**`</see>` | 指定指向其他程序元素的内联链接。 *引用* 是显示在 XML 文档文件中的名称。 *文本* 是链接中显示的文本。|

### <a name="user-defined-tags"></a>用户定义的标记

前面的标记表示由 F # 编译器和典型的 F # 编辑器工具识别的标记。 但用户可随意定义自己的标记。
诸如 fsdocs 之类的工具将支持额外的标记，例如 [\<namespacedoc>](https://github.com/fsharp/fslang-design/blob/master/tooling/FST-1031-xmldoc-extensions.md) 。
自定义或内部文档生成工具也可与标准标记配合使用，并支持 HTML 到 PDF 等多种输出格式。

## <a name="compile-time-checking"></a>编译时检查

`--warnon:3390`启用后，编译器将验证 XML 语法和和标记中引用的参数 `<param>` `<paramref>` 。

## <a name="documenting-f-constructs"></a>记录 F # 构造

F # 构造（如模块、成员、联合用例和记录字段）在 `///` 声明之前立即由注释记录。
如果需要，可以通过 `///` 在参数列表之前提供注释来记录类的隐式构造函数。 例如：

```fsharp
/// This is the type
type SomeType
      /// This is the implicit constructor
      (a: int, b: int) =

    /// This is the member
    member _.Sum() = a + b
```

## <a name="limitations"></a>限制

F # 不支持 c # 和其他 .NET 语言中的 XML 文档的某些功能。

- 在 F # 中，交叉引用必须使用相应符号的完整 XML 签名，例如 `cref="T:System.Console"` 。
  简单的 c # 样式交叉引用（如） `cref="Console"` 不会详细阐述为完整的 XML 签名，并且 F # 编译器不会检查这些元素。 某些文档工具可能允许通过后续处理使用这些交叉引用，但应使用完整的签名。

- `<include>` `<inheritdoc>` F # 编译器不支持标记。 如果使用了错误，则不会给出任何错误，但它们只是复制到生成的文档文件，而不会影响生成的文档。

- F # 编译器不检查交叉引用，即使 `-warnon:3390` 使用了也是如此。

- `<typeparam>` `<typeparamref>` 即使使用了，也不会通过 F # 编译器检查标记和中使用的名称 `--warnon:3390` 。

- 如果缺少文档，甚至在使用时也不会发出警告 `--warnon:3390` 。

## <a name="recommendations"></a>建议

出于多种原因，建议编制代码文档。 下面是一些最佳实践，一般用例方案，以及在 F # 代码中使用 XML 文档标记时应了解的内容。

- 启用 `--warnon:3390` 代码中的选项，以帮助确保 xml 文档是有效的 xml。

- 请考虑从实现中添加签名文件来分隔长 XML 文档注释。

- 为保持一致性，应编制所有公共可见类型及其成员的文档。 如果必须这么做，请完整全面地完成这一操作。

- 至少，模块、类型及其成员应具有普通 `///` 注释或 `<summary>` 标记。 这会显示在 F # 编辑工具中自动完成的工具提示窗口中。

- 应使用句号结尾的完整句子编写文档文本。

## <a name="see-also"></a>另请参阅

- C [# XML 文档注释 &#40;c&#35; 编程指南&#41;](../../csharp/programming-guide/xmldoc/index.md)。
- [F# 语言参考](index.md)
- [编译器选项](compiler-options.md)
