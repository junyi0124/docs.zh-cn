---
title: 通过 dotnet vstest 测试已发布的输出
description: 了解如何使用 dotnet vstest 命令在已发布的库上运行测试，而不在源代码上运行测试。
author: kendrahavens
ms.author: kehavens
ms.date: 10/18/2017
ms.openlocfilehash: 7618d37782de3a16f1963380bbb56945fb73e8eb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "75714256"
---
# <a name="test-published-output-with-dotnet-vstest"></a>通过 dotnet vstest 测试已发布的输出

可以使用 `dotnet vstest` 命令测试已发布的输出。 这将适用于 xUnit、MSTest 和 NUnit 测试。 只需找到属于已发布输出的 DLL 文件，然后运行：

```dotnetcli
dotnet vstest <MyPublishedTests>.dll
```

其中，`<MyPublishedTests>` 是已发布的测试项目的名称。

## <a name="example"></a>示例

下面的命令演示在已发布的 DLL 上运行测试。

```dotnetcli
dotnet new mstest -o MyProject.Tests
cd MyProject.Tests
dotnet publish -o out
dotnet vstest out/MyProject.Tests.dll
```

> [!NOTE]
> 注意：如果你的应用面向 `netcoreapp` 之外的框架，仍可通过传入带有框架标志的目标框架来运行 `dotnet vstest` 命令。 例如 `dotnet vstest <MyPublishedTests>.dll --Framework:".NETFramework,Version=v4.6"`。 在 Visual Studio 2017 Update 5 及更高版本中，自动检测所需的框架。

## <a name="see-also"></a>请参阅

- [使用 dotnet 测试和 xUnit 进行单元测试](unit-testing-with-dotnet-test.md)
- [使用 dotnet 测试和 NUnit 进行单元测试](unit-testing-with-nunit.md)
- [使用 dotnet 测试和 MSTest 进行单元测试](unit-testing-with-mstest.md)
