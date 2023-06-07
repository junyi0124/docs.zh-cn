---
title: 事件
description: '了解 F # 事件如何使你能够将函数调用与用户操作关联，这在 GUI 编程中非常重要。'
ms.date: 08/15/2020
ms.openlocfilehash: 17e0cc8840053bf24d5c69694fe94d544c44510d
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740336"
---
# <a name="events"></a>事件

事件允许您将函数调用与用户操作关联，并且是 GUI 编程的关键所在。 事件也可以由应用程序或操作系统触发。

## <a name="handling-events"></a>处理事件

当您使用诸如 Windows 窗体或 Windows Presentation Foundation (WPF) 之类的 GUI 库时，应用程序中的大部分代码都是针对该库预定义的事件运行的。 这些预定义事件是诸如窗体和控件等 GUI 类的成员。 通过引用有意义的特定命名事件（例如，`Click` 类的 `Form` 事件）并调用 `Add` 方法，您可以向预先存在的事件（例如按钮单击）中添加自定义行为，如下面的代码所示。 如果您从 F# Interactive 运行此事件，请忽略对 `System.Windows.Forms.Application.Run(System.Windows.Forms.Form)` 的调用。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3601.fs)]

`Add` 方法的类型为 `('a -> unit) -> unit`。 因此，事件处理程序方法将接受一个参数（通常为事件自变量），并返回 `unit`。 前面的示例显示了 lambda 表达式形式的事件处理程序。 事件处理程序也可以为函数值，如下面的代码示例所示。 下面的代码示例还显示了事件处理程序参数的用法，这些参数提供特定于事件类型的信息。 对于 `MouseMove` 事件，系统将传递 `System.Windows.Forms.MouseEventArgs` 对象，其中包含指针的 `X` 和 `Y` 位置。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3602.fs)]

## <a name="creating-custom-events"></a>创建自定义事件

F # 事件由 F # [事件](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpevent-1.html) 类型表示，该类型实现 [IEvent](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-ievent-1.html) 接口。 `IEvent` 本身是一个接口，它合并了两个其他接口（ `System.IObservable<'T>` 和 [IDelegateEvent](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-idelegateevent-1.html)）的功能。 因此，在其他语言中，`Event` 具有委托的同等功能，以及来自 `IObservable` 的附加功能，这意味着 F# 事件支持事件筛选并使用 F# 第一类函数和 lambda 表达式作为事件处理程序。 此功能在 [事件模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-eventmodule.html)中提供。

若要像任何其他 .NET Framework 事件一样为某个类创建事件，请向该类添加一个 `let` 绑定，用于将 `Event` 定义为类中的字段。 您可以将所需的事件参数类型指定为类型参数，或将其保留为空，让编译器推断出相应的类型。 还必须定义一个将事件公开为 CLI 事件的事件成员。 此成员应具有 [CLIEvent](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-clieventattribute.html) 属性。 它的声明方式与属性类似，其实现只是对事件的 [发布](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpevent-1.html#Publish) 属性的调用。 类用户可使用已发布事件的 `Add` 方法来添加处理程序。 `Add` 方法的参数可以为 lambda 表达式。 你可以使用事件的 `Trigger` 属性来引发事件，并将自变量传递给处理程序函数。 下面的代码示例阐释了这一点。 在此示例中，事件的推断类型参数是一个元组，它表示 lambda 表达式的参数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3605.fs)]

输出如下所示。

```console
Event1 occurred! Object data: Hello World!
```

此处阐释了 `Event` 模块提供的附加功能。 下面的代码示例阐释了 `Event.create` 的基本用法：创建一个事件和一个触发器方法，添加两个 Lambda 表达式形式的事件处理程序，然后触发该事件来执行这两个 Lambda 表达式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3603.fs)]

上述代码的输出结果如下。

```console
Event occurred.
Given a value: Event occurred.
```

## <a name="processing-event-streams"></a>处理事件流

您可以使用模块中的函数[Event.add](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-eventmodule.html#add) `Event` 以高度自定义的方式来处理事件流，而不是仅通过使用 event 函数为事件添加事件处理程序。 为此，可以使用前向管道 (`|>`) 以及事件作为一系列函数调用中的第一个值，并使用 `Event` 模块函数作为后续的函数调用。

下面的代码示例显示如何设置仅在某些情况下才会为其调用事件处理程序的事件。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet3604.fs)]

可 [观察模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-observablemodule.html) 包含可在可观察对象上操作的类似函数。 可观测对象与事件类似，但只有在其本身被订阅时才会主动订阅事件。

