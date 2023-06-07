---
title: 事件介绍
description: 本概述中介绍 .NET Core 中的事件和事件的语言设计目标。
ms.date: 06/20/2016
ms.assetid: 9b8d2a00-1584-4a5b-8994-5003d54d8e0c
ms.openlocfilehash: 4da44c151244e8b5de34f550040c271131d9598c
ms.sourcegitcommit: e7acba36517134238065e4d50bb4a1cfe47ebd06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89465229"
---
# <a name="introduction-to-events"></a>事件介绍

[上一页](delegates-patterns.md)

和委托类似，事件是*后期绑定*机制。 实际上，事件是建立在对委托的语言支持之上的。

事件是对象用于（向系统中的所有相关组件）广播已发生事情的一种方式。 任何其他组件都可以订阅事件，并在事件引发时得到通知。

你可能已在某些编程中使用过事件。 许多图形系统都具有用于报告用户交互的事件模型。 这些事件会报告鼠标移动、按钮点击和类似的交互。 这是使用事件的最常见情景之一，但并非唯一的情景。

可以定义应针对类引发的事件。 使用事件时，需要注意的一点是特定事件可能没有任何注册的对象。 必须编写代码，以确保在未配置侦听器时不会引发事件。

通过订阅事件，还可在两个对象（事件源和事件接收器）之间创建耦合。 需要确保当不再对事件感兴趣时，事件接收器将从事件源取消订阅。

## <a name="design-goals-for-event-support"></a>事件支持的设计目标

事件的语言设计针对这些目标：

- 在事件源和事件接收器之间启用非常小的耦合。 这两个组件可能不会由同一个组织编写，甚至可能会通过完全不同的计划进行更新。

- 订阅事件并从同一事件取消订阅应该非常简单。

- 事件源应支持多个事件订阅服务器。 它还应支持不附加任何事件订阅服务器。

你会发现事件的目标与委托的目标非常相似。
因此，事件语言支持基于委托语言支持构建。

## <a name="language-support-for-events"></a>事件的语言支持

用于定义事件以及订阅或取消订阅事件的语法是对委托语法的扩展。

定义使用 `event` 关键字的事件：

```csharp
public event EventHandler<FileListArgs> Progress;
```

该事件（在此示例中，为 `EventHandler<FileListArgs>`）的类型必须为委托类型。 声明事件时，应遵循许多约定。 通常情况下，事件委托类型具有无效的返回。
事件声明应为谓词或谓词短语。
当事件报告已发生的事情时，请使用过去时。 使用现在时谓词（例如 `Closing`）报告将要发生的事情。 通常，使用现在时表示类支持某种类型的自定义行为。 最常见的方案之一是支持取消。 例如，`Closing` 事件可能包括指示是否应继续执行关闭操作的参数。  其他方案可能会允许调用方通过更新事件参数的属性来修改行为。 你可以引发一个事件以指示算法将采取的建议的下一步操作。 事件处理程序可以通过修改事件参数的属性授权不同的操作。

想要引发事件时，使用委托调用语法调用事件处理程序：

```csharp
Progress?.Invoke(this, new FileListArgs(file));
```

如[委托](delegates-patterns.md)部分中所介绍的那样，?.
运算符可以轻松确保在事件没有订阅服务器时不引发事件。

通过使用 `+=` 运算符订阅事件：

```csharp
EventHandler<FileListArgs> onProgress = (sender, eventArgs) =>
    Console.WriteLine(eventArgs.FoundFile);

fileLister.Progress += onProgress;
```

处理程序方法通常为前缀“On”，后跟事件名称，如上所示。

使用 `-=` 运算符取消订阅：

```csharp
fileLister.Progress -= onProgress;
```

请务必为表示事件处理程序的表达式声明局部变量。 这将确保取消订阅删除该处理程序。
如果使用的是 lambda 表达式的主体，则将尝试删除从未附加过的处理程序，此操作为无效操作。

下一篇文章将介绍有关典型事件模式及此示例的不同变体的详细信息。

[下一部分](event-pattern.md)
