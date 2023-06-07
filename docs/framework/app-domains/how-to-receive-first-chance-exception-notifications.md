---
title: 如何：接收第一机会异常通知
description: 在 CLR 搜索异常处理程序之前，通过 AppDomain 类的 FirstChanceException 事件获取 .NET 中的第一机会异常通知。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- first-chance exception notifications
- exceptions, first chance notifications
ms.assetid: 66f002b8-a97d-4a6e-a503-2cec01689113
ms.openlocfilehash: 0b3150a52a68e078d1052a9894bb652ad35027d0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242560"
---
# <a name="how-to-receive-first-chance-exception-notifications"></a>如何：接收第一机会异常通知

通过 <xref:System.AppDomain> 类的 <xref:System.AppDomain.FirstChanceException> 事件，可在公共语言运行时开始搜索异常处理程序之前，收到已引发异常的通知。

 该事件在应用程序域级别引发。 由于执行线程可能经过多个应用程序域，因此可在一个应用程序域中处理另一个应用程序域中未处理的异常。 已添加该事件的处理程序的每个应用程序域中均会出现通知，直到某个应用程序域处理该异常。

 本文中的过程和示例说明如何在具有一个应用程序域的简单程序中和创建的应用程序域中接收最可能发生的异常通知。

 有关跨多个应用程序域的更复杂示例，请参阅 <xref:System.AppDomain.FirstChanceException> 事件的示例。

## <a name="receiving-first-chance-exception-notifications-in-the-default-application-domain"></a>在默认应用程序域中接收最可能发生的异常通知

 在以下过程中，应用程序的入口点（`Main()` 方法）在默认应用程序域中运行。

#### <a name="to-demonstrate-first-chance-exception-notifications-in-the-default-application-domain"></a>在默认应用程序域中演示最可能发生的异常通知

1. 使用 lambda 函数为 <xref:System.AppDomain.FirstChanceException> 事件定义事件处理程序，并将其附加到此事件。 在此示例中，事件处理程序输出了在其中处理事件的应用程序域的名称和异常的 <xref:System.Exception.Message%2A> 属性。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#2)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#2)]

2. 引发异常并将其捕获。 在运行时找到异常处理程序之前，引发 <xref:System.AppDomain.FirstChanceException> 事件并显示一条消息。 此消息后跟由异常处理程序显示的消息。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#3)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#3)]

3. 引发异常，但不将其捕获。 在运行时查找异常处理程序之前，引发 <xref:System.AppDomain.FirstChanceException> 事件并显示一条消息。 由于没有任何异常处理程序，因此应用程序将终止。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#4)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#4)]

     此过程前 3 个步骤中显示的代码构成了一个完整的控制台应用程序。 应用程序的输出因 .exe 文件的名称而异，因为默认应用程序域的名称是由 .exe 文件的名称和扩展名组成的。 有关示例输出，请参阅以下内容。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#5)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#5)]

## <a name="receiving-first-chance-exception-notifications-in-another-application-domain"></a>在另一个应用程序域中接收最可能发生的异常通知

 如果程序包含多个应用程序域，则可选择用于接收通知的应用程序域。

#### <a name="to-receive-first-chance-exception-notifications-in-an-application-domain-that-you-create"></a>在创建的应用程序域中接收最可能发生的异常通知

1. 为 <xref:System.AppDomain.FirstChanceException> 事件定义事件处理程序。 此示例使用 `static` 方法（在 Visual Basic 中为 `Shared` 方法），该方法可输出在其中处理事件的应用程序域的名称和异常的 <xref:System.Exception.Message%2A> 属性。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#3)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#3)]

2. 创建一个应用程序域，并为该应用程序域的 <xref:System.AppDomain.FirstChanceException> 事件添加事件处理程序。 在此示例中，该应用程序域的名称为 `AD1`。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#2)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#2)]

     可按照相同的方式在默认应用程序域中处理此事件。 使用 `Main()` 中的 `static`（在 Visual Basic 中为 `Shared`）<xref:System.AppDomain.CurrentDomain%2A?displayProperty=nameWithType> 属性可获取对默认应用程序域的引用。

#### <a name="to-demonstrate-first-chance-exception-notifications-in-the-application-domain"></a>在应用程序域中演示最可能发生的异常通知

1. 在上一过程创建的应用程序域中创建一个 `Worker` 对象。 `Worker` 类必须是公共的，且派生自 <xref:System.MarshalByRefObject>，如本文末尾的完整示例中所示。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#4)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#4)]

2. 调用 `Worker` 对象的一个可引发异常的方法。 在此示例中，调用了两次 `Thrower` 方法。 第一次调用时，该方法的参数为 `true`，这使该方法捕获自己的异常。 第二次调用时，该方法的参数为 `false`，并且 `Main()` 方法捕获默认应用程序域中的异常。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#6)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#6)]

3. 在 `Thrower` 方法中放置代码以控制该方法是否处理自己的异常。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#5)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#5)]

## <a name="example"></a>示例

 下例创建一个名为 `AD1` 的应用程序域，并向该应用程序域的 <xref:System.AppDomain.FirstChanceException> 事件添加一个事件处理程序。 此示例还会在应用程序域中创建 `Worker` 类的一个实例，并调用名为 `Thrower` 的方法，该方法将引发 <xref:System.ArgumentException>。 该方法可能会捕获异常，也可能无法处理异常，具体取决于其参数的值。

 每当 `Thrower` 方法在 `AD1` 中引发异常时，`AD1` 中就会引发 <xref:System.AppDomain.FirstChanceException> 事件，并且事件处理程序会显示一条消息。 然后，运行时将查找异常处理程序。 在第一种情况下，可在 `AD1` 中找到异常处理程序。 在第二种情况下，不会在 `AD1` 中处理异常，而是在默认应用程序域中捕获异常。

> [!NOTE]
> 默认应用程序域的名称与可执行文件的名称相同。

 如果将 <xref:System.AppDomain.FirstChanceException> 事件的处理程序添加到默认应用程序域，则在默认应用程序域处理异常之前，系统将引发并处理该事件。 为此，请将 C# 代码 `AppDomain.CurrentDomain.FirstChanceException += FirstChanceException;`（在 Visual Basic 中为 `AddHandler AppDomain.CurrentDomain.FirstChanceException, FirstChanceException`）添加到 `Main()` 的开头。

 [!code-csharp[System.AppDomain.FirstChanceException_howto#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#1)]
 [!code-vb[System.AppDomain.FirstChanceException_howto#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#1)]

## <a name="see-also"></a>请参阅

- <xref:System.AppDomain.FirstChanceException>
