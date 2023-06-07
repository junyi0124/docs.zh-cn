---
title: 运行选择性单元测试
description: 如何使用筛选表达式通过 .NET Core 中的 dotnet 测试命令运行选择性单元测试。
author: smadala
ms.date: 05/18/2020
zone_pivot_groups: unit-testing-framework-set-one
ms.openlocfilehash: 6a6bbb0687742d1e3288d64fb88f6825dc678e28
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2020
ms.locfileid: "83702989"
---
# <a name="run-selective-unit-tests"></a>运行选择性单元测试

借助 .NET Core 中的 [`dotnet test`](../tools/dotnet-test.md) 命令，可以使用筛选表达式来运行选择性测试。 本文演示如何筛选运行哪些测试。 下面的示例使用 `dotnet test`。 如果使用的是 `vstest.console.exe`，请将 `--filter` 替换成 `--testcasefilter:`。

## <a name="character-escaping"></a>字符转义

在 `*nix` 上使用包含感叹号 `!` 的筛选器需要转义，因为保留了 `!`。 例如，如果命名空间包含 IntegrationTests，则此筛选器将跳过所有测试：

```dotnetcli
dotnet test --filter FullyQualifiedName\!~IntegrationTests
```

> [!IMPORTANT]
> 反斜杠位于感叹号之前，表示它是转义字符 `\!`。

对于包含泛型类型参数的逗号的 `FullyQualifiedName` 值，请使用 `%2C` 来转义逗号。 例如：

```dotnetcli
dotnet test --filter "FullyQualifiedName=MyNamespace.MyTestsClass<ParameterType1%2CParameterType2>.MyTestMethod"
```

:::zone pivot="mstest"

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace MSTestNamespace
{
    [TestClass]
    public class UnitTest1
    {
        [TestMethod, Priority(1), TestCategory("CategoryA")]
        public void TestMethod1()
        {
        }

        [TestMethod, Priority(2)]
        public void TestMethod2()
        {
        }
    }
}
```

| 表达式 | 结果 |
|--|--|
| `dotnet test --filter Method` | 运行 <xref:System.Reflection.Module.FullyQualifiedName> 包含 `Method` 的测试。 在 `vstest 15.1+` 中可用。 |
| `dotnet test --filter Name~TestMethod1` | 运行名称包含 `TestMethod1` 的测试。 |
| `dotnet test --filter ClassName=MSTestNamespace.UnitTest1` | 运行属于类 `MSTestNamespace.UnitTest1` 的测试。<br>**注意：** 由于 `ClassName` 值应有命名空间，因此 `ClassName=UnitTest1` 无效。 |
| `dotnet test --filter FullyQualifiedName!=MSTestNamespace.UnitTest1.TestMethod1` | 运行除 `MSTestNamespace.UnitTest1.TestMethod1` 之外的其他所有测试。 |
| `dotnet test --filter TestCategory=CategoryA` | 运行含 `[TestCategory("CategoryA")]` 批注的测试。 |
| `dotnet test --filter Priority=2` | 运行含 `[Priority(2)]` 批注的测试。 |

使用条件运算符 `|` 和 `&` 的示例：

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `UnitTest1` 或 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCategoryAttribute> 为 `"CategoryA"` 的测试。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1|TestCategory=CategoryA"
```

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `UnitTest1` 且 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCategoryAttribute> 为 `"CategoryA"` 的测试。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1&TestCategory=CategoryA"
```

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `UnitTest1` 且 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestCategoryAttribute> 为 `"CategoryA"` 或 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.PriorityAttribute> 的优先级为 `1` 的测试。

```dotnetcli
dotnet test --filter "(FullyQualifiedName~UnitTest1&TestCategory=CategoryA)|Priority=1"
```

:::zone-end
:::zone pivot="xunit"

```csharp
using Xunit;

namespace XUnitNamespace
{
    public class TestClass1
    {
        [Fact, Trait("Priority", "1"), Trait("Category", "CategoryA")]
        public void Test1()
        {
        }

