---
title: 标准 .NET 事件模式
description: 介绍 .NET 事件模式，如何创建标准事件源以及订阅并处理代码中的标准事件。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 8a3133d6-4ef2-46f9-9c8d-a8ea8898e4c9
ms.openlocfilehash: dec516767e43a6bf4edfa555e34f3adcc21a46e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146136"
---
# <a name="standard-net-event-patterns"></a>标准 .NET 事件模式

[上一篇](events-overview.md)

.NET 事件通常遵循几种已知模式。 标准化这些模式意味着开发人员可利用这些标准模式的相关知识，将其应用于任何 .NET 事件程序。

让我们开始通览这些标准模式，以便你掌握创建标准事件源、在代码中订阅和处理标准事件所需的全部知识。

## <a name="event-delegate-signatures"></a>事件委托签名

.NET 事件委托的标准签名是：

```csharp
void OnEventRaised(object sender, EventArgs args);
```

返回类型为 void。 事件基于委托，而且是多播委托。 对任何事件源都支持多个订阅服务器。 来自方法的单个返回值不会扩展到多个事件订阅服务器。 引发事件后事件源的返回值是什么？ 稍后在本文中将介绍如何创建事件协议，以支持事件订阅服务器向事件源报告信息。

参数列表包含两种参数：发件人和事件参数。 `sender` 的编译时类型为 `System.Object`，即使有一个始终正确的更底层派生的类型亦是如此。 按照惯例使用 `object`。

第二种参数通常是派生自 `System.EventArgs` 的类型。 （在[下一部分](modern-events.md)中此约定不再强制执行。）即使事件类型无需任何其他参数，你仍将提供这两种参数。
应使用特殊值 `EventArgs.Empty` 来表示事件不包含任何附加信息。

让我们生成一个类，它在目录或遵循模式的任何子目录中列出文件。 此组件为每个找到的与模式相匹配的文件引发事件。

使用事件模型有一些设计优势。 可以创建多个事件侦听器，用于在找到查找的文件时执行不同的操作。 合并不同的侦听器可以创建更可靠的算法。

下面是找到查找的文件时的初始事件参数声明：

