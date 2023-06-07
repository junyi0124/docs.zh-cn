---
title: 扩展 My 命名空间
ms.date: 07/20/2015
f1_keywords:
- vb.AddingMyExtensions
helpviewer_keywords:
- My namespace [Visual Basic], customizing
- My namespace
- My namespace [Visual Basic], extending
ms.assetid: 808e8617-b01c-4135-8b21-babe87389e8e
ms.openlocfilehash: 2a7b0b84061fccd9a333a68e4a19477bd19ca4ff
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74330317"
---
# <a name="extending-the-my-namespace-in-visual-basic"></a>扩展 Visual Basic 中的 `My` 命名空间

Visual Basic 中的 `My` 命名空间公开使你可以轻松利用 .NET Framework 的强大功能的属性和方法。 `My` 命名空间简化了常见编程问题，通常可将困难任务简化为一行代码。 此外，`My` 命名空间完全可扩展，因此，你可以自定义 `My` 的行为并将新服务添加到其层次结构中，以适应特定应用程序需求。 本主题讨论如何自定义 `My` 命名空间的现有成员以及如何向 `My` 命名空间添加自己的自定义类。

## <a name="customizing-existing-my-namespace-members"></a>自定义现有 `My` 命名空间成员

Visual Basic 中的 `My` 命名空间公开有关应用程序、计算机等的常用信息。 有关 `My` 命名空间中对象的完整列表，请参阅 [My 引用](../../language-reference/keywords/my-reference.md)。 你可能必须自定义 `My` 命名空间的现有成员，以便它们更好地满足你的应用程序的需求。 `My` 命名空间中的对象的所有非只读属性都可以设置为自定义值。

例如，假设你经常为运行应用程序的用户使用 `My.User` 对象访问当前安全上下文。 但是，你的公司使用自定义用户对象为公司内的用户公开其他信息和功能。 在此方案中，你可以使用自己的自定义主体对象的实例替换 `My.User.CurrentPrincipal` 属性的默认值，如以下示例所示：

