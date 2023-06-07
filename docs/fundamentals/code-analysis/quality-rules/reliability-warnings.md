---
title: '可靠性规则 (代码分析) '
description: 了解代码分析可靠性规则。
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.reliabilityrules
helpviewer_keywords:
- rules, reliability
- reliability rules
- managed code analysis rules, reliability rules
author: gewarren
ms.author: gewarren
ms.openlocfilehash: a747dd4dcda351a1ddb0f3d069bb7bac895c32f8
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2020
ms.locfileid: "96590536"
---
# <a name="reliability-rules"></a>可靠性规则

可靠性规则支持库和应用程序的可靠性，如正确的内存和线程使用。 可靠性规则包括：

|规则|描述|
|----------|-----------------|
|[CA2000:丢失范围之前释放对象](ca2000.md)|由于可能发生异常事件，导致对象的终结器无法运行，因此，应显式释放对象，以避免对该对象的所有引用超出范围。|
|[CA2002:不要锁定具有弱标识的对象](ca2002.md)|当可以跨应用程序域边界直接进行访问对象时，则认为该对象具有弱标识。 对于尝试获取对具有弱标识的对象的锁的线程，该线程可能会被其他应用程序域中持有对同一对象的锁的另一线程所阻止。|
|[CA2007：不直接等待任务](ca2007.md)|异步方法会[awaits](../../../csharp/language-reference/operators/await.md)直接等待 <xref:System.Threading.Tasks.Task> 。|
|[CA2008：不要在未传递 TaskScheduler 的情况下创建任务](ca2008.md)|任务创建或继续操作使用未指定参数的方法重载 <xref:System.Threading.Tasks.TaskScheduler> 。|
|[CA2009：请勿对 ImmutableCollection 值调用 ToImmutableCollection](ca2009.md)|`ToImmutable` 不必要地对命名空间中的不可变集合调用方法 <xref:System.Collections.Immutable> 。|
|[CA2011:请勿在其资源库中分配属性](ca2011.md) | 属性在其自身的 [set 访问器](../../../csharp/programming-guide/classes-and-structs/using-properties.md#the-set-accessor)中被意外赋值。 |
|[CA2012:正确使用 ValueTask](ca2012.md) | 从成员调用返回的 ValueTasks 将直接等待。  多次尝试使用 ValueTask 或在已知完成前直接访问一个结果可能会导致异常或损坏。  忽略此类 ValueTask 可能表明出现功能 bug，并可能会降低性能。 |
|[CA2013:请勿将 ReferenceEquals 与值类型结合使用](ca2013.md) | 使用比较值时 <xref:System.Object.ReferenceEquals%2A?displayProperty=fullName> ，如果 objA 和 objB 是值类型，则在将其传递给方法之前将它们装箱 <xref:System.Object.ReferenceEquals%2A> 。 这意味着，即使 objA 和 objB 都表示值类型的同一个实例，该方法也会 <xref:System.Object.ReferenceEquals%2A> 返回 false。 |
|[CA2014：不要在循环中使用 stackalloc。](ca2014.md) | Stackalloc 分配的堆栈空间仅在当前方法的调用结束时释放。  在循环中使用此方法可能会导致无限堆栈增长，并最终产生堆栈溢出情况。 |
|[CA2015：不要为派生自 MemoryManager T 的类型定义终结器 &lt;&gt;](ca2015.md) | 将终结器添加到从派生的类型 <xref:System.Buffers.MemoryManager%601> 时，可能会允许在仍在使用时释放内存 <xref:System.Span%601> 。 |
|[CA2016：将 CancellationToken 参数转发到采用一个该参数的方法](ca2016.md) | 将 `CancellationToken` 参数转发到方法，这些方法采用一个来确保正确传播操作取消通知，或显式传递 `CancellationToken.None` 以指示有意不传播标记。 |
