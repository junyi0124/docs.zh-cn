---
title: 基于事件的异步模式 (EAP)
description: 点击链接即可参阅文章，了解基于事件的异步模式 (EAP) 的相关信息，如实现、最佳做法、实现 EAP 客户端等。
ms.date: 07/23/2018
helpviewer_keywords:
- asynchronous calls
- asynchronous programming, design patterns
- asynchronous programming
ms.assetid: c6baed9f-2a25-4728-9a9a-53b7b14840cf
ms.openlocfilehash: c15bb6c9ec6ea737e6f240376e12bf8de3aa61bc
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830397"
---
# <a name="event-based-asynchronous-pattern-eap"></a>基于事件的异步模式 (EAP)

有多种方式可向客户端代码公开异步功能。 基于事件的异步模式规定了类呈现异步行为的一种方式。  
  
> [!NOTE]
> 从 .NET Framework 4 开始，任务并行库为异步编程和并行编程提供了一种新模型。 有关详细信息，请参阅 “[任务并行库 (TPL)](../parallel-programming/task-parallel-library-tpl.md)” 和 “[基于任务的异步模式 (TAP)](task-based-asynchronous-pattern-tap.md)”。
  
## <a name="in-this-section"></a>本节内容

 [基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)  
 描述基于事件的异步模式如何在隐藏多线程设计中固有的许多复杂问题的同时，提供多线程应用程序的优点。  
  
 [实现基于事件的异步模式](implementing-the-event-based-asynchronous-pattern.md)  
 描述打包具有异步功能的类的标准方式。  
  
 [实现基于事件的异步模式的最佳做法](best-practices-for-implementing-the-event-based-asynchronous-pattern.md)  
 描述根据基于事件的异步模式公开异步功能的需求。  
  
 [确定何时实现基于事件的异步模式](deciding-when-to-implement-the-event-based-asynchronous-pattern.md)  
 描述如何确定何时应选择实现基于事件的异步模式而不是由[异步编程模型 (APM)](asynchronous-programming-model-apm.md) 表示的 <xref:System.IAsyncResult> 模式
  
 [如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)  
 说明如何创建实现基于事件的异步模式的组件。 它是使用 <xref:System.ComponentModel?displayProperty=nameWithType> 命名空间的帮助器类实现的，这可确保该组件在任何应用程序模型下均可正常工作。  

 [如何：实现基于事件的异步模式的客户端](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)  
 说明如何创建使用实现基于事件的异步模式的组件的客户端。
  
 [如何：使用支持基于事件的异步模式的组件](how-to-use-components-that-support-the-event-based-asynchronous-pattern.md)  
 描述如何使用支持基于事件的异步模式的组件。  
  
## <a name="reference"></a>参考

 <xref:System.ComponentModel.AsyncOperation>  
 描述 <xref:System.ComponentModel.AsyncOperation> 类并提供指向其所有成员的链接。  
  
 <xref:System.ComponentModel.AsyncOperationManager>  
 描述 <xref:System.ComponentModel.AsyncOperationManager> 类并提供指向其所有成员的链接。  
  
 <xref:System.ComponentModel.BackgroundWorker>  
 描述 <xref:System.ComponentModel.BackgroundWorker> 组件并提供指向其所有成员的链接。  
  
## <a name="related-sections"></a>相关章节

 [任务并行库 (TPL)](../parallel-programming/task-parallel-library-tpl.md)  
 描述用于异步和并行操作的编程模型。  
  
 [线程处理](../threading/index.md)  
 描述 .NET 中的多线程处理功能。  
  
## <a name="see-also"></a>请参阅

- [托管线程处理的最佳做法](../threading/managed-threading-best-practices.md)
- [事件](../events/index.md)
- [异步编程设计模式](index.md)