[!code-csharp[EventArgs](../../samples/snippets/csharp/events/Program.cs#EventArgsV1 "Define event arguments")]

尽管这种类型看上去是小型的仅限数据的类型，但仍应按约定将其设为引用 (`class`) 类型。 这意味着参数对象将通过引用来传递，并且所有订阅服务器都将查看到任何数据更新。 第一版是不可变对象。 应优先将事件参数类型中的属性设为不可变。 这样一来，一个订阅服务器在其他订阅服务器看到值之前便无法更改值。 （但对此也有例外，如下所示。）  

接下来，我们要在 FileSearcher 类中创建事件声明。 利用 `EventHandler<T>` 类型意味着尚无需创建其他类型定义。 只需使用泛型专业化即可。

让我们通过填充 FileSearcher 类来搜索与模式匹配的文件，并在发现匹配时引发正确的事件。

[!code-csharp[FileSearcher](../../samples/snippets/csharp/events/Program.cs#FileSearcherV1 "Create the initial file searcher")]

## <a name="defining-and-raising-field-like-events"></a>定义并引发类似字段的事件

要将事件添加到类，最简单的方式是将该事件声明为公共字段，如上面的示例中所示：

[!code-csharp[DeclareEvent](../../samples/snippets/csharp/events/Program.cs#DeclareEvent "Declare the file found event")]

看起来它像在声明一个公共字段，这似乎是一个面向对象的不良实践。 你希望通过属性或方法来保护数据访问。 虽然这可能看起来是糟糕的做法，但编译器生成的代码确实会创建包装器，以便事件对象只能通过安全的方式进行访问。 类似字段的事件上唯一可用的操作是添加处理程序：

[!code-csharp[DeclareEventHandler](../../samples/snippets/csharp/events/Program.cs#DeclareEventHandler "Declare the file found event handler")]

和删除处理程序：

[!code-csharp[RemoveEventHandler](../../samples/snippets/csharp/events/Program.cs#RemoveHandler "Remove the event handler")]

请注意，处理程序有一个局部变量。 如果使用了 lambda 的正文，则删除操作无法正常进行。 它将成为不同的委托实例，并静默地不执行任何操作。

类之外的代码无法引发事件，也不能执行任何其它操作。

## <a name="returning-values-from-event-subscribers"></a>从事件订阅服务器返回值

你的简单版本当前运行正常。 让我们添加另一项功能：取消。

在引发找到的事件时，如果此文件是最后查找到的文件，则侦听器应能够停止进一步的处理。

事件处理程序不返回值，因此需以其它方式进行通信。 标准事件模式使用 EventArgs 对象来包含字段，事件订阅服务器使用这些字段进行通信取消。

根据“取消”协定的语义，有两种不同的模式可供使用。 在两种情况下，你都将为找到的文件事件向 EventArguments 添加布尔字段。

其中一种模式允许任一订阅服务器取消操作。
在此模式下，新字段会初始化为 `false`。 任何订阅服务器都可将其更改为 `true`。 当所有订阅服务器观察到事件已引发后，FileSearcher 组件将检查布尔值，并执行操作。

在第二种模式下，仅当所有订阅服务器都要取消操作时才可取消操作。 在此模式下，新字段会初始化为指示操作应取消，而任何订阅服务器都可将其更改为指示操作应继续。
当所有订阅服务器观察到事件已引发后，FileSearcher 组件将检查布尔，并执行操作。 此模式还有一个额外步骤：组件需知道是否有任何订阅服务器已经看到过该事件。 如果没有订阅服务器，字段会错误地指示取消。

让我们来实现此示例的第一版。 需要将名为 `CancelRequested` 的布尔字段添加到 `FileFoundArgs` 类型：

[!code-csharp[EventArgs](../../samples/snippets/csharp/events/Program.cs#EventArgs "Update event arguments")]

此新字段将自动初始化为 `false`，即布尔字段的默认值，因此不会意外取消。 对组件进行的唯一其它更改是在引发事件后检查标志，查看是否有任何订阅服务器提出了取消请求：

```csharp
public void List(string directory, string searchPattern)
{
    foreach (var file in Directory.EnumerateFiles(directory, searchPattern))
    {
        var args = new FileFoundArgs(file);
        FileFound?.Invoke(this, args);
        if (args.CancelRequested)
            break;
    }
}
```

此模式的一个优点是不会造成重大更改。
在此之前没有订阅服务器请求取消，现在也不会有。 没有任何订阅服务器代码需要更新，除非它们想支持新的取消协议。 这是极为松散耦合的。

让我们来更新订阅服务器，使其在找到第一个可执行文件时请求取消：

```csharp
EventHandler<FileFoundArgs> onFileFound = (sender, eventArgs) =>
{
    Console.WriteLine(eventArgs.FoundFile);
    eventArgs.CancelRequested = true;
};
```

## <a name="adding-another-event-declaration"></a>添加另一个事件声明

让我们再添加一项功能，并演示事件的其它语言习惯用语。 让我们添加搜索文件时遍历所有子目录的 `Search` 方法的重载。

在拥有多个子目录的目录中，此操作可能要花较长时间。 让我们添加一个在每次新目录搜索开始时引发的事件。 这让订阅服务器可以跟踪进度，并根据进度更新用户。 目前为止，你所创建的所有示例都是公共的。 让我们把这个示例设为内部事件。 这意味着你也可以将这些类型用于参数内部。

首先，创建新的 EventArgs 派生类，用于报告新目录和进度。

[!code-csharp[DirEventArgs](../../samples/snippets/csharp/events/Program.cs#SearchDirEventArgs "Define search directory event arguments")]

同样，可以根据建议为事件参数设置不可变的引用类型。

接下来，定义事件。 此时，需使用不同的语法。 除使用字段语法之外，还可以显式创建包含添加或删除处理程序的属性。 在本例中，这些处理程序中无需包含额外的代码，但这一步演示了你可以如何创建它们。

[!code-csharp[Declare event with add and remove handlers](../../samples/snippets/csharp/events/Program.cs#DeclareSearchEvent "Declare the event with add and remove handlers")]

在许多方面，此处编写的代码可反映编译器为你已见过的字段事件定义所生成的代码。 创建事件所使用的语法与用于[属性](properties.md)的语法是极为相似的。 请注意，处理程序的名称各不相同：`add` 和 `remove`。 通过调用它们来订阅事件，或取消订阅事件。 请注意，还必须声明一个私有支持字段以存储事件变量。 它初始化为 null。

接下来，让我们添加 `Search` 方法的重载，该重载遍历子目录，并引发这两个事件。 要实现此目的，最简单的方法是使用默认参数来指定你要搜索所有目录：

[!code-csharp[SearchImplementation](../../samples/snippets/csharp/events/Program.cs#FinalImplementation "Implementation to search directories")]

此时可运行调用重载的应用程序来搜索所有子目录。 虽然新 `ChangeDirectory` 事件中没有订阅服务器，但使用 `?.Invoke()` 习惯用语可确保此操作正常。

 让我们通过添加处理程序来编写一行，用于在控制台窗口显示进度。

[!code-csharp[Search](../../samples/snippets/csharp/events/Program.cs#Search "Declare event handler")]

你已了解了整个 .NET 生态系统所遵循的模式。
通过学习这些模式和约定，将能够快速编写惯用的 C# 和 .NET。

接下来，将了解在 .NET 的最新版本中关于这些模式的一些更改。

[下一页](modern-events.md)