[!code-vb[VbVbcnExtendingMy#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#1)]

在 `My.User` 对象上设置 `CurrentPrincipal` 属性将更改应用程序运行所依据的身份。 `My.User` 对象反过来会返回有关新指定用户的信息。
  
## <a name="adding-members-to-my-objects"></a>将成员添加到 `My` 对象

从 `My.Application` 和 `My.Computer` 返回的类型定义为 `Partial` 类。 因此，可以通过创建名为 `MyApplication` 或 `MyComputer` 的 `Partial` 类来扩展 `My.Application` 和 `My.Computer` 对象。 该类不能是 `Private` 类。 如果将类指定为 `My` 命名空间的一部分，则可以添加将包含在 `My.Application` 或 `My.Computer` 对象中的属性和方法。

以下示例将名为 `DnsServerIPAddresses` 的属性添加到 `My.Computer` 对象：

[!code-vb[VbVbcnExtendingMy#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class2.vb#2)]

## <a name="adding-custom-objects-to-the-my-namespace"></a>将自定义对象添加到 `My` 命名空间

尽管 `My` 命名空间为许多常见的编程任务提供了解决方案，但你可能会遇到 `My` 命名空间未解决的任务。 例如，你的应用程序可能会访问用于用户数据的自定义目录服务，或者，你的应用程序可能会使用 Visual Basic 未默认安装的程序集。 你可以扩展 `My` 命名空间，以包括针对特定于你的环境的常见任务的自定义解决方案。 `My` 命名空间可以轻松扩展以添加新成员，从而满足不断增长的应用程序需求。 此外，你可以将 `My` 命名空间扩展作为 Visual Basic 模板部署给其他开发人员。
  
### <a name="adding-members-to-the-my-namespace"></a>将成员添加到 `My` 命名空间

因为 `My` 是与任何其他命名空间一样的命名空间，所以可以通过添加模块并指定 `My` 的 `Namespace` 来为其添加顶级属性。 使用 `HideModuleName` 属性注释模块，如下面的示例所示。 `HideModuleName` 属性可确保 IntelliSense 在显示 `My` 命名空间的成员时不显示模块名称。

[!code-vb[VbVbcnExtendingMy#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#3)]

若要将成员添加到 `My` 命名空间，请根据需要向模块添加属性。 对于添加到 `My` 命名空间的每个属性，添加一个类型为 `ThreadSafeObjectProvider(Of T)` 的专用字段，该类型为自定义属性返回的类型。 此字段用于创建线程安全的对象实例，该实例由属性通过调用 `GetInstance` 方法返回。 因此，每个访问扩展属性的线程都会收到其自己的返回类型的实例。 以下示例将名为 `SampleExtension` 的属性（类型为 `SampleExtension`）添加到 `My` 命名空间：

[!code-vb[VbVbcnExtendingMy#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#4)]

## <a name="adding-events-to-custom-my-objects"></a>将事件添加到自定义 `My` 对象

通过扩展 `My` 命名空间中的 `MyApplication` 分部类，可以使用 `My.Application` 对象公开自定义 `My` 对象的事件。 对于基于 Windows 的项目，可以在**解决方案资源管理器**中双击项目的“ My 项目”节点  。 在 Visual Basic **项目设计器**中，单击“应用程序”选项卡，然后单击“查看应用程序事件”按钮   。 将创建一个名为 *ApplicationEvents.vb* 的新文件。 它包含以下用于扩展 `MyApplication` 类的代码：

[!code-vb[VbVbcnExtendingMy#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#5)]

你可以通过向 `MyApplication` 类添加自定义事件处理程序来为自定义 `My` 对象添加事件处理程序。 借助自定义事件，你可以添加将在添加、删除事件处理程序时或引发事件时执行的代码。 请注意，仅当用户添加了用于处理事件的代码时，才会运行用于自定义事件的 `AddHandler` 代码。 例如，假设前面部分中的 `SampleExtension` 对象具有要为其添加自定义事件处理程序的 `Load` 事件。 以下代码示例演示了名为 `SampleExtensionLoad` 的自定义事件处理程序，该事件处理程序将在 `My.SampleExtension.Load` 事件发生时被调用。 添加代码以处理新的 `My.SampleExtensionLoad` 事件时，将执行此自定义事件代码的 `AddHandler` 部分。 代码示例中包含 `MyApplication_SampleExtensionLoad` 方法，以显示处理 `My.SampleExtensionLoad` 事件的事件处理程序的示例。 请注意，在你编辑 *ApplicationEvents.vb* 文件时，如果在代码编辑器左上方的下拉列表中选择“My 应用程序事件”选项，则 `SampleExtensionLoad` 事件将可用  。

[!code-vb[VbVbcnExtendingMy#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnExtendingMy/VB/Class1.vb#6)]

## <a name="design-guidelines"></a>设计指南

在开发 `My` 命名空间的扩展时，请遵循以下指南以帮助最大程度地降低扩展组件的维护成本：

- **仅包含扩展逻辑。** `My` 命名空间扩展中包含的逻辑应仅包含在 `My` 命名空间中公开所需功能所需的代码。 因为扩展将作为源代码驻留在用户项目中，所以更新扩展组件会产生较高的维护成本，应尽可能避免这种情况。
- **最大程度地减少项目假设。** 在创建 `My` 命名空间的扩展时，请勿假设使用一组引用、项目级导入或特定编译器设置（例如，关闭 `Option Strict`）。 相反，请使用 `Global` 关键字最大程度地减少依赖项并完全限定所有类型引用。 此外，请确保在打开 `Option Strict` 的情况下编译扩展，以最大程度地减少扩展中的错误。
- **隔离扩展代码。** 将代码放在单个文件中可使你的扩展能够作为 Visual Studio 项目模板轻松部署。 有关详细信息，请参阅本主题后面的“打包和部署扩展”。 将所有 `My` 命名空间扩展代码放在项目中的单个文件或单独文件夹中还将有助于用户找到 `My` 命名空间扩展。

## <a name="designing-class-libraries-for-my"></a>为 `My` 设计类库

与大多数对象模型一样，某些设计模式适用于 `My` 命名空间，而另一些设计模式则不适用。 设计 `My` 命名空间扩展时，请考虑以下原则：

- **无状态方法。** `My` 命名空间中的方法应提供针对特定任务的完整解决方案。 确保传递给方法的参数值提供完成特定任务所需的全部输入。 避免创建依赖于之前状态的方法，例如与资源的开放连接。
- **全局实例。** `My` 命名空间中维护的唯一状态是项目的全局状态。 例如，`My.Application.Info` 封装整个应用程序共享的状态。
- **简单参数类型。** 通过避免使用复杂参数类型使操作变得简单。 相反，创建不使用参数输入或使用简单输入类型（例如字符串、基元类型等）的方法。
- **工厂方法。** 某些类型必然很难实例化。 提供工厂方法作为 `My` 命名空间的扩展，这样一来，你可以更轻松地发现和使用属于此类别的类型。 `My.Computer.FileSystem.OpenTextFileReader` 是一个效果很好的工厂方法示例。 .NET Framework 中有几种可用的流类型。 通过专门指定文本文件，`OpenTextFileReader` 可帮助用户了解要使用的流。

这些指南不妨碍类库的一般设计原则。 相反，它们是针对正在使用 Visual Basic 和 `My` 命名空间的开发人员进行优化的建议。 有关创建类库的一般设计原则，请参阅[框架设计指南](../../../standard/design-guidelines/index.md)。

## <a name="packaging-and-deploying-extensions"></a>打包和部署扩展

你可以在 Visual Studio 项目模板中包括 `My` 命名空间扩展，也可以打包扩展并将其作为 Visual Studio 项目模板部署。 将 `My` 命名空间扩展打包为 Visual Studio 项目模板时，可以利用 Visual Basic 提供的其他功能。 这些功能使你可以在项目引用特定程序集时包括扩展，或者使用户能够使用 Visual Basic 项目设计器的“My 扩展”页来显式添加 `My` 命名空间扩展  。

有关如何部署 `My` 命名空间扩展的详细信息，请参阅[打包和部署自定义 My 扩展](packaging-and-deploying-custom-my-extensions.md)。

## <a name="see-also"></a>请参阅

- [打包和部署自定义 My 扩展](packaging-and-deploying-custom-my-extensions.md)
- [扩展 Visual Basic 应用程序模型](extending-the-visual-basic-application-model.md)
- [自定义 My 中可用的对象](customizing-which-objects-are-available-in-my.md)
- [“项目设计器”->“My 扩展”页](/visualstudio/ide/reference/my-extensions-page-project-designer-visual-basic)
- [“项目设计器”->“应用程序”页 (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
- [Partial](../../language-reference/modifiers/partial.md)
