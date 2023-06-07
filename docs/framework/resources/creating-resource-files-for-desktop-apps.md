---
title: 为 .NET 应用创建资源文件
description: 为 .NET 应用创建资源文件。 以编程方式生成包含字符串资源的文本文件、XML 文件或二进制文件，或者生成包含字符串、图像或对象数据的 XML 文件。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- resource files, .resources files
- .resources files
- application resources, creating files
- resource files, creating
ms.assetid: 6c5ad891-66a0-4e7a-adcf-f41863ba6d8d
ms.openlocfilehash: d10af40420c1ab9ab177514c0babeaf5cea96922
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96259071"
---
# <a name="create-resource-files-for-net-apps"></a>为 .NET 应用创建资源文件

可以将字符串、图像或对象数据等资源包含在资源文件中，方便应用程序使用。 .NET Framework 提供了五种创建资源文件的方法：

- 创建一个包含字符串资源的文本文件。 可以使用[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 将文本文件转换成二进制资源 (.resources) 文件。 然后使用语言编译器将这个二进制资源文件嵌入可执行应用程序或应用程序库，或者使用[程序集链接器 (Al.exe)](../tools/al-exe-assembly-linker.md) 将这个二进制资源文件嵌入附属程序集。 有关详细信息，请参阅[文本文件中的资源](creating-resource-files-for-desktop-apps.md#TextFiles)部分。

- 创建一个包含字符串、图像或对象数据的 XML 资源 (.resx) 文件。 可以使用[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 将 .resx 文件转换成二进制资源 (.resources) 文件。 然后使用语言编译器将这个二进制资源文件嵌入可执行应用程序或应用程序库，或者使用[程序集链接器 (Al.exe)](../tools/al-exe-assembly-linker.md) 将这个二进制资源文件嵌入附属程序集。 有关详细信息，请参阅 [.resx 文件中的资源](creating-resource-files-for-desktop-apps.md#ResxFiles)部分。

- 使用 <xref:System.Resources> 命名空间中的类型以编程方式创建一个 XML 资源 (.resx) 文件。 可以创建一个 .resx 文件、枚举其资源并按名称检索特定资源。 有关详细信息，请参阅[以编程方式使用 .resx 文件](working-with-resx-files-programmatically.md)。

- 以编程方式创建一个二进制资源 (.resources) 文件。 然后使用语言编译器将该文件嵌入可执行应用程序或应用程序库，或者使用[程序集链接器 (Al.exe)](../tools/al-exe-assembly-linker.md) 将这个二进制资源文件嵌入附属程序集。 有关详细信息，请参阅 [.resources 文件中的资源](creating-resource-files-for-desktop-apps.md#ResourcesFiles)部分。

- 使用 [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 创建一个资源文件并将其包含在项目中。 Visual Studio 提供一个资源编辑器，借助该编辑器，可添加、删除和修改资源。 编译时，资源文件会自动转换成二进制 .resources 文件，并嵌入应用程序程序集或附属程序集中。 有关详细信息，请参阅 [Visual Studio 中的资源文件](creating-resource-files-for-desktop-apps.md#VSResFiles)部分。

<a name="TextFiles"></a>

## <a name="resources-in-text-files"></a>文本文件中的资源

文本（.txt 或 .restext）文件只能用于存储字符串资源。 对于非字符串资源，使用 .resx 文件或以编程方式创建它们。 包含字符串资源的文本文件使用以下格式：

```text
# This is an optional comment.
name = value

; This is another optional comment.
name = value

; The following supports conditional compilation if X is defined.
#ifdef X
name1=value1
name2=value2
#endif

# The following supports conditional compilation if Y is undefined.
#if !Y
name1=value1
name2=value2
#endif
```

 .txt 和 .restext 文件的资源文件格式是相同的。 .restext 文件扩展名仅用于明确区分文本文件和基于文本的资源文件。

 字符串资源显示为名称/值对，其中名称是标识资源的字符串，值是在将名称传递给资源检索方法（例如 <xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType>）时返回的资源字符串     。 名称和值必须用等号 (=) 分隔开   。 例如：

```text
FileMenuName=File
EditMenuName=Edit
ViewMenuName=View
HelpMenuName=Help
```

> [!CAUTION]
> 请勿使用资源文件存储密码、 安全敏感信息或私人数据。

 在文本文件中，允许存在空字符串（即值为 <xref:System.String.Empty?displayProperty=nameWithType> 的资源）。 例如：

```text
EmptyString=
```

 从 .NET Framework 4.5 开始并在所有版本的 .NET Core 中，文本文件支持使用 `#ifdef`symbol  ... `#endif` 和 `#if !`symbol  ... `#endif` 构造的传统编译。 还可以通过[资源文件生成器 (resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 使用 `/define` 开关来定义符号。 每个资源都需要其自己的 `#ifdef`symbol... `#endif` 或 `#if !`symbol... `#endif` 构造   。 如果使用的是 `#ifdef` 语句，并且定义了 symbol，则关联的资源将包括在 .resources 文件中；否则，将不包括此资源  。 如果使用的是 `#if !` 语句，并且没有定义 symbol，则关联的资源将包括在 .resources 文件中；否则，将不包括此资源  。

 注释在文本文件中为可选项，并且在每行开头使用分号 (;) 或者井号 (#) 开头。 包含注释的行可位于文件中的任何位置。 使用[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 创建的已编译 .resources 文件中不包含注释。

 文本文件中的所有空白行将视为空格并忽略。

 下面的示例定义名为 `OKButton` 和 `CancelButton` 的两个字符串资源。

```text
#Define resources for buttons in the user interface.
OKButton=OK
CancelButton=Cancel
```

 如果文本文件包含名称的重复匹配项，[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 将显示一条警告，并忽略第二个名称  。

 值不能包含换行符，但可以使用 C 语言样式的转义符，例如使用 `\n` 表示新行，使用 `\t` 表示制表符  。还可以使用经过转义的反斜杠字符（例如，“\\\\”）。 此外，允许使用空字符串。

 使用 little-endian 或 big-endian 字节顺序的 UTF-8 编码或 UTF-16 编码以文本文件格式保存资源。 但是在默认情况下，将 .txt 文件转换为 .resources 文件的[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 会将文件默认视为 UTF-8 文件。 如果希望 Resgen.exe 识别使用 UTF-16 编码的文件，必须在文件开头包含 Unicode 字节顺序标记 (U + FEFF)。

 若要将文本格式的资源文件嵌入到 .NET 程序集，必须使用[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 将文件转换为二进制资源 (.resources) 文件。 然后可使用语言编译器将 .resources 文件嵌入 .NET 程序集，或者使用[程序集链接器 (Al.exe)](../tools/al-exe-assembly-linker.md) 将其嵌入附属程序集。

 下面的示例在简单的“Hello World”控制台应用程序中使用了一个名为 GreetingResources.txt 的文本格式的资源文件。 该文本文件定义了 `prompt` 和 `greeting` 两个字符串，分别用于提示用户输入其名字和显示一条问候。

```text
# GreetingResources.txt
# A resource file in text format for a "Hello World" application.
#
# Initial prompt to the user.
prompt=Enter your name:
# Format string to display the result.
greeting=Hello, {0}!
```

使用以下命令可以将该文本文件转换成 .resources 文件：

```console
resgen GreetingResources.txt
```

 下面的示例展示了某控制台应用程序中的源代码，该控制台应用程序使用 .resources 文件向用户显示信息。

 [!code-csharp[Conceptual.Resources.TextFiles#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.textfiles/cs/greeting.cs#1)]
 [!code-vb[Conceptual.Resources.TextFiles#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.textfiles/vb/greeting.vb#1)]

 如果使用的是 Visual Basic，且源代码文件名为 Greeting.vb，以下命令将创建一个包含嵌入的 .resources 文件的可执行文件：

```console
vbc greeting.vb -resource:GreetingResources.resources
```

 如果使用的是 C#，并且将源代码文件命名为 Greeting.cs，以下命令将创建一个包含嵌入 .resources 文件的可执行文件：

```console
csc greeting.cs -resource:GreetingResources.resources
```

<a name="ResxFiles"></a>

## <a name="resources-in-resx-files"></a>.resx 文件中的资源

 与只能存储字符串资源的文本文件不同，XML 资源 (.resx) 文件可以存储字符串、二进制数据（图像、图标和音频剪辑等）以及编程对象。 .resx 文件包含一个标准标头，用以描述资源条目的格式，并指定用于解析数据的 XML 的版本信息。 资源文件数据跟在 XML 标头之后。 每个数据项由包含在 `data` 标记中的一个名称/值对构成。 其 `name` 属性定义资源名称，而嵌套的 `value` 标记包含资源值。 对于字符串数据，`value` 标记包含字符串。

 例如，以下 `data` 标记定义了一个名为 `prompt` 且值为“Enter your name”的字符串资源。

```xml
<data name="prompt" xml:space="preserve">
  <value>Enter your name:</value>
</data>
```

> [!WARNING]
> 请勿使用资源文件存储密码、 安全敏感信息或私人数据。

 对于资源对象，data 标记中包含了 `type` 属性，用于指示资源的数据类型  。 对于由二进制数据构成的对象，`data` 标记还包含 `mimetype` 属性，用以指示二进制数据的 `base64` 类型。

> [!NOTE]
> 所有的 .resx 文件都使用二进制序列化格式化程序来生成和分析指定类型的二进制数据。 因此如果对象的二进制序列化格式以不兼容的方式发生更改，.resx 文件可能失效。

 以下示例显示了部分包含 <xref:System.Int32> 资源和位图图像的 .resx 文件。

```xml
<data name="i1" type="System.Int32, mscorlib">
  <value>20</value>
</data>

<data name="flag" type="System.Drawing.Bitmap, System.Drawing,
    Version=1.0.5000.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a"
    mimetype="application/x-microsoft.net.object.bytearray.base64">
  <value>
    AAEAAAD/////AQAAAAAAAAAMAgAAADtTeX…
  </value>
</data>
```

> [!IMPORTANT]
> 由于 .resx 文件必须由采用预定义格式的格式标准的 XML 构成，不建议手动使用 .resx 文件，尤其是当 .resx 文件包含非字符串资源时。 相反，[Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 提供一个用于创建和操作 .resx 文件的透明接口。 有关详细信息，请参阅 [Visual Studio 中的资源文件](creating-resource-files-for-desktop-apps.md#VSResFiles)部分。 还可以通过编程方式创建和操作 .resx 文件。 有关详细信息，请参阅[以编程方式使用 .resx 文件](working-with-resx-files-programmatically.md)。

<a name="ResourcesFiles"></a>

## <a name="resources-in-resources-files"></a>.resources 文件中的资源

可以使用 <xref:System.Resources.ResourceWriter?displayProperty=nameWithType> 类以编程方式从代码中直接创建二进制资源 (.resources) 文件。 还可以使用[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 从文本文件或 .resx 文件创建 .resources 文件。 .resources 文件除了可以包含字符串数据之外，还可以包含二进制数据（字节数组）和对象数据。 以编程方式创建 .resources 文件需要执行下列步骤：

1. 创建一个具有唯一文件名的 <xref:System.Resources.ResourceWriter> 对象。 可以通过将文件名或文件流指定为 <xref:System.Resources.ResourceWriter> 类构造函数来实现这此操作。

2. 调用 <xref:System.Resources.ResourceWriter.AddResource%2A?displayProperty=nameWithType> 方法的重载之一以便将每个已命名的资源添加到文件。 该资源可以是字符串、对象或二进制数据（字节数组）集合。

3. 通过调用 <xref:System.Resources.ResourceWriter.Close%2A?displayProperty=nameWithType> 方法将资源写入文件并关闭 <xref:System.Resources.ResourceWriter> 对象。

> [!NOTE]
> 请勿使用资源文件存储密码、 安全敏感信息或私人数据。

 下面的示例以编程方式创建了一个名为 CarResources.resources 的 .resources 文件，该文件存储了六个字符串、一个图标和两个由应用程序定义的对象（两个 `Automobile` 对象）。 示例中定义并实例化的 `Automobile` 类，使用 <xref:System.SerializableAttribute> 属性进行标记，这样二进制序列化格式化程序便可持久保留该类。

 [!code-csharp[Conceptual.Resources.Resources#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resources/cs/resources1.cs#1)]
 [!code-vb[Conceptual.Resources.Resources#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resources/vb/resources1.vb#1)]

 创建 .resources 文件后，可以通过含入语言编译器的 `/resource` 开关将其嵌入运行时可执行文件或库中，或者通过使用[程序集链接器 (Al.exe)](../tools/al-exe-assembly-linker.md) 将其嵌入附属程序集。

<a name="VSResFiles"></a>

## <a name="resource-files-in-visual-studio"></a>Visual Studio 中的资源文件

将资源文件添加到 [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 项目时，Visual Studio 会在项目目录中创建一个 .resx 文件。 Visual Studio 会提供资源编辑器，可用于添加字符串、图像和二进制对象。 编辑器只能用于处理静态数据，因此不能用于储存编程对象；必须以编程方式将对象数据写入 .resx 文件或 .resources 文件。 有关详细信息，请参阅[以编程方式使用 .resx 文件](working-with-resx-files-programmatically.md)和 [.resources 文件中的资源](creating-resource-files-for-desktop-apps.md#ResourcesFiles)部分。

如果要添加本地化资源，请为它们提供与主资源文件相同的根文件名称。 还应在文件名中指定其区域性。 例如，如果要添加一个名为 Resources.resx 的资源文件，或许还需要创建名为 Resources.en-US.resx 和 Resources.fr-fr.resx 的资源文件，分别用以保存英语（美国）和法语（法国）区域性的本地化资源。 还应该指定应用程序的默认区域性。 找不到特定区域性的本地化资源时，将使用此默认区域性的资源。 若要指定默认区域性，请在 Visual Studio 的解决方案资源管理器中，右键单击项目名称，指向“应用程序”，单击“程序集信息”，然后在“非特定语言”列表中选择合适的语言/区域性   。

编译时，Visual Studio 首先将项目中的 .resx 文件转换为二进制资源 (.resources) 文件，并将其存储在项目 obj  目录的子目录中。 Visual Studio 会将不包含本地化资源的所有资源文件嵌入项目生成的主程序集中。 如果资源文件包含本地化资源，Visual Studio 会将其嵌入用于每个本地化区域性的单独的附属程序集中。 然后将每个附属程序集存储到名称与本地化区域性相对应的目录中。 例如，本地化的英语（美国）资源存储在 en-US 子目录的附属程序集中。

## <a name="see-also"></a>请参阅

- <xref:System.Resources>
- [.NET 应用中的资源](index.md)
- [打包和部署资源](packaging-and-deploying-resources-in-desktop-apps.md)
