---
title: 异常的最佳做法 - .NET
description: 了解异常的最佳做法，例如使用 try/catch/finally、处理没有异常的常见条件以及使用预定义的 .NET 异常类型。
ms.date: 12/05/2018
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- exceptions, best practices
ms.assetid: f06da765-235b-427a-bfb6-47cd219af539
ms.openlocfilehash: 815dcc81cf41465bffd1515d366a66ff558304fa
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828226"
---
# <a name="best-practices-for-exceptions"></a>异常的最佳做法

设计良好的应用处理异常和错误以防止应用崩溃。 本部分介绍了处理和创建异常的最佳做法。

## <a name="use-trycatchfinally-blocks-to-recover-from-errors-or-release-resources"></a>使用 try/catch/finally 块从错误中恢复或释放资源

对可能生成异常的代码使用 `try`/`catch` 块，代码就可以从该异常中恢复。 在 `catch` 块中，始终按从派生程度最高到派生程度最低的顺序对异常排序。 所有异常都派生自 <xref:System.Exception>。 位于处理基本异常类的 catch 子句之后的 catch 子句不处理派生程度较高的异常。 当代码无法从异常中恢复时，请勿捕获该异常。 如有可能，请启用调用堆栈中更上层的方法来进行恢复。

使用 `using` 语句或 `finally` 块清除分配的资源。 当引发了异常时，优先使用 `using` 语句自动清除资源。 使用 `finally` 块清除未实现 <xref:System.IDisposable> 的资源。 即使引发了异常，通常也会执行 `finally` 子句中的代码。

## <a name="handle-common-conditions-without-throwing-exceptions"></a>在不引发异常的前提下，处理常见情况

对于易于发生但可能会触发异常的情况，请考虑使用能避免引发异常的方法进行处理。 例如，如果尝试关闭已关闭的连接，则会获得 `InvalidOperationException`。 尝试关闭前，可通过使用 `if` 语句检查连接状态，避免该情况。

