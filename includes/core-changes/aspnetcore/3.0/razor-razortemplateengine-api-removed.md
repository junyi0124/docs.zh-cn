---
ms.openlocfilehash: 35dd8db243b03e1dfd6311195ec97673cf114877
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032301"
---
### <a name="razor-razortemplateengine-api-removed"></a>Razor：已删除 RazorTemplateEngine API

已删除 [RazorTemplateEngine](/dotnet/api/microsoft.aspnetcore.razor.language.razortemplateengine?view=aspnetcore-2.2) API，该内容已替换为 <xref:Microsoft.AspNetCore.Razor.Language.RazorProjectEngine>。

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#25215](https://github.com/dotnet/aspnetcore/issues/25215)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

可创建模板引擎并将其用于分析和生成 Razor 文件的代码。

#### <a name="new-behavior"></a>新行为

可创建 `RazorProjectEngine`，使其提供与 `RazorTemplateEngine` 相同类型的信息，以分析和生成 Razor 文件的代码。 `RazorProjectEngine` 也可提供额外的配置级别。

#### <a name="reason-for-change"></a>更改原因

`RazorTemplateEngine` 与现有实现过于紧密耦合。 在尝试正确配置 Razor 分析/生成管道时，这种紧密耦合会导致更多问题。

#### <a name="recommended-action"></a>建议操作

请使用 `RazorProjectEngine`，而不是 `RazorTemplateEngine`。 请考虑以下示例。

##### <a name="create-and-configure-the-razorprojectengine"></a>创建和配置 RazorProjectEngine

```csharp
RazorProjectEngine projectEngine =
    RazorProjectEngine.Create(RazorConfiguration.Default,
        RazorProjectFileSystem.Create(@"C:\source\repos\ConsoleApp4\ConsoleApp4"),
        builder =>
        {
            builder.ConfigureClass((document, classNode) =>
            {
                classNode.ClassName = "MyClassName";

                // Can also configure other aspects of the class here.
            });

            // More configuration can go here
        });
```

##### <a name="generate-code-for-a-razor-file"></a>生成 Razor 文件的代码

```csharp
RazorProjectItem item = projectEngine.FileSystem.GetItem(
    @"C:\source\repos\ConsoleApp4\ConsoleApp4\Example.cshtml",
    FileKinds.Legacy);
RazorCodeDocument output = projectEngine.Process(item);

// Things available
RazorSyntaxTree syntaxTree = output.GetSyntaxTree();
DocumentIntermediateNode intermediateDocument =
    output.GetDocumentIntermediateNode();
RazorCSharpDocument csharpDocument = output.GetCSharpDocument();
```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- [RazorTemplateEngine](/dotnet/api/microsoft.aspnetcore.razor.language.razortemplateengine?view=aspnetcore-2.2)
- [RazorTemplateEngineOptions](/dotnet/api/microsoft.aspnetcore.razor.language.razortemplateengineoptions?view=aspnetcore-2.2)

<!--

#### Affected APIs

- `T:Microsoft.AspNetCore.Razor.Language.RazorTemplateEngine`
- `T:Microsoft.AspNetCore.Razor.Language.RazorTemplateEngineOptions`

-->