        [Fact, Trait("Priority", "2")]
        public void Test2()
        {
        }
    }
}
```

| 表达式 | 结果 |
|--|--|
| `dotnet test --filter DisplayName=XUnitNamespace.TestClass1.Test1` | 仅运行一个测试，即 `XUnitNamespace.TestClass1.Test1`。 |
| `dotnet test --filter FullyQualifiedName!=XUnitNamespace.TestClass1.Test1` | 运行除 `XUnitNamespace.TestClass1.Test1` 之外的其他所有测试。 |
| `dotnet test --filter DisplayName~TestClass1` | 运行显示名称包含 `TestClass1` 的测试。 |

在代码示例中，包含键 `"Category"` 和 `"Priority"` 的已定义特征可用于筛选。

| 表达式 | 结果 |
|--|--|
| `dotnet test --filter XUnit` | 运行 <xref:System.Reflection.Module.FullyQualifiedName> 包含 `XUnit` 的测试。  在 `vstest 15.1+` 中可用。 |
| `dotnet test --filter Category=CategoryA` | 运行包含 `[Trait("Category", "CategoryA")]` 的测试。 |

使用条件运算符 `|` 和 `&` 的示例：

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `TestClass1` 或 `Trait` 的键为 `"Category"` 且值为 `"CategoryA"` 的测试。

```dotnetcli
dotnet test --filter "FullyQualifiedName~TestClass1|Category=CategoryA"
```

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `TestClass1` 且 `Trait` 的键为 `"Category"` 且值为 `"CategoryA"` 的测试。

```dotnetcli
dotnet test --filter "FullyQualifiedName~TestClass1&Category=CategoryA"
```

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `TestClass1` 且 `Trait` 的键为 `"Category"` 且值为 `"CategoryA"` 或 `Trait` 的键为 `"Priority"` 且值为 `1` 的测试。

```dotnetcli
dotnet test --filter "(FullyQualifiedName~TestClass1&Category=CategoryA)|Priority=1"
```

:::zone-end
:::zone pivot="nunit"

```csharp
using NUnit.Framework;

namespace NUnitNamespace
{
    public class UnitTest1
    {
        [Test, Property("Priority", 1), Category("CategoryA")]
        public void TestMethod1()
        {
        }

        [Test, Property("Priority", 2)]
        public void TestMethod2()
        {
        }
    }
}
```

| 表达式 | 结果 |
|--|--|
| `dotnet test --filter Method` | 运行 <xref:System.Reflection.Module.FullyQualifiedName> 包含 `Method` 的测试。 在 `vstest 15.1+` 中可用。 |
| `dotnet test --filter Name~TestMethod1` | 运行名称包含 `TestMethod1` 的测试。 |
| `dotnet test --filter FullyQualifiedName~NUnitNamespace.UnitTest1` | 运行属于类 `NUnitNamespace.UnitTest1` 的测试。 |
| `dotnet test --filter FullyQualifiedName!=NUnitNamespace.UnitTest1.TestMethod1` | 运行除 `NUnitNamespace.UnitTest1.TestMethod1` 之外的其他所有测试。 |
| `dotnet test --filter TestCategory=CategoryA` | 运行含 `[Category("CategoryA")]` 批注的测试。 |
| `dotnet test --filter Priority=2` | 运行含 `[Priority(2)]` 批注的测试。 |

使用条件运算符 `|` 和 `&` 的示例：

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `UnitTest1` 或 `Category` 为 `"CategoryA"` 的测试。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1|TestCategory=CategoryA"
```

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `UnitTest1` 且 `Category` 为 `"CategoryA"` 的测试。

```dotnetcli
dotnet test --filter "FullyQualifiedName~UnitTest1&TestCategory=CategoryA"
```

运行 <xref:System.Reflection.Module.FullyQualifiedName> 中包含 `UnitTest1` 且 `Category` 为 `"CategoryA"` 或 `Property` 的 `"Priority"` 为 `1` 的测试。

```dotnetcli
dotnet test --filter "(FullyQualifiedName~UnitTest1&TestCategory=CategoryA)|Priority=1"
```

有关详细信息，请参阅 [TestCase 筛选器](https://github.com/Microsoft/vstest-docs/blob/master/docs/filter.md)

:::zone-end

## <a name="see-also"></a>请参阅

- [dotnet test](../tools/dotnet-test.md)
- [dotnet test -- 筛选器](../tools/dotnet-test.md#filter-option-details)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [对单元测试排序](order-unit-tests.md)
