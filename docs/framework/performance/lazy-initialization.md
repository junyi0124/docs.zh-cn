---
title: 延迟初始化
description: 探索 .NET 中的迟缓初始化，性能提高意味着对象创建被延迟到首次使用该对象时。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- lazy initialization in .NET, introduction
ms.assetid: 56b4ae5c-4745-44ff-ad78-ffe4fcde6b9b
ms.openlocfilehash: 6cbda2bd45f2cdc5085ca30af878824069fb019a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242170"
---
# <a name="lazy-initialization"></a>延迟初始化

对象的迟缓初始化意味着推迟创建该对象，直到它被首次使用。  (本主题中，术语 " *迟缓初始化* " 和 " *迟缓实例化* " 是同义词。 ) 迟缓初始化主要用于提高性能，避免浪费计算，并降低程序内存要求。 以下是常见方案：  
  
- 对象创建成本高且程序可能不会使用它。 例如，假定内存中有具有 `Orders` 属性的 `Customer` 对象，该对象包含大量 `Order` 对象，初始化这些对象需要数据库连接。 如果用户永远不要求显示 Orders 或在计算中使用该数据，则无需使用系统内存或计算周期来创建它。 通过使用 `Lazy<Orders>` 来声明 `Orders` 对象用于迟缓初始化，可以避免在不使用该对象时浪费系统资源。  
  
- 对象创建成本高，且希望将其创建推迟到其他高成本操作完成后。 例如，假定程序在启动时加载多个对象实例，但是只需立即加载其中一部分。 可以通过推迟初始化不需要的对象，直到创建所需对象，提升程序的启动性能。  
  
 虽然可以编写自己的代码来执行迟缓初始化，但我们建议使用 <xref:System.Lazy%601>。 <xref:System.Lazy%601> 及其相关的类型还支持线程安全并提供一致的异常传播策略。  
  
 下表列出了 .NET Framework 版本 4 提供的在不同方案中启用迟缓初始化的类型。  
  
|类型|描述|  
|----------|-----------------|  
|<xref:System.Lazy%601>|为任何类库或用户定义类型提供迟缓初始化语义的包装类。|  
|<xref:System.Threading.ThreadLocal%601>|类似于 <xref:System.Lazy%601>，除了该包装类基于线程本地提供迟缓初始化语义。 每个线程都可以访问自己唯一的值。|  
|<xref:System.Threading.LazyInitializer>|为对象的迟缓初始化提供高级 `static`（Visual Basic 中的 `Shared`）方法，无需支付类的成本。|  
  