## <a name="implementing-an-interface-event"></a>实现 Interface 事件

在开发 UI 组件时，你通常会先创建继承自现有窗体或控件的新窗体或新控件。 事件经常是在接口上定义的，在此情况下，你必须实现接口才能实现事件。 `System.ComponentModel.INotifyPropertyChanged` 接口定义单个 `System.ComponentModel.INotifyPropertyChanged.PropertyChanged` 事件。 以下代码演示如何实现此继承接口定义的事件：

```fsharp
module CustomForm

open System.Windows.Forms
open System.ComponentModel

type AppForm() as this =
    inherit Form()

    // Define the propertyChanged event.
    let propertyChanged = Event<PropertyChangedEventHandler, PropertyChangedEventArgs>()
    let mutable underlyingValue = "text0"

    // Set up a click event to change the properties.
    do
        this.Click |> Event.add(fun evArgs ->
            this.Property1 <- "text2"
            this.Property2 <- "text3")

    // This property does not have the property-changed event set.
    member val Property1 : string = "text" with get, set

    // This property has the property-changed event set.
    member this.Property2
        with get() = underlyingValue
        and set(newValue) =
            underlyingValue <- newValue
            propertyChanged.Trigger(this, new PropertyChangedEventArgs("Property2"))

    // Expose the PropertyChanged event as a first class .NET event.
    [<CLIEvent>]
    member this.PropertyChanged = propertyChanged.Publish

    // Define the add and remove methods to implement this interface.
    interface INotifyPropertyChanged with
        member this.add_PropertyChanged(handler) = propertyChanged.Publish.AddHandler(handler)
        member this.remove_PropertyChanged(handler) = propertyChanged.Publish.RemoveHandler(handler)

    // This is the event-handler method.
    member this.OnPropertyChanged(args : PropertyChangedEventArgs) =
        let newProperty = this.GetType().GetProperty(args.PropertyName)
        let newValue = newProperty.GetValue(this :> obj) :?> string
        printfn "Property {args.PropertyName} changed its value to {newValue}"

// Create a form, hook up the event handler, and start the application.
let appForm = new AppForm()
let inpc = appForm :> INotifyPropertyChanged
inpc.PropertyChanged.Add(appForm.OnPropertyChanged)
Application.Run(appForm)
```

若要在构造函数中挂钩事件，代码会有点复杂，因为事件挂钩必须位于其他构造函数的 `then` 块中，如下面的示例所示：

```fsharp
module CustomForm

open System.Windows.Forms
open System.ComponentModel

// Create a private constructor with a dummy argument so that the public
// constructor can have no arguments.
type AppForm private (dummy) as this =
    inherit Form()

    // Define the propertyChanged event.
    let propertyChanged = Event<PropertyChangedEventHandler, PropertyChangedEventArgs>()
    let mutable underlyingValue = "text0"

    // Set up a click event to change the properties.
    do
        this.Click |> Event.add(fun evArgs ->
            this.Property1 <- "text2"
            this.Property2 <- "text3")

    // This property does not have the property changed event set.
    member val Property1 : string = "text" with get, set

    // This property has the property changed event set.
    member this.Property2
        with get() = underlyingValue
        and set(newValue) =
            underlyingValue <- newValue
            propertyChanged.Trigger(this, new PropertyChangedEventArgs("Property2"))

    [<CLIEvent>]
    member this.PropertyChanged = propertyChanged.Publish

    // Define the add and remove methods to implement this interface.
    interface INotifyPropertyChanged with
        member this.add_PropertyChanged(handler) = this.PropertyChanged.AddHandler(handler)
        member this.remove_PropertyChanged(handler) = this.PropertyChanged.RemoveHandler(handler)

    // This is the event handler method.
    member this.OnPropertyChanged(args : PropertyChangedEventArgs) =
        let newProperty = this.GetType().GetProperty(args.PropertyName)
        let newValue = newProperty.GetValue(this :> obj) :?> string
        printfn "Property {args.PropertyName} changed its value to {newValue}"

    new() as this =
        new AppForm(0)
        then
            let inpc = this :> INotifyPropertyChanged
            inpc.PropertyChanged.Add(this.OnPropertyChanged)

// Create a form, hook up the event handler, and start the application.
let appForm = new AppForm()
Application.Run(appForm)
```

## <a name="see-also"></a>请参阅

- [成员](index.md)
- [处理和引发事件](../../../standard/events/index.md)
- [Lambda 表达式： `fun` 关键字](../functions/lambda-expressions-the-fun-keyword.md)
