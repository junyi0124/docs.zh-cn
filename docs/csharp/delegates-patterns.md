---
title: 委托的常见模式
description: 了解在代码中使用委托避免组件间强耦合的常见模式。
ms.date: 06/20/2016
ms.assetid: 0ff8fdfd-6a11-4327-b061-0f2526f35b43
ms.openlocfilehash: 22ab88e5b139381e3a8921baa20df035f1405146
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223694"
---
# <a name="common-patterns-for-delegates"></a>委托的常见模式

[上一页](delegates-strongly-typed.md)

委托提供了一种机制，可实现涉及组件间最小耦合度的软件设计。

此类设计的出色示例为 LINQ。 LINQ 查询表达式模式依赖于其所有功能的委托。 请考虑此简单示例：

```csharp
var smallNumbers = numbers.Where(n => n < 10);
```

这会将数字序列筛选为仅小于值 10 的数字序列。
`Where` 方法使用委托来确定序列的哪些元素可通过筛选器。 创建 LINQ 查询时，为此特定目的提供委托的实现。

Where 方法的原型是：

```csharp
public static IEnumerable<TSource> Where<TSource> (this IEnumerable<TSource> source, Func<TSource, bool> predicate);
```

此示例重复使用所有方法，这些方法都为 LINQ 的一部分。 它们都依赖于管理特定查询的代码委托。 此 API 设计模式功能强大，需要学习和理解。

此简单示例说明了委托在组件之间仅需要极少耦合度的原因。 不需要创建从特定基类派生的类。 不需要实现特定接口。
唯一的要求是提供对当前任务至关重要的方法实现。

## <a name="building-your-own-components-with-delegates"></a>通过委托生成自己的组件

基于此示例，通过使用依赖于委托的设计来创建组件，从而进行生成。

定义一个可用于大型系统中日志消息的组件。 库组件可以在多种不同的环境中和多个不同的平台上使用。 管理日志的组件中有很多常用功能。 它需要接受来自系统中任何组件的消息。 这些消息将具有不同的优先级（核心组件可进行管理）。 消息应当具有其最终存档形式的时间戳。 对于更高级的方案，你可以按源组件筛选消息。

此功能有一个方面会经常发生变化：写入消息的位置。 在某些环境中，它们可能会写入到错误控制台。 在其他环境中，可能会写入一个文件。 其他可能性包括数据库存储、操作系统事件日志或其他文档存储。

还有可能用于不同方案的输出组合。 建议你将消息写入控制台和文件。

基于委托的设计将提供极大的灵活性，从而轻松支持可能在以后添加的存储机制。

基于此设计，主日志组件可以是非虚拟，甚至是密封的类。 你可以插入任何委托集，将消息写入不同的存储介质。 对多播委托的内置支持有助于支持必须将消息写入多个位置（文件和控制台）的情况。

## <a name="a-first-implementation"></a>首次实现

我们从小处着手：初始实现会接受新消息并使用任意附加委托编写它们。 你可以从一个将消息写入控制台的委托开始。

