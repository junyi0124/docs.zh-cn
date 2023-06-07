---
title: lock 语句 - C# 参考
description: 使用 C# lock 语句同步对共享资源的线程访问
ms.date: 04/02/2020
f1_keywords:
- lock_CSharpKeyword
- lock
helpviewer_keywords:
- lock keyword [C#]
ms.assetid: 656da1a4-707e-4ef6-9c6e-6d13b646af42
ms.openlocfilehash: 6e9a6975977588ba7692c925d7940cd2ec26671f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406685"
---
# <a name="lock-statement-c-reference"></a>lock 语句（C# 参考）

`lock` 语句获取给定对象的互斥 lock，执行语句块，然后释放 lock。 持有 lock 时，持有 lock 的线程可以再次获取并释放 lock。 阻止任何其他线程获取 lock 并等待释放 lock。

`lock` 语句具有以下格式

```csharp
lock (x)
{
    // Your code...
}
```

其中 `x` 是[引用类型](reference-types.md)的表达式。 它完全等同于

```csharp
object __lockObj = x;
bool __lockWasTaken = false;
try
{
    System.Threading.Monitor.Enter(__lockObj, ref __lockWasTaken);
    // Your code...
}
finally
{
    if (__lockWasTaken) System.Threading.Monitor.Exit(__lockObj);
}
```

由于该代码使用 [try...finally](try-finally.md) 块，即使在 `lock` 语句的正文中引发异常，也会释放 lock。

在 `lock` 语句的正文中不能使用 [await 运算符](../operators/await.md)。

## <a name="guidelines"></a>准则

当同步对共享资源的线程访问时，请锁定专用对象实例（例如，`private readonly object balanceLock = new object();`）或另一个不太可能被代码无关部分用作 lock 对象的实例。 避免对不同的共享资源使用相同的 lock 对象实例，因为这可能导致死锁或锁争用。 具体而言，避免将以下对象用作 lock 对象：

- `this`（调用方可能将其用作 lock）。
- <xref:System.Type> 实例（可以通过 [typeof](../operators/type-testing-and-cast.md#typeof-operator) 运算符或反射获取）。
- 字符串实例，包括字符串文本，（这些可能是[暂存的](/dotnet/api/system.string.intern#remarks)）。

尽可能缩短持有锁的时间，以减少锁争用。

## <a name="example"></a>示例

以下示例定义了一个 `Account` 类，该类通过锁定专用的 `balanceLock` 实例来同步对其专用 `balance` 字段的访问。 使用相同的实例进行锁定可确保尝试同时调用 `Debit` 或 `Credit` 方法的两个线程无法同时更新 `balance` 字段。

[!code-csharp[lock-statement-example](snippets/LockStatementExample.cs)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [lock 语句](~/_csharplang/spec/statements.md#the-lock-statement)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 关键字](index.md)
- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- <xref:System.Threading.SpinLock?displayProperty=nameWithType>
- <xref:System.Threading.Interlocked?displayProperty=nameWithType>
- [同步基元概述](../../../standard/threading/overview-of-synchronization-primitives.md)
