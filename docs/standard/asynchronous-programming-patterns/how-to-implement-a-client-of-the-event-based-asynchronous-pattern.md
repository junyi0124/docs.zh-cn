---
title: 如何：实现基于事件的异步模式的客户端
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
ms.assetid: 21a858c1-3c99-4904-86ee-0d17b49804fa
ms.openlocfilehash: 44bcc831ddf6fa292fd96d8e79ad54f7be2f65c6
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678085"
---
# <a name="how-to-implement-a-client-of-the-event-based-asynchronous-pattern"></a>如何：实现基于事件的异步模式的客户端

下面的代码示例展示了如何使用符合[基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)要求的组件。 此示例的窗体使用[如何：实现支持基于事件的异步模式的组件](component-that-supports-the-event-based-asynchronous-pattern.md)中介绍的 `PrimeNumberCalculator` 组件。  
  
 运行使用此示例的项目时，将会看到包含一个网格和两个按钮（“启动新任务”  和“取消”  ）的“质数计算器”窗体。 可以连续多次单击“启动新任务”  按钮。每次单击后，异步操作都会开始计算，以确定随机生成的测试数字是否为质数。 窗体会定期显示进度和增量结果。 每个操作都分配有唯一的任务 ID。 计算结果显示在“结果”  列中；如果测试数字不是质数，将会把它标记为“复合”  ，并显示第一个除数。  
  
 可使用“取消”  按钮，取消任意挂起的操作。 可以多选。  
  
> [!NOTE]
> 数字大多不是质数。 如果在多个操作已完成后找不到质数，只需启动更多任务，最终会找到一些质数。  
  
## <a name="example"></a>示例  

 [!code-csharp[System.ComponentModel.AsyncOperationManager#10](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/primenumbercalculatormain.cs#10)]
 [!code-vb[System.ComponentModel.AsyncOperationManager#10](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/primenumbercalculatormain.vb#10)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ComponentModel.AsyncOperation>
- <xref:System.ComponentModel.AsyncOperationManager>
- <xref:System.Windows.Forms.WindowsFormsSynchronizationContext>
