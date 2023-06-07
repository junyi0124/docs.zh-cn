---
title: 如何：实现支持基于事件的异步模式的组件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET], asynchronous
- Asynchronous Pattern
- AsyncOperationManager class
- threading [.NET], asynchronous features
- components [.NET], asynchronous
- AsyncOperation class
- threading [Windows Forms], asynchronous features
- AsyncCompletedEventArgs class
ms.assetid: 61f676b5-936f-40f6-83ce-f22805ec9c2f
ms.openlocfilehash: ef7363cd1c5161217fa4cf74dbfae9dee86fa76f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697735"
---
# <a name="how-to-implement-a-component-that-supports-the-event-based-asynchronous-pattern"></a>如何：实现支持基于事件的异步模式的组件

若要编写的类有一些可能会带来明显延迟的操作，请考虑按照[基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)中的步骤操作，为它实现异步功能。  
  
 本演练展示了如何创建实现基于事件的异步模式的组件。 此组件是使用 <xref:System.ComponentModel?displayProperty=nameWithType> 命名空间中的帮助程序类进行实现，这可确保它在任何应用模型（包括 ASP.NET、控制台应用和 Windows 窗体应用）下都能正常运行。 也可以使用 <xref:System.Windows.Forms.PropertyGrid> 控件和自己的自定义设计器来设计此组件。  
  
 本演练使用异步计算质数的应用。 应用有主用户界面 (UI) 线程，以及用于每次质数计算的线程。 尽管测试大数字是否为质数需要花费很长时间，但主 UI 线程不会被此延迟中断，并且窗体在计算期间仍为响应式。 不仅可以同时运行计算（数量不限），还能选择性地取消挂起的计算。  
  
 本演练涉及以下任务：  
  
- 创建组件  
  
- 定义公共异步事件和委托  
  
- 定义专用委托  
  
- 实现公共事件  
  
- 实现完成方法  
  
- 实现工作方法  
  
- 实现启动和取消方法  
  
 若要将本主题中的代码复制为一个代码清单，请参阅[如何：实现基于事件的异步模式的客户端](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)。  
  
## <a name="creating-the-component"></a>创建组件  

 第一步是，创建实现基于事件的异步模式的组件。  
  
### <a name="to-create-the-component"></a>创建组件的具体步骤  
  
- 创建继承自 <xref:System.ComponentModel.Component> 的类 `PrimeNumberCalculator`。  
  
## <a name="defining-public-asynchronous-events-and-delegates"></a>定义公共异步事件和委托  

 组件使用事件与客户端进行通信。 _MethodName_**Completed** 事件预警客户端注意异步任务完成，_MethodName_**ProgressChanged** 事件向客户端告知异步任务的进度。  
  
### <a name="to-define-asynchronous-events-for-clients-of-your-component"></a>若要定义组件客户端的异步事件，请执行以下步骤：  
  
1. 在文件顶部，导入 <xref:System.Threading?displayProperty=nameWithType> 和 <xref:System.Collections.Specialized?displayProperty=nameWithType> 命名空间。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#11](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#11)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#11](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#11)]  
  
2. 在 `PrimeNumberCalculator` 类定义前面，声明进度和完成事件的委托。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#7](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#7)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#7](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#7)]  
  
3. 在 `PrimeNumberCalculator` 类定义中，声明向客户端报告进度和完成事件的事件。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#8](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#8)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#8](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#8)]  
  
4. 在 `PrimeNumberCalculator` 类定义后面，派生 `CalculatePrimeCompletedEventArgs` 类，向 `CalculatePrimeCompleted` 事件的客户端事件处理程序报告每次计算的结果。 除了 `AsyncCompletedEventArgs` 属性外，客户端还可以使用此类确定测试的数字是什么、数字是否为质数，以及第一个除数是什么（如果不是质数的话）。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#6](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#6)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#6](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#6)]  
  
## <a name="checkpoint"></a>检查点  

 此时，可以生成组件。  
  
### <a name="to-test-your-component"></a>测试组件的具体步骤  
  