[!code-csharp[LoggerImplementation](../../samples/snippets/csharp/delegates-and-events/Logger.cs#FirstImplementation "A first Logger implementation.")]

上面的静态类是可以发挥作用的最简单的类。 我们需要编写将消息写入控制台的方法的单个实现：

[!code-csharp[LogToConsole](../../samples/snippets/csharp/delegates-and-events/LoggingMethods.cs#LogToConsole "A Console logger.")]

最后，你需要通过将委托附加到记录器中声明的 WriteMessage 委托来进行挂钩：

[!code-csharp[ConnectDelegate](../../samples/snippets/csharp/delegates-and-events/Program.cs#ConnectDelegate "Connect to the delegate")]

## <a name="practices"></a>实践

到目前为止，我们的示例都相当简单，但仍演示了一些关于委托设计的重要指南。

使用在核心框架中定义的委托类型，用户可更轻松地使用委托。 无需定义新类型，而且使用你库的开发者不需要学习新的专用委托类型。

使用的接口尽可能小且灵活：若要创建新的输出记录器，必须创建一个方法。 该方法可以是静态方法或实例方法。 它可能具有任何访问权限。

## <a name="formatting-output"></a>设置输出的格式

让第一个版本更加可靠，然后开始创建其他日志记录机制。

然后，向 `LogMessage()` 方法添加一些参数，以便日志类创建更多结构化消息：

[!code-csharp[Severity](../../samples/snippets/csharp/delegates-and-events/Logger.cs#Severity "Define severities")]
[!code-csharp[NextLogger](../../samples/snippets/csharp/delegates-and-events/Logger.cs#LoggerTwo "Refine the Logger")]

接下来，使用 `Severity` 参数来筛选发送到日志输出的消息。

[!code-csharp[FinalLogger](../../samples/snippets/csharp/delegates-and-events/Logger.cs#LoggerFinal "Finish the Logger")]

## <a name="practices"></a>实践

已向日志记录基础结构添加了新功能。 由于记录器组件极其松散地耦合到输出机制，因此可在不影响代码实现记录器托管的情况下添加新功能。

继续构建，你会看到更多的示例，其中显示这种松散的耦合度在更新站点部件方面实现了很高的灵活性，而不会对其他位置做出更改。 实际上，在更大的应用程序中，记录器输出类可能位于不同的程序集中，甚至不需要重新生成。

## <a name="building-a-second-output-engine"></a>生成第二个输出引擎

还将附带日志组件。 我们再添加一个将消息记录到文件的输出引擎。 这将是一个更为普及的输出引擎。 它将是一个封装文件操作的类，并确保文件在每次写入后始终处于关闭状态。 这可以确保生成每条消息后将所有数据刷新到磁盘。

下面是基于文件的记录器：

[!code-csharp[FileLogger](../../samples/snippets/csharp/delegates-and-events/FileLogger.cs#FileLogger "Log to files")]

创建此类后，可将它进行实例化，然后它会将 LogMessage 方法附加到记录器组件中：

[!code-csharp[FileLogger](../../samples/snippets/csharp/delegates-and-events/Program.cs#FileLogger "Log to files")]

这两项并不互相排斥。 你可以附加这两种日志方法并生成要发送到控制台和文件的消息：

```csharp
var fileOutput = new FileLogger("log.txt");
Logger.WriteMessage += LoggingMethods.LogToConsole; // LoggingMethods is the static class we utilized earlier
```

以后，即使在同一个应用程序中，也可在不对系统产生任何其他问题的情况下删除其中一个委托：

```csharp
Logger.WriteMessage -= LoggingMethods.LogToConsole;
```

## <a name="practices"></a>实践

现在，你已添加日志记录子系统的第二个输出处理程序。
这需要更多的基础结构来正确支持文件系统。 此委托为实例方法。 其为私有方法。
由于委托基础结构可以连接委托，因此不需要太高的可访问性。

其次，基于委托的设计可实现多种输出方法，且无需额外的代码。 无需生成任何其他基础结构来支持多种输出方法。 它们将变为调用列表上的另一种方法。

需要特别注意文件日志记录输出方法中的代码。 对其进行编码以确保不引发任何异常。 虽然不是绝对必要，但这通常是很好的做法。 如果任意一种委托方法引发异常，将不会调用该调用中剩余的其他委托。

最后请注意，文件记录器必须通过打开和关闭每条日志消息上的文件来管理其资源。 可以选择让文件保持打开状态，并在完成后执行 IDisposable 以关闭文件。
这两种方法各有利弊。 两者都在类之间创建了更高的耦合度。

为了支持这两种方案，记录器类中的代码都不需要更新。

## <a name="handling-null-delegates"></a>处理 Null 委托

最后，更新 LogMessage 方法，从而在没有选择输出机制的情况下更加可靠。 `WriteMessage` 委托没有附加调用列表时，当前实现将引发 `NullReferenceException`。
你可能更需要在没有附加方法时自行继续的设计。 将 null 条件运算符与 `Delegate.Invoke()` 方法结合使用时，很容易实现该目标：

```csharp
public static void LogMessage(string msg)
{
    WriteMessage?.Invoke(msg);
}
```

当左操作数（本例中为 `WriteMessage`）为 null 时，null 条件运算符（`?.`）会短路，这意味着不会尝试记录消息。

不会在 `System.Delegate` 或 `System.MulticastDelegate` 的文档中列出 `Invoke()` 方法。 编译器将为声明的所有委托类型生成类型安全的 `Invoke` 方法。 在此示例中，这意味着 `Invoke` 只需要一个 `string` 参数，并且有一个无效返回类型。

## <a name="summary-of-practices"></a>实践摘要

你已了解日志组件的起始部分，可以使用其他编写器和其他功能进行扩展。 通过在设计中使用委托，这些不同的组件非常松散地耦合在一起。 这样可提供多种优势。 可轻松创建新的输出机制并将它们附加到系统中。 这些机制只需要一种方法：编写日志消息的方法。 这种设计在添加新功能时有非常强的复原能力。 所有编写器所需的协定都是为了实现一种方法。 该方法可以是静态方法或实例方法。 它可以是公用、专用或任何其他合法访问。

记录器类可在不引入重大更改的情况下进行任何数量的增强或更改。 与类相似，无法在没有重大更改的风险下修改公共 API。 但是，因为仅通过委托进行记录器和输出引擎之间的耦合，因此不涉及其他类型（如接口或基类）。 耦合度越小越好。

[下一部分](events-overview.md)
