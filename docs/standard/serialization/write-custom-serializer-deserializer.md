---
title: 如何使用 System.Text.Json 编写自定义序列化程序和反序列化程序
description: 了解如何使用 System.Text.Json 命名空间编写适用于 JSON 的自定义序列化程序和反序列化程序。
ms.date: 11/30/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: a01d3c8dd18c114ea1c3aabc402bc841a6025ffe
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96439774"
---
# <a name="how-to-write-custom-serializers-and-deserializers-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 编写自定义序列化程序和反序列化程序

<xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName> 是面向 UTF-8 编码 JSON 文本的一个高性能、低分配的只进读取器，从 `ReadOnlySpan<byte>` 或 `ReadOnlySequence<byte>` 读取信息。 `Utf8JsonReader` 是一种低级类型，可用于生成自定义分析器和反序列化程序。 <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> 方法在后台使用 `Utf8JsonReader`。

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName> 是一种高性能方式，从常见 .NET 类型（例如，`String`、`Int32` 和 `DateTime`）编写 UTF-8 编码的 JSON 文本。 该编写器是一种低级类型，可用于生成自定义序列化程序。 <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> 方法在后台使用 `Utf8JsonWriter`。

<xref:System.Text.Json.JsonDocument?displayProperty=fullName> 提供使用 `Utf8JsonReader` 生成只读文档对象模型 (DOM) 的功能。 DOM 提供对 JSON 有效负载中的数据的随机访问。 可以通过 <xref:System.Text.Json.JsonElement> 类型访问构成有效负载的 JSON 元素。 `JsonElement` 类型提供数组和对象枚举器，以及用于将 JSON 文本转换为常见 .NET 类型的 API。 `JsonDocument` 公开了 <xref:System.Text.Json.JsonDocument.RootElement> 属性。

以下部分演示如何使用这些工具读取和写入 JSON。

## <a name="use-jsondocument-for-access-to-data"></a>使用 JsonDocument 访问数据

下面的示例演示如何使用 <xref:System.Text.Json.JsonDocument> 类来随机访问 JSON 字符串中的数据：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/JsonDocumentDataAccess.cs" id="AverageGrades1":::

前面的代码：

* 假设要分析的 JSON 处于名为 `jsonString` 的字符串中。
* 对 `Students` 数组中具有 `Grade` 属性的对象计算平均成绩。
* 为没有成绩的学生分配默认成绩 70。
* 通过在每次迭代时使 `count` 变量递增来对学生进行计数。 一种替代方法是调用 <xref:System.Text.Json.JsonElement.GetArrayLength%2A>，如以下示例中所示：

  :::code language="csharp" source="snippets/system-text-json-how-to/csharp/JsonDocumentDataAccess.cs" id="AverageGrades2":::

下面是此代码处理的 JSON 示例：

:::code language="json" source="snippets/system-text-json-how-to/csharp/GradesPrettyPrint.json":::

## <a name="use-jsondocument-to-write-json"></a>使用 JsonDocument 写入 JSON

下面的示例演示如何通过 <xref:System.Text.Json.JsonDocument> 写入 JSON：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/JsonDocumentWriteJson.cs" id="Serialize":::

前面的代码：

* 读取 JSON 文件，将数据加载到 `JsonDocument` 中，并将格式化（进行了优质打印的）JSON 写入文件。
* 使用 <xref:System.Text.Json.JsonDocumentOptions> 可指定允许但会忽略输入 JSON 中的注释。
* 完成后，对编写器调用 <xref:System.Text.Json.Utf8JsonWriter.Flush%2A>。 一种替代方法是让编写器在释放时自动刷新。

下面是要由示例代码处理的 JSON 输入的示例：

:::code language="json" source="snippets/system-text-json-how-to/csharp/Grades.json":::

结果是以下进行了优质打印的 JSON 输出：

:::code language="json" source="snippets/system-text-json-how-to/csharp/GradesPrettyPrint.json":::

## <a name="use-utf8jsonwriter"></a>使用 Utf8JsonWriter

下面的示例演示如何使用 <xref:System.Text.Json.Utf8JsonWriter> 类：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/Utf8WriterToStream.cs" id="Serialize":::

## <a name="use-utf8jsonreader"></a>使用 Utf8JsonReader

