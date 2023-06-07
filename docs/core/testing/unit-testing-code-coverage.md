---
title: 将代码覆盖率用于单元测试
description: 了解如何将代码覆盖率功能用于 .NET 单元测试。
author: IEvangelist
ms.author: dapine
ms.date: 07/01/2020
ms.openlocfilehash: 4d2c8f3db26eaabcb973378a349ef57912e92bfa
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90538142"
---
# <a name="use-code-coverage-for-unit-testing"></a>将代码覆盖率用于单元测试

单元测试有助于确保功能的正常运行，并为重构工作提供一种验证方法。 代码覆盖率是单元测试运行的代码量（行、分支或方法）的度量值。 例如，如果你有一个简单的应用程序，其中只有两个条件分支（分支 a 和分支 b ），则验证条件分支 a 的单元测试将报告 50% 的分支代码覆盖率。

本文介绍如何通过 Coverlet 在单元测试中使用代码覆盖率和使用 ReportGenerator 生成报表。 尽管本文重点介绍 C# 和 xUnit 作为测试框架，但 MSTest 和 NUnit 也适用。 Coverlet 是 [GitHub 上的开源项目](https://github.com/coverlet-coverage/coverlet)，可为 C# 提供跨平台代码覆盖率框架。 [Coverlet](https://dotnetfoundation.org/projects/coverlet) 是 .NET Foundation 的一部分。 Coverlet 收集 Cobertura 覆盖率测试运行数据，用于生成报表。