- 编译组件。  
  
     将看到下面两个编译器警告：  
  
    ```console  
    warning CS0067: The event 'AsynchronousPatternExample.PrimeNumberCalculator.ProgressChanged' is never used  
    warning CS0067: The event 'AsynchronousPatternExample.PrimeNumberCalculator.CalculatePrimeCompleted' is never used  
    ```  
  
     这些警告会在下一部分中得到清除。  
  
## <a name="defining-private-delegates"></a>定义专用委托  

 `PrimeNumberCalculator` 组件的异步特性是通过特殊的 <xref:System.Threading.SendOrPostCallback> 委托在内部进行实现。 <xref:System.Threading.SendOrPostCallback> 表示对 <xref:System.Threading.ThreadPool> 线程执行的回调方法。 回调方法必须有需要使用单个 <xref:System.Object> 类型参数的签名。也就是说，需要在包装类中的各委托之间传递状态。 有关详细信息，请参阅 <xref:System.Threading.SendOrPostCallback>。  
  
### <a name="to-implement-your-components-internal-asynchronous-behavior"></a>若要实现组件的内部异步行为，请执行以下操作：  
  
1. 在 `PrimeNumberCalculator` 类中声明并创建 <xref:System.Threading.SendOrPostCallback> 委托。 在 `InitializeDelegates` 实用工具方法中创建 <xref:System.Threading.SendOrPostCallback> 对象。  
  
     需要使用两个委托：一个用于向客户端报告进度事件，另一个用于向客户端报告完成事件。  

     [!code-csharp[System.ComponentModel.AsyncOperationManager#9](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#9)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#9](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#9)]  
    [!code-csharp[System.ComponentModel.AsyncOperationManager#20](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#20)]
    [!code-vb[System.ComponentModel.AsyncOperationManager#20](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#20)]  
  
2. 在组件的构造函数中调用 `InitializeDelegates` 方法。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#21](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#21)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#21](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#21)]  
  
3. 在 `PrimeNumberCalculator` 类中声明委托，以处理实际要异步完成的工作。 此委托包装用于测试数字是否为质数的工作方法。 此委托需要使用 <xref:System.ComponentModel.AsyncOperation> 参数，用于跟踪异步操作的生存期。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#22](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#22)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#22](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#22)]  
  
4. 创建一个集合，用于管理挂起的异步操作的生存期。 客户端需要通过一种途径，跟踪已执行和完成的操作。若要执行这样的跟踪，客户端必须在调用异步方法时，传递唯一令牌或任务 ID。 `PrimeNumberCalculator` 组件必须跟踪所有调用，具体方法是将任务 ID 与其对应的调用相关联。 如果客户端传递的任务 ID 不唯一，`PrimeNumberCalculator` 组件必须抛出异常。  
  
     `PrimeNumberCalculator` 组件使用特殊的集合类 <xref:System.Collections.Specialized.HybridDictionary> 跟踪任务 ID。 在类定义中，创建名为 `userTokenToLifetime` 的 <xref:System.Collections.Specialized.HybridDictionary>。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#23](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#23)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#23](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#23)]  
  
## <a name="implementing-public-events"></a>实现公共事件  

 实现基于事件的异步模式的组件使用事件与客户端进行通信。 这些事件在 <xref:System.ComponentModel.AsyncOperation> 类的相助下对适当的线程调用。  
  
### <a name="to-raise-events-to-your-components-clients"></a>若要向组件的客户端抛出事件，请执行以下操作：  
  
1. 实现公共事件，以向客户端报告事件。 需要实现两个事件，一个用于报告进度事件，另一个用于报告完成事件。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#24](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#24)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#24](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#24)]  
  
## <a name="implementing-the-completion-method"></a>实现完成方法  

 完成委托是在异步操作最终成功完成、出错或取消时，由基础的自由线程异步行为调用的方法。 此调用发生在任意线程上。  
  
 在此方法中，客户端任务 ID 从唯一客户端令牌的内部集合中删除。 另外，此方法还对相应的 <xref:System.ComponentModel.AsyncOperation> 调用 <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A>方法，结束特定异步操作的生存期。 此调用对适用于应用模型的线程抛出完成事件。 调用 <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> 后，便无法再使用此 <xref:System.ComponentModel.AsyncOperation> 实例，随后只要尝试使用它，就会导致异常抛出。  
  
 `CompletionMethod` 签名必须保留描述异步操作结果所需的全部状态。 它保留以下状态：此异步操作测试的数字是什么、数字是否为质数，以及第一个除数的值是什么（如果不是质数的话）。 此外，它还保留描述所发生的任何异常的状态，以及与此任务对应的 <xref:System.ComponentModel.AsyncOperation>。  
  
### <a name="to-complete-an-asynchronous-operation"></a>若要完成异步操作，请执行以下操作：  
  
- 实现完成方法。 此方法需要使用六个参数，用于填充通过客户端的 `CalculatePrimeCompletedEventHandler` 返回到客户端的 `CalculatePrimeCompletedEventArgs`。 它还从内部集合中删除客户端的任务 ID 令牌，并通过调用 <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> 结束异步操作的生存期。 <xref:System.ComponentModel.AsyncOperation> 封送对适用于应用模型的线程或上下文执行的调用。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#26](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#26)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#26](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#26)]  
  
## <a name="checkpoint"></a>检查点  

 此时，可以生成组件。  
  
### <a name="to-test-your-component"></a>测试组件的具体步骤  
  
- 编译组件。  
  
     将看到下面的一个编译器警告：  
  
    ```console  
    warning CS0169: The private field 'AsynchronousPatternExample.PrimeNumberCalculator.workerDelegate' is never used  
    ```  
  
     此警告会在下一部分中得到解析。  
  
## <a name="implementing-the-worker-methods"></a>实现工作方法  

 至此，已实现 `PrimeNumberCalculator` 组件的支持异步代码。 现在，可以实现执行实际工作的代码。 将实现以下三个方法：`CalculateWorker`、`BuildPrimeNumberList` 和 `IsPrime`。 `BuildPrimeNumberList` 和 `IsPrime` 共同构成了著名的埃拉托斯特尼筛法，通过在测试数字的平方根范围内查找所有质数，确定数字是否为质数。 如果使用这种方法没有找到任何除数，表明测试数字为质数。  
  
 如果此组件旨在最大限度地提高效率，便会记住对不同测试数字执行各种调用时发现的所有质数。 它还会检查是否有最简单的除数（如 2、3 和 5）。 虽然此示例旨在展示如何异步执行非常耗时的操作，但这些优化是留给大家练练手的。  
  
 `CalculateWorker` 方法包装在委托中，通过调用 `BeginInvoke` 进行异步调用。  
  
> [!NOTE]
> 进度事件报告是在 `BuildPrimeNumberList` 方法中实现。 在快速运行的计算机上，`ProgressChanged` 事件可能会快速连续抛出。 对其抛出这些事件的客户端线程必须能够处理这种情况。 消息可能会像洪水般涌入用户界面代码，导致代码无法不断更新，继而导致无响应。 有关处理这种情况的示例用户界面，请参阅[如何：实现基于事件的异步模式的客户端](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)。  
  
### <a name="to-execute-the-prime-number-calculation-asynchronously"></a>若要异步执行质数计算，请执行以下操作：  
  
1. 实现 `TaskCanceled` 实用工具方法。 此方法检查任务生存期集合中是否有给定的任务 ID；如果找不到任务 ID，就会返回 `true`。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#32](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#32)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#32](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#32)]  
  
2. 实现 `CalculateWorker` 方法。 它需要使用下面两个参数：要测试的数字和 <xref:System.ComponentModel.AsyncOperation>。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#27](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#27)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#27](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#27)]  
  
3. 实现 `BuildPrimeNumberList`。 它需要使用下面两个参数：要测试的数字和 <xref:System.ComponentModel.AsyncOperation>。 此方法使用 <xref:System.ComponentModel.AsyncOperation> 报告进度和增量结果。 这可确保对适用于应用模型的线程或上下文调用客户端的事件处理程序。 如果 `BuildPrimeNumberList` 找到了质数，它会将此作为增量结果，报告给 `ProgressChanged` 事件的客户端事件处理程序。 为此，必须使用派生自 <xref:System.ComponentModel.ProgressChangedEventArgs> 的类 `CalculatePrimeProgressChangedEventArgs`，其中新增有一个属性，即 `LatestPrimeNumber`。  
  
     `BuildPrimeNumberList` 方法还会定期调用 `TaskCanceled` 方法，并在此方法返回 `true` 时退出。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#5](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#5)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#5](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#5)]  
  
4. 实现 `IsPrime`。 它需要使用下面三个参数：已知质数列表、要测试的数字，以及找到的第一个除数的输出参数。 它根据质数列表确定测试数字是否为质数。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#28](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#28)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#28](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#28)]  
  
5. 从 <xref:System.ComponentModel.ProgressChangedEventArgs> 派生 `CalculatePrimeProgressChangedEventArgs`。 必须有此类，才能向 `ProgressChanged` 事件的客户端事件处理程序报告增量结果。 它新增有一个属性，即 `LatestPrimeNumber`。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#29](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#29)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#29](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#29)]  
  
## <a name="checkpoint"></a>检查点  

 此时，可以生成组件。  
  
### <a name="to-test-your-component"></a>测试组件的具体步骤  
  
- 编译组件。  
  
     剩下要编写的就是，异步操作的启动和取消方法，即 `CalculatePrimeAsync` 和 `CancelAsync`。  
  
## <a name="implementing-the-start-and-cancel-methods"></a>实现启动和取消方法  

 若要对它自己的线程启动工作方法，请对包装方法的委托调用 `BeginInvoke`。 若要管理特定异步操作的生存期，请对 <xref:System.ComponentModel.AsyncOperationManager> 帮助程序类调用 <xref:System.ComponentModel.AsyncOperationManager.CreateOperation%2A> 方法。 这会返回 <xref:System.ComponentModel.AsyncOperation>，将对客户端事件处理程序的调用封送到适当的线程或上下文。  
  
 若要取消特定挂起操作，请对相应的 <xref:System.ComponentModel.AsyncOperation> 调用 <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A>。 这样一来，可以结束操作，随后只要调用 <xref:System.ComponentModel.AsyncOperation> 都会导致异常抛出。  
  
### <a name="to-implement-start-and-cancel-functionality"></a>若要实现启动和取消功能，请执行以下操作：  
  
1. 实现 `CalculatePrimeAsync` 方法。 请确保相对于表示当前挂起任务的所有令牌，客户端提供的令牌（任务 ID）都是唯一的。 如果客户端传入的令牌不唯一，`CalculatePrimeAsync` 会抛出异常。 如果唯一，令牌会被添加到任务 ID 集合中。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#3](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#3)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#3](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#3)]  
  
2. 实现 `CancelAsync` 方法。 如果令牌集合中有 `taskId` 参数，将会删除此参数。 这可以防止尚未启动的已取消任务运行。 如果任务正在运行，`BuildPrimeNumberList` 方法会在检测到任务 ID 已从生存期集合中删除时退出。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#4](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#4)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#4](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#4)]  
  
## <a name="checkpoint"></a>检查点  

 此时，可以生成组件。  
  
### <a name="to-test-your-component"></a>测试组件的具体步骤  
  
- 编译组件。  
  
 `PrimeNumberCalculator` 组件现已完成且可供使用。  
  
 有关使用 `PrimeNumberCalculator` 组件的示例客户端，请参阅[如何：实现基于事件的异步模式的客户端](how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)。  
  
## <a name="next-steps"></a>后续步骤  

 可以编写与 `CalculatePrimeAsync` 方法相当的同步方法 `CalculatePrime`，扩充此示例。 这样一来，`PrimeNumberCalculator` 组件就完全符合基于事件的异步模式了。  
  
 若要改进此示例，可以保留对不同测试数字执行各种调用时发现的所有质数列表。 使用这种方法，所有任务都将受益于前面完成的任务。 请使用 `lock` 区域小心保护此列表，以序列化不同线程对列表的访问。  
  
 还可以测试是否有最简单的除数（如 2、3 和 5）来改进此示例。  
  
## <a name="see-also"></a>另请参阅

- [如何：在后台运行操作](/dotnet/desktop/winforms/controls/how-to-run-an-operation-in-the-background)
- [基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)
- [基于事件的异步模式 (EAP)](event-based-asynchronous-pattern-eap.md)
