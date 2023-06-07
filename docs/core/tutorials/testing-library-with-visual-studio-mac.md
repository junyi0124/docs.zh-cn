---
title: 使用 Visual Studio for Mac 测试 .NET 类库
description: 为 .NET 类库创建单元测试项目。 验证 .NET 类库能否正确地进行单元测试。
ms.date: 11/18/2020
ms.openlocfilehash: 02d5aa74258ec15c5447b23246a3c7e9c61a6760
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599521"
---
# <a name="test-a-net-class-library-using-visual-studio"></a>使用 Visual Studio 测试 .NET 类库

本教程演示如何通过将测试项目添加到解决方案来自动执行单元测试。

## <a name="prerequisites"></a>先决条件

- 本教程适用于在[使用 Visual Studio for Mac 创建 .NET 类库](library-with-visual-studio-mac.md)中创建的解决方案。

## <a name="create-a-unit-test-project"></a>创建单元测试项目

单元测试在开发和发布期间提供自动化的软件测试。 [MSTest](https://github.com/Microsoft/testfx-docs) 是可供选择的三个测试框架之一。 其他两个是 [xUnit](https://xunit.net/) 和 [nUnit](https://nunit.org/)。

1. 启动 Visual Studio for Mac。

1. 打开在[使用 Visual Studio for Mac 创建 .NET 类库](library-with-visual-studio-mac.md)中创建的 `ClassLibraryProjects` 解决方案。

1. 在“解决方案”边栏中，按住 <kbd>Ctrl</kbd> 并单击 `ClassLibraryProjects` 解决方案，然后选择“添加” > “新建项目”。

1. 在“新建项目”对话框中，选择“Web 和控制台”节点中的“测试”。 依次选择“MSTest 项目”、“下一步”。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-project.png" alt-text="创建测试项目的 Visual Studio Mac“新建项目”对话框":::

1. 选择“.NET Core 5.0”作为“目标框架”，然后选择“下一步”。

1. 将新项目命名为"StringLibraryTest"，然后选择“创建”。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-new-project-name.png" alt-text="提供项目名称的 Visual Studio Mac“新建项目”对话框":::

   Visual Studio 使用以下代码创建类文件：

   ```csharp
   using Microsoft.VisualStudio.TestTools.UnitTesting;

   namespace StringLibraryTest
   {
       [TestClass]
       public class UnitTest1
       {
           [TestMethod]
           public void TestMethod1()
           {
           }
       }
   }
   ```

   单元测试模板创建的源代码负责执行以下操作：

   - 它会导入 <xref:Microsoft.VisualStudio.TestTools.UnitTesting?displayProperty=nameWithType> 命名空间，其中包含用于单元测试的类型。
   - 向 `UnitTest1` 类应用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 特性。
   - 它向 `TestMethod1` 应用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 特性。

   使用 [[TestClass]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) 标记的测试类中标记有 [[TestMethod]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) 的所有测试方法都会在单元测试运行时自动执行。

## <a name="add-a-project-reference"></a>添加项目引用

对于要使用 `StringLibrary` 类的测试库，请将引用添加到 `StringLibrary` 项目中。

1. 在“解决方案”边栏中，按住 <kbd>Ctrl</kbd> 并单击“StringLibraryTest”下的“依赖项”  。 从上下文菜单中选择“添加引用”。

1. 在“引用”对话框中，选择“StringLibrary”项目 。 选择“确定”。

      :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-edit-references.png" alt-text="Visual Studio Mac“编辑引用”对话框":::

## <a name="add-and-run-unit-test-methods"></a>添加并运行单元测试方法

运行单元测试时，Visual Studio 执行使用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 特性标记的类中标记有 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 特性的所有方法。 当第一次遇到测试不通过或测试方法中的所有测试均已成功通过时，测试方法终止。

最常见的测试调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 类的成员。 许多断言方法至少包含两个参数，其中一个是预期的测试结果，另一个是实际的测试结果。 下表显示了 `Assert` 类最常调用的一些方法：

| 断言方法     | 函数 |
| ------------------ | -------- |
| `Assert.AreEqual`  | 验证两个值或对象是否相等。 如果值或对象不相等，则断言失败。 |
| `Assert.AreSame`   | 验证两个对象变量引用的是否是同一个对象。 如果这些变量引用不同的对象，则断言失败。 |
| `Assert.IsFalse`   | 验证条件是否为 `false`。 如果条件为 `true`，则断言失败。 |
| `Assert.IsNotNull` | 验证对象是否不为 `null`。 如果对象为 `null`，则断言失败。 |

还可以在测试方法中使用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.ThrowsException%2A?displayProperty=nameWithType> 方法来指示它应引发的异常的类型。 如果未引发指定异常，则测试不通过。

测试 `StringLibrary.StartsWithUpper` 方法时，需要提供许多以大写字符开头的字符串。 在这种情况下，此方法应返回 `true`，以便可以调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsTrue%2A?displayProperty=nameWithType> 方法。 同样，需要提供许多以非大写字符开头的字符串。 在这种情况下，此方法应返回 `false`，以便可以调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsFalse%2A?displayProperty=nameWithType> 方法。

由于库方法处理的是字符串，因此还需要确保它能够成功处理[空字符串 (`String.Empty`)](xref:System.String.Empty)（不含字符且 <xref:System.String.Length> 为 0 的有效字符串）和 `null` 字符串（尚未初始化的字符串）。 可以直接将 `StartsWithUpper` 作为静态方法进行调用，并向其传递一个 <xref:System.String> 自变量。 或者，可以对分配给 `null` 的 `string` 变量将 `StartsWithUpper` 作为扩展方法进行调用。

将定义三个方法，每个方法都会对字符串数组中的各个元素调用它的 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 方法。 你将调用方法重载，以便指定在测试失败时要显示的错误消息。 消息标识导致失败的字符串。

创建测试方法：

1. 打开 UnitTest1.cs 文件，并将代码替换为以下代码：

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibraryTest/UnitTest1.cs":::

   `TestStartsWithUpper` 方法中的大写字符的测试包括希腊文大写字母 alpha (U+0391) 和西里尔文大写字母 EM (U+041C)。 `TestDoesNotStartWithUpper` 方法中的小写字符的测试包括希腊文小写字母 alpha (U+03B1) 和西里尔文小写字母 Ghe (U+0433)。

1. 在菜单栏中，选择“文件” > “另存为” 。 在对话框中，确保“编码”设置为“Unicode (UTF-8)” 。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/save-file-as-dialog.png" alt-text="Visual Studio“文件另存为”对话框":::

1. 当系统询问你是否要替换现有文件时，请选择“替换”。

   如果无法将源代码保存为 UTF8 编码文件，Visual Studio 可能会将其另存为 ASCII 文件。 在这种情况下，运行时将无法准确解码 ASCII 范围以外的 UTF8 字符，且测试结果也会不正确。

1. 打开屏幕右侧的“单元测试”面板。 从菜单中选择“查看” > “测试”。

1. 单击“停靠”图标使此面板保持打开状态。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-dock-icon.png" alt-text="Visual Studio for Mac“单元测试”面板停靠图标":::

1. 单击“全部运行”按钮。

   所有测试通过。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-pass.png" alt-text="Visual Studio for Mac 预期测试通过":::

## <a name="handle-test-failures"></a>处理测试失败

如果进行的是测试驱动开发 (TDD)，请先编写测试，然后测试会在第一次运行时失败。 接着将可以使测试成功的代码添加到应用。 在本教程中，先编写了测试要验证的应用代码然后才创建测试，所以没有看到测试失败。 若要验证测试是否在预期失败时失败，请在测试输入中添加无效值。

1. 通过修改 `TestDoesNotStartWithUpper` 方法中的 `words` 数组来包含字符串“Error”。 由于 Visual Studio 将在生成运行测试的解决方案时自动保存打开的文件，因此无需手动保存。

   ```csharp
   string[] words = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " };
   ```

1. 重新运行测试。

   这一次，“测试资源管理器”窗口指示有两个测试成功，还有一个失败。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/failed-test-window.png" alt-text="未通过测试的“测试资源管理器”窗口":::

1. 按 <kbd>Ctrl</kbd> 并单击失败的测试 `TestDoesNotStartWithUpper`，然后从上下文菜单中选择“显示结果边栏”。

   “结果”边栏显示断言生成的消息：“Assert.IsFalse 失败。 “Error”应返回 false；实际返回 True”。 由于此次失败，数组中“Error”之后的所有字符串都未进行测试。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-mac-unit-test-failure.png" alt-text="显示 IsFalse 断言失败的“测试资源管理器”窗口":::

1. 删除在步骤 1 中添加的字符串“Error”。 重新运行测试，测试将通过。

## <a name="test-the-release-version-of-the-library"></a>测试库的发行版本

至此，在运行库的调试版本时，测试已全部通过，接下来应对库的发行版本再运行一次这些测试。 许多因素（包括编译器优化）有时可能会导致调试版本和发行版本出现行为差异。

若要测试发行版本，请执行以下操作：

1. 在 Visual Studio 工具栏中，将生成配置从 **“调试”** 更改为 **“发行”** 。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/visual-studio-toolbar-release.png" alt-text="突出显示发布版本的 Visual Studio 工具栏":::

1. 在“解决方案”边栏中，按 <kbd>Ctrl</kbd> 并单击“StringLibrary”项目，然后从上下文菜单中选择“生成”，重新编译库。

   :::image type="content" source="media/testing-library-with-visual-studio-mac/build-library-context-menu.png" alt-text="带有生成命令的 StringLibrary 上下文菜单":::

1. 再次运行单元测试。

   测试通过。

## <a name="debug-tests"></a>调试测试

如果使用 Visual Studio for Mac 作为 IDE，可以使用[教程：使用 Visual Studio for Mac 调试 .NET 控制台应用程序](debugging-with-visual-studio-mac.md)中所示的相同过程，使用单元测试项目调试代码。 按住 <kbd>Ctrl</kbd> 并单击“StringLibraryTests”项目，然后从上下文菜单中选择“启动调试项目” ，而不是启动 ShowCase 应用项目。

Visual Studio 启动附有调试器的测试项目。 执行将在添加到测试项目的任何断点或基础库代码处停止。

## <a name="additional-resources"></a>其他资源

* [.NET 中的单元测试](../testing/index.md)

## <a name="next-steps"></a>后续步骤

在本教程中，你对类库进行了单元测试。 你可以将库作为包发布到 [NuGet](https://nuget.org)，使其可供其他人使用。 若要了解如何操作，请遵循 NuGet 教程：

> [!div class="nextstepaction"]
> [创建并发布包 (dotnet CLI)](/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)

如果将库作为 NuGet 包发布，其他人可以安装并使用它。 若要了解如何操作，请遵循 NuGet 教程：

> [!div class="nextstepaction"]
> [在 Visual Studio for Mac 中安装和使用包](/nuget/quickstart/install-and-use-a-package-in-visual-studio-mac)

库并非必须作为包进行分发。 它还可与使用它的控制台应用捆绑在一起。 若要了解如何发布控制台应用，请参阅本系列中前面的教程：

> [!div class="nextstepaction"]
> [使用 Visual Studio for Mac 发布 .NET 控制台应用程序](publishing-with-visual-studio-mac.md)
