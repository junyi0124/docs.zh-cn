---
title: 资源管理:Use 关键字
description: 了解F#关键字 "use" 和 "using" 函数, 它可以控制资源的初始化和发布。
ms.date: 05/16/2016
ms.openlocfilehash: 5e0838401bee02050343b2f6dcc646a8dc8b4dc0
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68627259"
---
# <a name="resource-management-the-use-keyword"></a>资源管理:Use 关键字

本主题介绍关键字`use` `using`和函数, 它可以控制资源的初始化和发布。

## <a name="resources"></a>资源

术语 "*资源*" 使用多种方法。 是的, 资源可以是应用程序使用的数据, 例如字符串、图形等, 但在此上下文中,*资源*是指软件或操作系统资源, 如图形设备上下文、文件句柄、网络和数据库连接、并发对象 (如等待句柄等)。 应用程序对这些资源的使用涉及到从操作系统或其他资源提供程序获取资源, 然后将该资源的更高版本发布到池中, 以便可以将其提供给其他应用程序。 当应用程序不将资源释放回公用池时, 会出现问题。

## <a name="managing-resources"></a>管理资源

若要有效地管理应用程序中的资源, 必须以可预测的方式立即释放资源。 .NET Framework 提供`System.IDisposable`接口, 有助于实现此目的。 实现`System.IDisposable`的类型`System.IDisposable.Dispose`具有可正确释放资源的方法。 编写良好的应用程序可`System.IDisposable.Dispose`保证当不再需要任何持有有限资源的对象时, 会立即调用。 幸运的是, 大多数 .NET 语言都提供支持来简化这F#一过程, 并且不会出现异常。 有两种支持 dispose 模式的有用语言构造: `use`绑定`using`和函数。

## <a name="use-binding"></a>使用绑定

关键字的形式与`let`绑定类似: `use`

使用*值* = *表达式*

它提供与`let`绑定相同的功能, 但在值超出范围`Dispose`时对值添加对的调用。 请注意, 编译器会对值插入 null 检查, 因此, 如果该值为`null`, 则不`Dispose`会尝试调用。

下面的示例演示如何使用`use`关键字自动关闭文件。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6301.fs)]

> [!NOTE]
> 可以在计算`use`表达式中使用, 在这种情况下, 将使用`use`自定义版本的表达式。 有关详细信息, 请参阅[序列](sequences.md)、[异步工作流](asynchronous-workflows.md)和[计算表达式](computation-expressions.md)。

## <a name="using-function"></a>使用函数

`using`函数具有以下格式:

`using`(*表达式*=)*函数或 lambda*

在表达式*中, 表达式*= 创建必须释放的对象。 `using` *表达式*类型 (必须释放的对象) 的结果将成为*函数或 lambda*的参数 (*值*), 这是一个函数, 该函数需要类型的单个剩余参数与生成*的值相匹配*表达式类型或需要该类型的参数的 lambda 表达式。 在函数执行结束时, 运行时会调用`Dispose`并释放资源 (除非值为`null`, 在这种情况下, 不尝试调用 Dispose)。

下面的示例演示带有`using` lambda 表达式的表达式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6302.fs)]

下一个示例显示`using`带有函数的表达式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6303.fs)]

请注意, 该函数可能是已应用了某些参数的函数。 下面的代码示例展示了此操作。 它会创建一个包含字符串`XYZ`的文件。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6304.fs)]

`using` 函数`use`和绑定几乎等效于完成相同操作的方法。 关键字可以更好地控制调用`Dispose`的时间。 `using` 使用`using`时, `Dispose`在函数或 lambda 表达式的末尾调用`use` ; 当使用关键字时, `Dispose`将在包含代码块的末尾调用。 一般情况下, 您应该优先使用`use`而不是`using`函数。

## <a name="see-also"></a>请参阅

- [F# 语言参考](index.md)