[!code-cpp[Conceptual.Exception.Handling#2](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#2)]
[!code-csharp[Conceptual.Exception.Handling#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#2)]
[!code-vb[Conceptual.Exception.Handling#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#2)]

如果关闭前未检查连接状态，则可能捕获 `InvalidOperationException` 异常。

[!code-cpp[Conceptual.Exception.Handling#3](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#3)]
[!code-csharp[Conceptual.Exception.Handling#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#3)]
[!code-vb[Conceptual.Exception.Handling#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#3)]

选择的方法取决于希望时间发生的频率。

- 如果此事件未经常发生（也就是说，如果此事件确实为异常并指示错误（如意外的文件尾）），则使用异常处理。 如果使用异常处理，将在正常条件下执行较少代码。

- 如果事件例行发生，且被视为正常性执行的一部分，请检查代码中是否存在错误情况。 检查常见错误情况时，为了避免异常，执行较少的代码。

## <a name="design-classes-so-that-exceptions-can-be-avoided"></a>设计类，以避免异常

类可提供一些方法或属性来确保避免生成会引发异常的调用。 例如，<xref:System.IO.FileStream> 类提供可帮助确实是否已到达文件末尾的方法。 它可用于避免在读取超过文件末尾时引发的异常。 下方示例显示如何读取文件末尾而不会引发异常。

[!code-cpp[Conceptual.Exception.Handling#5](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#5)]
[!code-csharp[Conceptual.Exception.Handling#5](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#5)]
[!code-vb[Conceptual.Exception.Handling#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#5)]

避免异常的另一方法是，对极为常见的错误案例返回 NULL（或默认值），而不是引发异常。 极其常见的错误案例可被视为常规控制流。 通过在这些情况下返回 NULL（或默认值），可最大程度地减小对应用的性能产生的影响。

对于值类型，是否使用 `Nullable<T>` 或默认值作为错误指示符是特定应用需要考虑的内容。 通过使用 `Nullable<Guid>`，`default` 变为 `null` 而非 `Guid.Empty`。 有时，添加 `Nullable<T>` 可更加明确值何时存在或不存在。 在其他时候，添加 `Nullable<T>` 可以创建额外的案例以查看不必要的内容，并且仅用于创建潜在的错误源。

## <a name="throw-exceptions-instead-of-returning-an-error-code"></a>引发异常而不是返回错误代码

异常可确保故障不被忽略，因为调用代码不会检查返回代码。

## <a name="use-the-predefined-net-exception-types"></a>使用预定义的 .NET 异常类型

仅当预定义的异常类不适用时，引入新异常类。 例如：

- 如果根据对象的当前状态，属性集或方法调用不适当，则会引发 <xref:System.InvalidOperationException> 异常。

- 如果传送了无效的参数，则引发 <xref:System.ArgumentException> 异常或从 <xref:System.ArgumentException> 派生的一个预定义类。

## <a name="end-exception-class-names-with-the-word-exception"></a>异常类名称的结尾为 `Exception`

需要自定义异常时，对其正确命名并从 <xref:System.Exception> 类进行派生。 例如：

[!code-cpp[Conceptual.Exception.Handling#4](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#4)]
[!code-csharp[Conceptual.Exception.Handling#4](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#4)]
[!code-vb[Conceptual.Exception.Handling#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#4)]

## <a name="include-three-constructors-in-custom-exception-classes"></a>在自定义异常类中包括三种构造函数

创建自己的异常类时，请至少使用三种公共构造函数：无参数构造函数、采用字符串消息的构造函数以及采用字符串消息和内部异常的构造函数。

- <xref:System.Exception.%23ctor>（使用默认值）。

- <xref:System.Exception.%23ctor%28System.String%29>，它接受字符串消息。

- <xref:System.Exception.%23ctor%28System.String%2CSystem.Exception%29>，它接受字符串消息和内部异常。

有关示例，请参见 [如何：创建用户定义的异常](how-to-create-user-defined-exceptions.md)。

## <a name="ensure-that-exception-data-is-available-when-code-executes-remotely"></a>确保代码远程执行时异常数据可用

创建用户定义的异常时，请确保异常的元数据对远程执行的代码可用。

例如，在支持应用域的 .NET 实现中，异常可能会跨应用域抛出。 假设应用域 A 创建应用域 B，后者执行引发异常的代码。 应用域 A 若想正确捕获和处理异常，它必须能够找到包含应用域 B 所引发的异常的程序集。如果应用域 B 在其应用程序基下（但未在应用域 A 的应用程序基下）引发了一个包含在程序集内的异常，那么应用域 A 将无法找到异常，且公共语言运行时将引发 <xref:System.IO.FileNotFoundException> 异常。 为避免此情况，可以两种方式部署包含异常信息的程序集：

- 将程序集放在两个应用域共享的公共应用程序基中。

    \- 或 -

- 如果两个应用域不共享一个公共应用程序基，则用强名称为包含异常信息的程序集签名并将其部署到全局程序集缓存中。

## <a name="use-grammatically-correct-error-messages"></a>使用语法正确的错误消息

编写清晰的句子，包括结束标点。 分配给 <xref:System.Exception.Message?displayProperty=nameWithType> 属性的字符串中的每个句子应以句点结尾。 例如，“日志表已溢出”。 是一个正确的消息字符串。

## <a name="include-a-localized-string-message-in-every-exception"></a>在每个异常中都包含一个本地化字符串消息

用户看到的错误消息派生自引发的异常的 <xref:System.Exception.Message?displayProperty=nameWithType> 属性，而不是派生自异常类的名称。 通常将值赋给 <xref:System.Exception.Message?displayProperty=nameWithType> 属性，方法是将消息字符串传递到[异常构造函数](xref:System.Exception.%23ctor%2A)的 `message` 参数。

对于本地化应用程序，应为应用程序可能引发的每个异常提供本地化消息字符串。 资源文件用于提供本地化错误消息。 有关本地化应用程序和检索本地化字符串的信息，请参阅以下文章：

- [如何：使用本地化的异常消息创建用户定义的异常](how-to-create-localized-exception-messages.md)
- [桌面应用中的资源](../../framework/resources/index.md)
- <xref:System.Resources.ResourceManager?displayProperty=nameWithType>

## <a name="in-custom-exceptions-provide-additional-properties-as-needed"></a>在自定义异常中，按需提供其他属性

仅当存在附加信息有用的编程方案时，才在异常中提供附加属性（不包括自定义消息字符串）。 例如，<xref:System.IO.FileNotFoundException> 提供 <xref:System.IO.FileNotFoundException.FileName> 属性。

## <a name="place-throw-statements-so-that-the-stack-trace-will-be-helpful"></a>放置引发语句，使得堆栈跟踪有所帮助

堆栈跟踪从引发异常的语句开始，到捕获异常的 `catch` 语句结束。

## <a name="use-exception-builder-methods"></a>使用异常生成器方法

类从其实现中的不同位置引发同一异常是常见的情况。 为避免过多的代码，应使用帮助器方法创建异常并将其返回。 例如：

[!code-cpp[Conceptual.Exception.Handling#6](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#6)]
[!code-csharp[Conceptual.Exception.Handling#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#6)]
[!code-vb[Conceptual.Exception.Handling#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#6)]

在某些情况下，更适合使用异常的构造函数生成异常。 例如，<xref:System.ArgumentException> 等全局异常类。

## <a name="restore-state-when-methods-dont-complete-due-to-exceptions"></a>因发生异常而未完成方法时还原状态

当异常从方法引发时，调用方应能够假定没有副作用。 例如，如果你的代码可以通过从一个帐户取钱并存入另一个帐户来转移资金，而在存款时引发了异常，你不希望取款仍然有效。

```csharp
public void TransferFunds(Account from, Account to, decimal amount)
{
    from.Withdrawal(amount);
    // If the deposit fails, the withdrawal shouldn't remain in effect.
    to.Deposit(amount);
}
```

```vb
Public Sub TransferFunds(from As Account, [to] As Account, amount As Decimal)
    from.Withdrawal(amount)
    ' If the deposit fails, the withdrawal shouldn't remain in effect.
    [to].Deposit(amount)
End Sub
```

上面的方法不会直接引发任何异常，但必须以防御方式进行编写，以便在存款操作失败时撤销取款。

解决这一情况的一种方法是，捕获由存款交易引发的异常，然后回滚取款。

```csharp
private static void TransferFunds(Account from, Account to, decimal amount)
{
    string withdrawalTrxID = from.Withdrawal(amount);
    try
    {
        to.Deposit(amount);
    }
    catch
    {
        from.RollbackTransaction(withdrawalTrxID);
        throw;
    }
}
```

```vb
Private Shared Sub TransferFunds(from As Account, [to] As Account, amount As Decimal)
    Dim withdrawalTrxID As String = from.Withdrawal(amount)
    Try
        [to].Deposit(amount)
    Catch
        from.RollbackTransaction(withdrawalTrxID)
        Throw
    End Try
End Sub
```

此示例介绍如何使用 `throw` 重新引发原始异常，让调用方更轻松地发现问题的真正原因，而无需检查 <xref:System.Exception.InnerException> 属性。 另一种方法是，引发一个新的异常并将原始异常包括在其中作为内部异常：

```csharp
catch (Exception ex)
{
    from.RollbackTransaction(withdrawalTrxID);
    throw new TransferFundsException("Withdrawal failed.", innerException: ex)
    {
        From = from,
        To = to,
        Amount = amount
    };
}
```

```vb
Catch ex As Exception
    from.RollbackTransaction(withdrawalTrxID)
    Throw New TransferFundsException("Withdrawal failed.", innerException:=ex) With
    {
        .From = from,
        .[To] = [to],
        .Amount = amount
    }
End Try
```

## <a name="see-also"></a>请参阅

- [异常](index.md)