此外，本文详细介绍如何使用从 Coverlet 测试运行收集的代码覆盖率信息来生成报表。 可以使用另一个 [GitHub 上的开源项目 - ReportGenerator](https://github.com/danielpalme/ReportGenerator) 来生成报表。 ReportGenerator 将由 Cobertura 生成的覆盖率报表转换为各种格式的用户可读的报表。

本文基于示例浏览器中提供的[示例源代码项目](/samples/dotnet/samples/unit-testing-code-coverage-cs)。

## <a name="system-under-test"></a>测试中的系统

“测试中的系统”指的是要对其编写单元测试的代码，这可能是对象、服务或其他任何公开可测试功能的内容。 为了本文的目的，你将创建一个类库（它将成为测试中的系统），以及两个对应的单元测试项目。

### <a name="create-a-class-library"></a>创建类库

在名为 `UnitTestingCodeCoverage` 的新目录中的命令提示符下，使用 [`dotnet new classlib`](../tools/dotnet-new.md#classlib) 命令创建新的 .NET 标准类库：

```dotnetcli
dotnet new classlib -n Numbers
```

下面的代码段定义了一个简单的 `PrimeService` 类，该类提供了用于检查数值是否为质数的功能。 复制下面的代码片段，并替换在“编号”目录中自动创建的“Class1.cs”文件的内容 。 将“Class1.cs”文件重命名为“PrimeService.cs” 。

```csharp
namespace System.Numbers
{
    public class PrimeService
    {
        public bool IsPrime(int candidate)
        {
            if (candidate < 2)
            {
                return false;
            }

            for (int divisor = 2; divisor <= Math.Sqrt(candidate); ++divisor)
            {
                if (candidate % divisor == 0)
                {
                    return false;
                }
            }
            return true;
        }
    }
}
```

> [!TIP]
> 值得一提的是，`Numbers` 类库是有意添加到 `System` 命名空间中的。 因此，无需 `using System;` 命名空间声明即可访问 <xref:System.Math?displayProperty=fullName>。 有关详细信息，请参阅[命名空间（C# 参考）](../../csharp/language-reference/keywords/namespace.md)。

### <a name="create-test-projects"></a>创建测试项目

使用 [`dotnet new xunit`](../tools/dotnet-new.md#test) 命令，在同一命令提示符下创建两个新的“xUnit 测试项目(.NET Core)”模板：

```dotnetcli
dotnet new xunit -n XUnit.Coverlet.Collector
```

```dotnetcli
dotnet new xunit -n XUnit.Coverlet.MSBuild
```

这两个新创建的 xUnit 测试项目都需要添加 Numbers 类库的项目引用。 这是为了使测试项目有权访问 PrimeService 以便进行测试。 在命令提示符下，使用 [`dotnet add`](../tools/dotnet-add-reference.md) 命令：

```dotnetcli
dotnet add XUnit.Coverlet.Collector\XUnit.Coverlet.Collector.csproj reference Numbers\Numbers.csproj
```

```dotnetcli
dotnet add XUnit.Coverlet.MSBuild\XUnit.Coverlet.MSBuild.csproj reference Numbers\Numbers.csproj
```

MSBuild 项目命名正确，因为它依赖于 [coverlet.msbuild](https://www.nuget.org/packages/coverlet.msbuild) NuGet 包。 通过运行 [`dotnet add package`](../tools/dotnet-add-package.md) 命令添加此包依赖项：

```dotnetcli
cd XUnit.Coverlet.MSBuild && dotnet add package coverlet.msbuild && cd ..
```

之前的命令更改了有效作用于 MSBuild 测试项目的目录，然后添加了 NuGet 包。 完成此操作后，它会更改目录，向上执行一个级别。

打开两个 UnitTest1.cs 文件，并将其内容替换为以下代码片段。 将 UnitTest1.cs 文件重命名为 PrimeServiceTests.cs 。

```csharp
using System.Numbers;
using Xunit;

namespace XUnit.Coverlet
{
    public class PrimeServiceTests
    {
        readonly PrimeService _primeService;

        public PrimeServiceTests() => _primeService = new PrimeService();

        [
            Theory,
            InlineData(-1), InlineData(0), InlineData(1)
        ]
        public void IsPrime_ValuesLessThan2_ReturnFalse(int value) =>
            Assert.False(_primeService.IsPrime(value), $"{value} should not be prime");

        [
            Theory,
            InlineData(2), InlineData(3), InlineData(5), InlineData(7)
        ]
        public void IsPrime_PrimesLessThan10_ReturnTrue(int value) =>
            Assert.True(_primeService.IsPrime(value), $"{value} should be prime");

        [
            Theory,
            InlineData(4), InlineData(6), InlineData(8), InlineData(9)
        ]
        public void IsPrime_NonPrimesLessThan10_ReturnFalse(int value) =>
            Assert.False(_primeService.IsPrime(value), $"{value} should not be prime");
    }
}
```

### <a name="create-a-solution"></a>创建解决方案

在命令提示符下，创建一个用于封装类库和两个测试项目的新解决方案。 使用 [`dotnet sln`](../tools/dotnet-sln.md) 命令：

```dotnetcli
dotnet new sln -n XUnit.Coverage
```

这会在 UnitTestingCodeCoverage 目录中创建新的解决方案文件名 `XUnit.Coverage` 。 将项目添加到解决方案的根。

## <a name="linux"></a>[Linux](#tab/linux)

```dotnetcli
dotnet sln XUnit.Coverage.sln add **/*.csproj --in-root
```

## <a name="windows"></a>[Windows](#tab/windows)

```dotnetcli
dotnet sln XUnit.Coverage.sln add (ls **/*.csproj) --in-root
```

---

使用 [`dotnet build`](../tools/dotnet-build.md) 命令生成解决方案：

```dotnetcli
dotnet build
```

如果生成成功，则已创建了三个项目，正确引用了项目和包，并正确更新了源代码。 做得不错！

## <a name="tooling"></a>工具

代码覆盖率工具有两种类型：

- **数据收集器：** 数据收集器监视测试执行并收集有关测试运行的信息。 它们以各种输出格式（例如 XML 和 JSON）报告收集的信息。 有关详细信息，请参阅[第一个数据收集器](https://github.com/Microsoft/vstest-docs/blob/master/docs/extensions/datacollector.md)。
- **报表生成器：** 使用从测试运行收集的数据生成报表，通常为带样式的 HTML。

本部分重点介绍数据收集器工具。 若要通过 Coverlet 获得代码覆盖率，现有单元测试项目必须具有相应的包依赖项，或者依赖于 [.NET 全局工具](../tools/global-tools.md)和对应的 [coverlet.console](https://www.nuget.org/packages/coverlet.console) NuGet 包。

## <a name="integrate-with-net-test"></a>与 .NET 测试集成

默认情况下，xUnit 测试项目模板已与 [coverlet.collector](https://www.nuget.org/packages/coverlet.collector) 集成。
在命令提示符下，将目录更改为 XUnit.Coverlet.Collector 项目，并运行 [`dotnet test`](../tools/dotnet-test.md) 命令：

```dotnetcli
cd XUnit.Coverlet.Collector && dotnet test --collect:"XPlat Code Coverage"
```

> [!NOTE]
> `"XPlat Code Coverage"` 参数是与 Coverlet 中的数据收集器对应的易记名称。 此名称是必需的，但不区分大小写。

作为 `dotnet test` 运行的一部分，生成的 coverage.cobertura.xml 文件输出到 TestResults 目录 。 该 XML 文件包含结果。 这是一个依赖于 .NET Core CLI 的跨平台选项，非常适用于不可使用 MSBuild 的生成系统。

下面是 coverage.cobertura.xml 文件的示例 。

```xml
<?xml version="1.0" encoding="utf-8"?>
<coverage line-rate="1" branch-rate="1" version="1.9" timestamp="1592248008"
          lines-covered="12" lines-valid="12" branches-covered="6" branches-valid="6">
  <sources>
    <source>C:\</source>
  </sources>
  <packages>
    <package name="Numbers" line-rate="1" branch-rate="1" complexity="6">
      <classes>
        <class name="Numbers.PrimeService" line-rate="1" branch-rate="1" complexity="6"
               filename="Numbers\PrimeService.cs">
          <methods>
            <method name="IsPrime" signature="(System.Int32)" line-rate="1"
                    branch-rate="1" complexity="6">
              <lines>
                <line number="8" hits="11" branch="False" />
                <line number="9" hits="11" branch="True" condition-coverage="100% (2/2)">
                  <conditions>
                    <condition number="7" type="jump" coverage="100%" />
                  </conditions>
                </line>
                <line number="10" hits="3" branch="False" />
                <line number="11" hits="3" branch="False" />
                <line number="14" hits="22" branch="True" condition-coverage="100% (2/2)">
                  <conditions>
                    <condition number="57" type="jump" coverage="100%" />
                  </conditions>
                </line>
                <line number="15" hits="7" branch="False" />
                <line number="16" hits="7" branch="True" condition-coverage="100% (2/2)">
                  <conditions>
                    <condition number="27" type="jump" coverage="100%" />
                  </conditions>
                </line>
                <line number="17" hits="4" branch="False" />
                <line number="18" hits="4" branch="False" />
                <line number="20" hits="3" branch="False" />
                <line number="21" hits="4" branch="False" />
                <line number="23" hits="11" branch="False" />
              </lines>
            </method>
          </methods>
          <lines>
            <line number="8" hits="11" branch="False" />
            <line number="9" hits="11" branch="True" condition-coverage="100% (2/2)">
              <conditions>
                <condition number="7" type="jump" coverage="100%" />
              </conditions>
            </line>
            <line number="10" hits="3" branch="False" />
            <line number="11" hits="3" branch="False" />
            <line number="14" hits="22" branch="True" condition-coverage="100% (2/2)">
              <conditions>
                <condition number="57" type="jump" coverage="100%" />
              </conditions>
            </line>
            <line number="15" hits="7" branch="False" />
            <line number="16" hits="7" branch="True" condition-coverage="100% (2/2)">
              <conditions>
                <condition number="27" type="jump" coverage="100%" />
              </conditions>
            </line>
            <line number="17" hits="4" branch="False" />
            <line number="18" hits="4" branch="False" />
            <line number="20" hits="3" branch="False" />
            <line number="21" hits="4" branch="False" />
            <line number="23" hits="11" branch="False" />
          </lines>
        </class>
      </classes>
    </package>
  </packages>
</coverage>
```

> [!TIP]
> 有一种替代方法：如果生成系统已使用 MSBuild，则你可以使用 MSBuild 包。 在命令提示符下，将目录更改为 XUnit.Coverlet.MSBuild 项目，并运行 `dotnet test` 命令：
>
> ```dotnetcli
> dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=cobertura
> ```
>
> 生成的 coverage.cobertura.xml 文件为输出。  
> 可按照[此处](https://github.com/coverlet-coverage/coverlet/blob/master/Documentation/MSBuildIntegration.md)的 msbuild 集成指南操作

## <a name="generate-reports"></a>生成报告

现在，你既可从单元测试运行收集数据，就可以使用 [ReportGenerator](https://github.com/danielpalme/ReportGenerator) 来生成报表。 若要将 [ReportGenerator](https://www.nuget.org/packages/dotnet-reportgenerator-globaltool) NuGet 包安装为 [.NET 全局工具](../tools/global-tools.md)，请使用 [`dotnet tool install`](../tools/dotnet-tool-install.md) 命令：

```dotnetcli
dotnet tool install -g dotnet-reportgenerator-globaltool
```

给定先前的测试运行得到的输出 coverage.cobertura.xml 文件，运行该工具并提供所需的选项。

```console
reportgenerator
"-reports:Path\To\TestProject\TestResults\{guid}\coverage.cobertura.xml"
"-targetdir:coveragereport"
-reporttypes:Html
```

运行此命令后，HTML 文件表示生成的报表。

:::image type="content" source="media/test-report.png" lightbox="media/test-report.png" alt-text="单元测试生成的报表":::

## <a name="see-also"></a>请参阅

- [Visual Studio 单元测试代码覆盖率](/visualstudio/test/using-code-coverage-to-determine-how-much-code-is-being-tested)
- [GitHub - Coverlet 存储库](https://github.com/coverlet-coverage/coverlet)
- [GitHub - ReportGenerator 存储库](https://github.com/danielpalme/ReportGenerator)
- [ReportGenerator 项目网站](https://danielpalme.github.io/ReportGenerator)
- [.NET Core CLI 测试命令](../tools/dotnet-test.md)
- [示例源代码](/samples/dotnet/samples/unit-testing-code-coverage-cs)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [单元测试最佳做法](unit-testing-best-practices.md)
