---
title: 以编程方式使用 .resx 文件
description: 使用 .NET 类库的 System.Resources 命名空间中的类型和成员以编程方式从 XML 资源 (.resx) 文件中创建或检索数据。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- resource files, .resx files
- .resx files
ms.assetid: 168f941a-2b84-43f8-933f-cf4a8548d824
ms.openlocfilehash: c6b1ef6c7dd8be3dbc98b2298ab0e649ff74008e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254501"
---
# <a name="work-with-resx-files-programmatically"></a>以编程方式使用 .resx 文件

由于 XML 资源 (.resx) 文件必须由定义完善的 XML 组成，这些 XML 的标头必须遵循特定架构（后跟名称/值对的数据），因此你会发现手动创建这些文件很容易出错。 作为一种替代方法，可以使用 .NET 类库中的类型和成员以编程方式创建 .resx 文件。 还可以使用 .NET 类库来检索存储在 .resx 文件中的资源。 本文说明如何使用 <xref:System.Resources> 命名空间的类型和成员来操作 .resx 文件。

本文讨论的是如何操作包含资源的 XML (.resx) 文件。 有关操作嵌入程序集中的二进制资源文件的信息，请参阅 <xref:System.Resources.ResourceManager>。

> [!WARNING]
> 除编程方式以外还可以使用其他方式操作 .resx 文件。 如果将资源文件添加到 [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 项目，那么 Visual Studio 会提供一个用于创建和维护 .resx 文件的接口，并且在编译时自动将 .resx 文件转换为 .resources 文件。 你还可以使用文本编辑器来直接操作 .resx 文件。 但是，若要避免破坏文件，请注意不要修改存储在文件中的任何二进制信息。

## <a name="create-a-resx-file"></a>创建 .resx 文件

你可以使用 <xref:System.Resources.ResXResourceWriter?displayProperty=nameWithType> 类以编程方式创建 .resx 文件，步骤如下：

1. 通过调用 <xref:System.Resources.ResXResourceWriter> 方法和提供 .resx 文件的名称实例化 <xref:System.Resources.ResXResourceWriter.%23ctor%28System.String%29> 对象。 此文件名必须包括 .resx 扩展名。 如果实例化位于 <xref:System.Resources.ResXResourceWriter> 块中的 `using` 对象，则无需在步骤 3 中显式调用 <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> 方法。

2. 为每个要添加到此文件中的资源调用 <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType> 方法。 使用此方法的重载添加字符串、 对象和二进制（字节数组）数据。 如果资源是一个对象，则它必须是可序列化的。

3. 调用 <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> 方法以生成资源文件并释放所有资源。 如果 <xref:System.Resources.ResXResourceWriter> 对象是在 `using` 块中创建的，那么资源将写入 .resx 文件，并且在 <xref:System.Resources.ResXResourceWriter> 块的末尾释放 `using` 对象所使用的资源。

生成的 .resx 文件具有相应的标头，并且由 `data` 方法添加的每个资源都有一个 <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType> 标记。

> [!WARNING]
> 请勿使用资源文件存储密码、 安全敏感信息或私人数据。

下面的示例创建了一个名为 CarResources.resx 的 .resx 文件，该文件存储了六个字符串、一个图标和两个由应用程序定义的对象（两个 `Automobile` 对象）。 本示例中定义并实例化的 `Automobile` 类使用 <xref:System.SerializableAttribute> 属性进行标记。

[!code-csharp[Conceptual.Resources.ResX#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/create1.cs#1)]
[!code-vb[Conceptual.Resources.ResX#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/create1.vb#1)]

> [!TIP]
> 还可以使用 [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 创建 .resx 文件。 在编译时，Visual Studio 使用 [资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 将 .resx 文件转换为二进制资源 (.resources) 文件，然后还将其嵌入应用程序集或附属程序集。

你无法将 .resx 文件嵌入运行时可执行文件中或将其编译到附属程序集。 必须使用 [资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md)将 .resx 文件转换为二进制资源 (.resources) 文件 。 然后可以将生成的 .resources 文件嵌入应用程序集或附属程序集。 有关详细信息，请参阅 [Creating Resource Files](creating-resource-files-for-desktop-apps.md)。

## <a name="enumerate-resources"></a>枚举资源

 在某些情况下，你可能想要从 .resx 文件中检索所有资源，而不是某个特定资源。 若要执行此操作，可以使用 <xref:System.Resources.ResXResourceReader?displayProperty=nameWithType> 类，该类为 .resx 文件中的所有资源提供枚举器。 <xref:System.Resources.ResXResourceReader?displayProperty=nameWithType> 类将实现 <xref:System.Collections.IDictionaryEnumerator>，并返回 <xref:System.Collections.DictionaryEntry> 对象，该对象代表用于循环的每个迭代的特定资源。 此对象的 <xref:System.Collections.DictionaryEntry.Key%2A?displayProperty=nameWithType> 属性返回资源的键， <xref:System.Collections.DictionaryEntry.Value%2A?displayProperty=nameWithType> 属性返回资源的值。

 下面的示例为前面的示例中创建的 CarResources.resx 文件创建 <xref:System.Resources.ResXResourceReader> 对象，并在整个资源文件中进行迭代。 该示例将资源文件中定义的两个 `Automobile` 对象添加到 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> 对象，以及将六个字符串中的五个添加到 <xref:System.Collections.SortedList> 对象。 <xref:System.Collections.SortedList> 对象中的值将转换为一个参数数组，该参数数组用于向控制台显示列标题。 还将向控制台显示 `Automobile` 属性值。

 [!code-csharp[Conceptual.Resources.ResX#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/enumerate1.cs#2)]
 [!code-vb[Conceptual.Resources.ResX#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/enumerate1.vb#2)]

## <a name="retrieve-a-specific-resource"></a>检索特定的资源

 除了枚举 .resx 文件中的项，你可以使用 <xref:System.Resources.ResXResourceSet?displayProperty=nameWithType> 类按名称检索特定资源。 <xref:System.Resources.ResourceSet.GetString%28System.String%29?displayProperty=nameWithType> 方法用于检索命名字符串资源的值。 <xref:System.Resources.ResourceSet.GetObject%28System.String%29?displayProperty=nameWithType> 方法用于检索命名对象或二进制数据的值。 该方法返回的对象之后必须转换为（C# 中的 cast 或 Visual Basic 中的 convert）相应类型的对象。

 以下示例按资源名称检索窗体的标题字符串和图标， 还检索之前示例中使用的应用程序定义的 `Automobile` 对象，并在 <xref:System.Windows.Forms.DataGridView> 控件中显示这些对象。

 [!code-csharp[Conceptual.Resources.ResX#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.resx/cs/retrieve1.cs#3)]
 [!code-vb[Conceptual.Resources.ResX#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.resx/vb/retrieve1.vb#3)]

## <a name="convert-resx-files-to-binary-resources-files"></a>将 .resx 文件转换为二进制 .resources 文件

 将 .resx 文件转换为嵌入式二进制资源 (.resources) 文件可以带来明显的好处。 虽然 .resx 文件容易阅读，并且在应用程序开发期间易于维护，但是完成的应用程序很少包含这些文件。 如果这些文件随应用程序分发，那么它们将作为单独文件存在，与应用程序可执行文件和随附的库分开存放。 与此相反，.resources 文件则嵌入应用程序可执行文件或其随附的程序集中。 另外，对于在运行时依赖 .resx 文件的本地化应用程序，开发人员负责处理资源回退。 与此相反，如果创建了一组包含嵌入式 .resources 文件的附属程序集，则公共语言运行时将处理资源回退进程。

 要将 .resx 文件转换为 .resources 文件，请使用 [资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md)，它具有以下基本语法：

 **Resgen.exe** *.resxFilename*

 这会生成一个具有与 .resx 文件相同的根文件名的二进制资源文件，并且该文件的扩展名为 .resources。 然后，可在编译时将该文件编译为可执行文件或库。 如果你使用的是 Visual Basic 编译器，则使用以下语法以在应用程序的可执行文件中嵌入一个 .resources 文件：

 **vbc** *filename* **.vb -resource:** *.resourcesFilename*

 如果使用的是 C#，则语法如下所示：

 **csc** *filename* **.cs -resource:** *.resourcesFilename*

 还可以使用 [程序集链接器 (AL.exe)](../tools/al-exe-assembly-linker.md)在附属程序集中嵌入 .resources 文件，基本语法如下：

 **al** *resourcesFilename* **-out:** *assemblyFilename*

## <a name="see-also"></a>请参阅

- [创建资源文件](creating-resource-files-for-desktop-apps.md)
- [Resgen.exe（资源文件生成器）](../tools/resgen-exe-resource-file-generator.md)
- [Al.exe（程序集链接器）](../tools/al-exe-assembly-linker.md)