## <a name="basic-lazy-initialization"></a>基本迟缓初始化  

 若要定义迟缓初始化类型（例如 `MyType`），使用 `Lazy<MyType>`（Visual Basic 中的 `Lazy(Of MyType)`），如以下示例所示。 如果没有在 <xref:System.Lazy%601> 构造函数中传入委托，则在首次访问值属性时，将使用 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 创建包装类型。 如果该类型没有无参数构造函数，则会引发运行时异常。  
  
 在以下示例中，假定 `Orders` 是包含从数据库检索的大量 `Order` 对象的类。 `Customer` 对象包含 `Orders` 的实例，但根据用户操作，可能不需要 `Orders` 对象的数据。  
  
 [!code-csharp[Lazy#1](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#1)]
 [!code-vb[Lazy#1](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#1)]  
  
 还可以在 <xref:System.Lazy%601> 构造函数中传递委托（该委托在创建时调用包装类型上的特定构造函数重载），并执行所需的任何其他初始化步骤，如以下示例所示。  
  
 [!code-csharp[Lazy#2](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#2)]
 [!code-vb[Lazy#2](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#2)]  
  
 创建迟缓对象后，在首次访问迟缓变量的 <xref:System.Lazy%601.Value%2A> 属性前，不会创建 `Orders` 的实例。 首次访问时，会创建并返回包装类型，并将其存储起来以便将来访问。  
  
 [!code-csharp[Lazy#3](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#3)]
 [!code-vb[Lazy#3](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#3)]  
  
 <xref:System.Lazy%601> 对象总是返回其初始化的相同对象或值。 因此，<xref:System.Lazy%601.Value%2A> 属性为只读。 如果 <xref:System.Lazy%601.Value%2A> 存储了引用类型，将无法为其分配新对象。 但 (可以更改其可设置的公共字段和属性的值。 ) 如果 <xref:System.Lazy%601.Value%2A> 存储值类型，则不能修改其值。 但是，可以通过再次调用变量构造函数，使用新参数来创建新变量。  
  
 [!code-csharp[Lazy#4](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#4)]
 [!code-vb[Lazy#4](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#4)]  
  
 新的迟缓实例与之前的实例一样，不会实例化 `Orders`，直到首次访问其 <xref:System.Lazy%601.Value%2A> 属性。  
  
### <a name="thread-safe-initialization"></a>线程安全初始化  

 默认情况下，<xref:System.Lazy%601> 对象是线程安全的。 也就是说，如果构造函数没有指定线程安全性的类型，该函数创建的 <xref:System.Lazy%601> 对象是线程安全的。 在多线程方案中，访问线程安全 <xref:System.Lazy%601> 对象的 <xref:System.Lazy%601.Value%2A> 属性的第一个线程会为所有线程上的所有后续访问对其初始化，且所有线程共享相同的数据。 因此，哪个线程初始化对象并不重要，争用条件是良性的。  
  
> [!NOTE]
> 可以通过使用异常缓存将此一致性扩展到错误条件。 有关详细信息，请参阅下一部分[迟缓对象的异常](lazy-initialization.md#ExceptionsInLazyObjects)。  
  
 以下示例演示了相同的 `Lazy<int>` 实例对于三个单独的线程具有相同的值。  
  
 [!code-csharp[Lazy#8](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#8)]
 [!code-vb[Lazy#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#8)]  
  
 如果每个线程需要单独的数据，使用 <xref:System.Threading.ThreadLocal%601> 类型，如本主题后面所述。  
  
 一些 <xref:System.Lazy%601> 构造函数具有名为 `isThreadSafe` 的布尔参数，该参数用于指定是否从多线程访问 <xref:System.Lazy%601.Value%2A> 属性。 如果想要仅从一个线程访问属性，则传入 `false` 以获取适度的性能优势。 如果想要从多线程访问属性，则传入 `true` 以指示 <xref:System.Lazy%601> 实例正确处理争用条件（初始化时一个线程引发异常）。  
  
 一些 <xref:System.Lazy%601> 构造函数具有命名为 `mode` 的 <xref:System.Threading.LazyThreadSafetyMode> 参数。 这些构造函数可提供其他线程安全模式。 下表显示了 <xref:System.Lazy%601> 对象的线程安全性如何受到指定线程安全性的构造函数参数的影响。 每个构造函最多具有一个此类参数。  
  
|对象的线程安全性|`LazyThreadSafetyMode``mode`参数|布尔 `isThreadSafe` 参数|没有线程安全性参数|  
|---------------------------------|---------------------------------------------|--------------------------------------|---------------------------------|  
|完全线程安全；一次只有一个线程尝试初始化值。|<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>|`true`|是的。|  
|非线程安全。|<xref:System.Threading.LazyThreadSafetyMode.None>|`false`|不适用。|  
|完全线程安全；线程争用以初始化值。|<xref:System.Threading.LazyThreadSafetyMode.PublicationOnly>|不适用。|不适用。|  
  
 如此表所示，为 `mode` 参数指定 <xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication?displayProperty=nameWithType> 与为 `isThreadSafe` 参数指定 `true` 相同，并且指定 <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType> 与指定 `false` 相同。  
  
 指定 <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly?displayProperty=nameWithType> 允许多个线程尝试初始化 <xref:System.Lazy%601> 实例。 只有一个线程可以赢得此争用，而其他所有线程都将接收成功的线程初始化的值。 如果在初始化期间，某个线程引发了异常，则此线程不会接收成功的线程设置的值。 不会缓存异常，因此随后尝试访问 <xref:System.Lazy%601.Value%2A> 属性可能会导致初始化成功。 这与其他模式中的异常处理方式不同，下面将对此进行描述。 有关详细信息，请参见 <xref:System.Threading.LazyThreadSafetyMode> 枚举。  
  
<a name="ExceptionsInLazyObjects"></a>

## <a name="exceptions-in-lazy-objects"></a>迟缓对象的异常  

 如前所述，<xref:System.Lazy%601> 对象始终返回其初始化的相同对象或值，因此 <xref:System.Lazy%601.Value%2A> 属性是只读的。 如果启用异常缓存，则此永久性还会扩展到异常行为。 如果延迟初始化的对象已启用异常缓存并在第一次访问该属性时从其初始化方法引发异常 <xref:System.Lazy%601.Value%2A> ，则每次访问该属性的尝试都会引发相同的异常 <xref:System.Lazy%601.Value%2A> 。 也就是说，即使在多线程方案中，包装类型的构造函数也绝不会被重新调用。 因此，<xref:System.Lazy%601> 对象不能在一次访问时引发异常，并在随后访问时返回值。  
  
 当使用任何采用初始化方法（`valueFactory` 参数）的 <xref:System.Lazy%601?displayProperty=nameWithType> 构造函数时，都会启用异常缓存；例如，当使用 `Lazy(T)(Func(T))` 构造函数时，会启用异常缓存。 如果构造函数还使用 <xref:System.Threading.LazyThreadSafetyMode> 值（`mode` 参数），请指定 <xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication?displayProperty=nameWithType> 或 <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType>。 指定初始化方法可启用这两种模式的异常缓存。 初始化方法非常简单。 例如，它可能会调用的无参数构造函数 `T` ： `new Lazy<Contents>(() => new Contents(), mode)` 在 c # 中，或 `New Lazy(Of Contents)(Function() New Contents())` 在 Visual Basic 中。 如果使用不指定初始化方法的 <xref:System.Lazy%601?displayProperty=nameWithType> 构造函数，则不会缓存 `T` 无参数构造函数引发的异常。 有关详细信息，请参见 <xref:System.Threading.LazyThreadSafetyMode> 枚举。  
  
> [!NOTE]
> 如果通过将 `isThreadSafe` 构造函数参数设置为 `false` 或将 `mode` 构造函数参数设置为 <xref:System.Threading.LazyThreadSafetyMode.None?displayProperty=nameWithType> 来创建 <xref:System.Lazy%601> 对象，则必须从单个线程访问 <xref:System.Lazy%601> 对象或提供你自己的同步。 这适用于对象的所有方面，包括异常缓存。  
  
 如上一节所述，通过指定 <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly?displayProperty=nameWithType> 创建的 <xref:System.Lazy%601> 对象会以不同方式处理异常。 通过 <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly>，多个线程可以通过争用来初始化 <xref:System.Lazy%601> 实例。 在这种情况下，不会缓存异常，并且可以继续尝试访问 <xref:System.Lazy%601.Value%2A> 属性，直到初始化成功。  
  
 下表总结了 <xref:System.Lazy%601> 构造函数控制异常缓存的方式。  
  
|构造函数|线程安全性|使用初始化方法|异常被缓存|  
|-----------------|------------------------|--------------------------------|---------------------------|  
|Lazy(T)()|(<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>)|否|否|  
|Lazy(T)(Func(T))|(<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>)|是|是|  
|Lazy(T)(Boolean)|`True` (<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>) 或 `false` (<xref:System.Threading.LazyThreadSafetyMode.None>)|否|否|  
|Lazy(T)(Func(T), Boolean)|`True` (<xref:System.Threading.LazyThreadSafetyMode.ExecutionAndPublication>) 或 `false` (<xref:System.Threading.LazyThreadSafetyMode.None>)|是|是|  
|Lazy(T)(LazyThreadSafetyMode)|用户指定|否|否|  
|Lazy(T)(Func(T), LazyThreadSafetyMode)|用户指定|是|如果用户指定 <xref:System.Threading.LazyThreadSafetyMode.PublicationOnly> 则为 no；否则为 yes。|  
  
## <a name="implementing-a-lazy-initialized-property"></a>实现迟缓初始化的属性  

 若要通过使用迟缓初始化实现公共属性，则将该属性的支持字段定义为 <xref:System.Lazy%601>，并从该属性的 `get` 访问器返回 <xref:System.Lazy%601.Value%2A> 属性。  
  
 [!code-csharp[Lazy#5](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#5)]
 [!code-vb[Lazy#5](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#5)]  
  
 <xref:System.Lazy%601.Value%2A> 属性是只读的；因此，将其公开的属性没有 `set` 访问器。 如果需要由 <xref:System.Lazy%601> 对象支持的读取/写入属性，则 `set` 访问器必须创建一个新的 <xref:System.Lazy%601> 对象并将其分配到后备存储。 `set` 访问器必须创建一个 lambda 表达式（该表达式返回传递给 `set` 访问器的新属性值），并将该 lambda 表达式传递给新的 <xref:System.Lazy%601> 对象的构造函数。 下一次访问 <xref:System.Lazy%601.Value%2A> 属性将导致新 <xref:System.Lazy%601> 的初始化，并且其 <xref:System.Lazy%601.Value%2A> 属性此后会返回已分配给该属性的新值。 进行此复杂安排的原因是保留内置于 <xref:System.Lazy%601> 的多线程保护。 否则，属性访问器必须缓存由 <xref:System.Lazy%601.Value%2A> 属性返回的第一个值并仅修改缓存值，而用户必须编写自己的线程安全代码才能执行此操作。 因为由 <xref:System.Lazy%601> 对象支持的读取/写入属性需要其他初始化，因此此性能可能不可接受。 此外，可能需要额外的协调以避免资源库与 getter 之间的争用条件，具体取决于特定方案。  
  
## <a name="thread-local-lazy-initialization"></a>线程本地迟缓初始化  

 在一些多线程方案中，可能需要为每个线程提供其专用数据。 此类数据称为线程本地数据。 在 .NET Framework 版本 3.5 及先前版本中，可以将 `ThreadStatic` 属性应用到静态变量，使其成为本地线程。 然而，使用 `ThreadStatic` 属性可能会导致细微的错误。 例如，即使是基本的初始化语句也将导致仅在访问其的首个线程上初始化变量，如以下示例所示。  
  
 [!code-csharp[Lazy#6](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#6)]
 [!code-vb[Lazy#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#6)]  
  
 在所有其他线程上，变量将通过使用其默认值（零）进行初始化。 作为 .NET Framework 版本 4 中的替代方法，可以使用 <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> 类型创建一个基于实例的线程本地变量，该变量由你提供的 <xref:System.Action%601> 委托在所有线程上进行初始化。 在以下示例中，访问 `counter` 的所有线程都将其启动值视为 1。  
  
 [!code-csharp[Lazy#7](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#7)]
 [!code-vb[Lazy#7](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#7)]  
  
 <xref:System.Threading.ThreadLocal%601> 包装其对象的方式与 <xref:System.Lazy%601> 非常相似，但存在以下主要区别：  
  
- 每个线程都通过使用其自己的专有数据来初始化线程本地变量，这些数据不能从其他线程访问。  
  
- <xref:System.Threading.ThreadLocal%601.Value%2A?displayProperty=nameWithType> 属性可以读取和写入，并且可以修改任意次数。 这可能会影响异常传播，例如，一个 `get` 操作可能会引发异常，但下一个操作可能会成功初始化该值。  
  
- 如果没有提供初始化委托，<xref:System.Threading.ThreadLocal%601> 将通过使用该类型的默认值初始化其包装类型。 在这一方面，<xref:System.Threading.ThreadLocal%601> 与 <xref:System.ThreadStaticAttribute> 属性一致。  
  
 以下示例显示了每个访问 `ThreadLocal<int>` 实例的线程都将获取其数据的唯一副本。  
  
 [!code-csharp[Lazy#9](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#9)]
 [!code-vb[Lazy#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#9)]  
  
## <a name="thread-local-variables-in-parallelfor-and-foreach"></a>Parallel.For 和 ForEach 中的线程本地变量  

 当使用 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 方法或 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 方法来并行循环访问数据源时，可以使用包含针对线程本地数据的内置支持的重载。 在这些方法中，通过使用本地委托来创建、访问和清理数据，可以实现线程本地性。 有关详细信息，请参阅[如何：编写具有线程局部变量的 Parallel.For 循环](../../standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md)和[如何：使用分区本地变量编写 Parallel.ForEach 循环](../../standard/parallel-programming/how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md)。  
  
## <a name="using-lazy-initialization-for-low-overhead-scenarios"></a>将迟缓初始化用于开销较低的方案  

 在必须迟缓初始化大量对象的方案中，你可能会认为在 <xref:System.Lazy%601> 中包装每个对象需要过多的内存或过多的计算资源。 或者，你可能对如何公开迟缓初始化有严格要求。 在这种情况下，可以使用 <xref:System.Threading.LazyInitializer?displayProperty=nameWithType> 类的 `static`（Visual Basic 中的 `Shared`）方法来迟缓初始化每个对象，而不会将其包装在 <xref:System.Lazy%601> 的实例中。  
  
 在以下示例中，假定你仅具有所需的迟缓初始化的单个`Order` 对象，而不是将整个 `Orders` 对象包装在一个 <xref:System.Lazy%601> 对象中。  
  
 [!code-csharp[Lazy#10](../../../samples/snippets/csharp/VS_Snippets_Misc/lazy/cs/cs_lazycodefile.cs#10)]
 [!code-vb[Lazy#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/lazy/vb/lazy_vb.vb#10)]  
  
 在此示例中，请注意循环的每次迭代都会调用初始化过程。 在多线程方案中，所有线程都会知道调用初始化过程的第一个线程的值。 后续线程也会调用初始化过程，但不会使用其值。 如果这种潜在的争用条件是不可接受的，请使用 <xref:System.Threading.LazyInitializer.EnsureInitialized%2A?displayProperty=nameWithType> 的重载，获取布尔参数和同步对象。  
  
## <a name="see-also"></a>请参阅

- [托管线程处理基本知识](../../standard/threading/managed-threading-basics.md)
- [线程与线程处理](../../standard/threading/threads-and-threading.md)
- [任务并行库 (TPL)](../../standard/parallel-programming/task-parallel-library-tpl.md)
- [如何：执行对象的延迟初始化](how-to-perform-lazy-initialization-of-objects.md)