下面的示例演示如何使用 <xref:System.Text.Json.Utf8JsonReader> 类：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/Utf8ReaderFromBytes.cs" id="Deserialize":::

前面的代码假设 `jsonUtf8` 变量是包含有效 JSON（编码为 UTF-8）的字节数组。

### <a name="filter-data-using-utf8jsonreader"></a>使用 Utf8JsonReader 筛选数据

下面的示例演示如何同步读取文件并搜索值。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/Utf8ReaderFromFile.cs":::

若要获取此示例的异步版本，请参阅 [.NET 示例 JSON 项目](https://github.com/dotnet/samples/blob/18e31a5f1abd4f347bf96bfdc3e40e2cfb36e319/core/json/Program.cs)。

前面的代码：

* 假设 JSON 包含一个对象数组，并且每个对象都可能包含一个字符串类型的“name”属性。
* 对对象以及以“University”结尾的属性值进行计数。
* 假设文件编码为 UTF-16，并将它转码为 UTF-8。 可以使用以下代码，将编码为 UTF-8 的文件直接读入 `ReadOnlySpan<byte>`：

  ```csharp
  ReadOnlySpan<byte> jsonReadOnlySpan = File.ReadAllBytes(fileName);
  ```

  如果文件包含 UTF-8 字节顺序标记 (BOM)，请在将字节传递给 `Utf8JsonReader` 之前将它删除，因为读取器需要文本。 否则，BOM 被视为无效 JSON，读取器将引发异常。

下面是前面的代码可以读取的 JSON 示例。 生成的摘要消息为“2 out of 4 have names that end with 'University'”：

:::code language="json" source="snippets/system-text-json-how-to/csharp/Universities.json":::

### <a name="read-from-a-stream-using-utf8jsonreader"></a>使用 Utf8JsonReader 从流中读取

当读取大型文件（例如，1 GB 或更大的文件）时，你可能希望避免一次性将整个文件加载到内存中。 此时，可以使用 <xref:System.IO.FileStream>。

使用 `Utf8JsonReader` 从流中读取时，适用以下规则：

* 包含部分 JSON 有效负载的缓冲区必须至少与其中的最大 JSON 令牌一样大，以便读取器可以推进进度。
* 缓冲区的大小必须至少与 JSON 中的最大空格序列一样大。
* 读取器不会跟踪已读取的数据，直到它完全读取 JSON 有效负载中的下一个 <xref:System.Text.Json.Utf8JsonReader.TokenType%2A>。 因此，当缓冲区中有剩余字节时，必须再次将它们传递给读取器。 你可以使用 <xref:System.Text.Json.Utf8JsonReader.BytesConsumed%2A> 来确定剩余的字节数。

下面的代码演示了如何从流中读取。 本示例显示了 <xref:System.IO.MemoryStream>。 类似的代码将使用 <xref:System.IO.FileStream>，当 `FileStream` 在开头包含 UTF-8 BOM 时除外。 在这种情况下，需要先从缓冲区中去除这三个字节，然后再将剩余字节传递到 `Utf8JsonReader`。 否则，读取器将引发异常，因为 BOM 不被视为 JSON 的有效部分。

示例代码从 4 KB 缓冲区开始，每当发现大小不足以容纳完整的 JSON 令牌（必须容纳完整的令牌，读取器才能推动处理 JSON 有效负载）时，就会使缓冲区大小成倍增加。 仅当设置的初始缓冲区非常小（例如 10 个字节）时，代码片段中提供的 JSON 示例才会触发缓冲区大小增加。 如果将初始缓冲区大小设置为 10，则 `Console.WriteLine` 语句会说明缓冲区大小增加的原因和影响。 在初始缓冲区大小为 4KB 时，每个 `Console.WriteLine` 都会显示整个示例 JSON，并且不必增加缓冲区大小。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/Utf8ReaderPartialRead.cs":::

前面的示例未对缓冲区大小的增长设置任何限制。 如果令牌大小太大，则代码可能会失败，并出现 <xref:System.OutOfMemoryException> 异常。 如果 JSON 包含大小约为 1 GB 或更大的令牌，则会发生这种情况，因为将 1 GB 大小加倍会导致令牌太大，无法放入 `int32` 缓冲区。

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何自定义字符编码](system-text-json-character-encoding.md)
* [如何编写用于 JSON 序列化的自定义转换器](system-text-json-converters-how-to.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
