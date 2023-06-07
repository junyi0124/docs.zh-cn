---
title: 确定何时实现基于事件的异步模式
ms.date: 03/30/2017
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET], asynchronous
- AsyncOperationManager class
- threading [.NET], asynchronous features
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: a00046aa-785d-4f7f-a8e5-d06475ea50da
ms.openlocfilehash: 096b3e9e5732989ff5e85a5b6df4ca413e29cbcd
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830410"
---
# <a name="deciding-when-to-implement-the-event-based-asynchronous-pattern"></a>确定何时实现基于事件的异步模式

基于事件的异步模式可用于公开类的异步行为。 通过引入此模式，.NET 定义了下面两种用于公开异步行为的模式：基于 <xref:System.IAsyncResult?displayProperty=nameWithType> 接口的异步模式和基于事件的模式。 本文介绍了何时适合实现这两种模式。

若要详细了解如何使用 <xref:System.IAsyncResult> 接口进行异步编程，请参阅[异步编程模型 (APM)](asynchronous-programming-model-apm.md)。

## <a name="general-principles"></a>一般原则

一般来说，应尽量使用基于事件的异步模式公开异步功能。 不过，基于事件的模式无法满足一些要求。 在这种情况下，除了基于事件的模式外，可能还需要实现 <xref:System.IAsyncResult> 模式。

> [!NOTE]
> 实现了 <xref:System.IAsyncResult> 模式但没有实现基于事件的模式，这种情况很少见。

## <a name="guidelines"></a>准则

下面列出了应在何时实现基于事件的异步模式的相关指南：

- 将基于事件的模式用作公开类的异步行为的默认 API。

- 如果类主要用于客户端应用（例如，Windows 窗体），请勿公开 <xref:System.IAsyncResult> 模式。

- 仅在需要满足特定要求时，才公开 <xref:System.IAsyncResult> 模式。 例如，为了与现有 API 兼容，可能需要公开 <xref:System.IAsyncResult> 模式。

- 请勿在不公开基于事件的模式的情况下公开 <xref:System.IAsyncResult> 模式。

- 如果必须公开 <xref:System.IAsyncResult> 模式，请以高级选项的形式这样做。 例如，如果生成代理对象，默认生成的是基于事件的模式，并含用于生成 <xref:System.IAsyncResult> 模式的选项。

- 在 <xref:System.IAsyncResult> 模式实现的基础之上生成基于事件的模式实现。

- 避免对相同的类公开基于事件的模式和 <xref:System.IAsyncResult> 模式。 请对“高级”类公开基于事件的模式，并对“低级”类公开 <xref:System.IAsyncResult> 模式。 例如，比较 <xref:System.Net.WebClient> 组件上基于事件的模式与 <xref:System.Web.HttpRequest> 类上的 <xref:System.IAsyncResult> 模式。

  - 出于兼容性需要，可以对相同的类公开基于事件的模式和 <xref:System.IAsyncResult> 模式。 例如，如果已释放使用 <xref:System.IAsyncResult> 模式的 API，需要保留 <xref:System.IAsyncResult> 模式，以实现向后兼容性。

  - 如果生成的对象模型复杂性远远超过分离实现的好处，请对相同的类公开基于事件的模式和 <xref:System.IAsyncResult> 模式。 对一个类公开两种模式优于避免公开基于事件的模式。

  - 如果必须对一个类公开基于事件的模式和 <xref:System.IAsyncResult> 模式，请将 <xref:System.ComponentModel.EditorBrowsableAttribute> 设置为 <xref:System.ComponentModel.EditorBrowsableState.Advanced>，以将 <xref:System.IAsyncResult> 模式实现标记为高级功能。 这会指示设计环境（如 Visual Studio IntelliSense）不显示 <xref:System.IAsyncResult> 属性和方法。 这些属性和方法仍完全可用，这样做只是为了让使用 IntelliSense 的开发人员对 API 更加明确。

## <a name="criteria-for-exposing-the-iasyncresult-pattern-in-addition-to-the-event-based-pattern"></a>除了基于事件的模式外还公开 IAsyncResult 模式的条件

虽然基于事件的异步模式在上述情况下有许多优点，但也有一些缺点。如果性能是最重要的要求，应注意这些缺点。

<xref:System.IAsyncResult> 模式比基于事件的模式更适用 的情况有三种：

- 对 <xref:System.IAsyncResult> 阻止等待操作

- 对多个 <xref:System.IAsyncResult> 对象阻止等待操作

- 对 <xref:System.IAsyncResult> 轮询完成状态

虽然可以使用基于事件的模式来处理这些情况，但这样做比使用 <xref:System.IAsyncResult> 模式更不方便。

开发人员经常对性能要求通常很高的服务使用 <xref:System.IAsyncResult> 模式。 例如，轮询完成状态就是一种高性能服务器技术。

此外，基于事件的模式的效率低于 <xref:System.IAsyncResult> 模式，因为前者创建的对象更多（尤其是 <xref:System.EventArgs>），并且跨线程同步。

下面列出了一些在决定使用 <xref:System.IAsyncResult> 模式时要遵循的建议：

- 仅在特别需要对 <xref:System.Threading.WaitHandle> 或<xref:System.IAsyncResult> 对象的支持时，才公开 <xref:System.IAsyncResult> 模式。

- 仅在有使用 <xref:System.IAsyncResult> 模式的现有 API 时，才公开 <xref:System.IAsyncResult> 模式。

- 如果有基于 <xref:System.IAsyncResult> 模式的现有 API，还请考虑在下一个版本中公开基于事件的模式。

- 仅在有高性能要求，且已验证无法通过基于事件的模式满足这些要求，但可以通过 <xref:System.IAsyncResult> 模式满足时，才公开 <xref:System.IAsyncResult> 模式。

## <a name="see-also"></a>另请参阅

- [如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)
- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
- [实现基于事件的异步模式](implementing-the-event-based-asynchronous-pattern.md)
- [实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)
