---
title: 如何：使用支持基于事件的异步模式的组件
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
ms.assetid: 35e9549c-1568-4768-ad07-17cc6dff11e1
ms.openlocfilehash: 36fe8015187833e03c4cc3fc1609ec647daf7278
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95677994"
---
# <a name="how-to-use-components-that-support-the-event-based-asynchronous-pattern"></a>如何：使用支持基于事件的异步模式的组件

许多组件都支持异步执行工作。 例如，通过 <xref:System.Media.SoundPlayer> 和 <xref:System.Windows.Forms.PictureBox> 组件，可以“在后台”加载音频和图像，同时主线程继续运行而不中断。  
  
 对支持 [基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)的类使用异步方法，这与将事件处理程序附加到组件的 _MethodName_**Completed** 事件一样简单，就像处理其他任何事件一样。 调用 _MethodName_**Async** 方法后，应用程序会继续运行而不中断，除非 _MethodName_**Completed** 事件抛出。 在事件处理程序中，可以检查 <xref:System.ComponentModel.AsyncCompletedEventArgs> 参数，以确定异步操作是已成功完成，还是已取消。  
  
 若要详细了解如何使用事件处理程序，请参阅[事件处理程序概述](/dotnet/desktop/winforms/event-handlers-overview-windows-forms)。  
  
 下面的过程展示了如何使用 <xref:System.Windows.Forms.PictureBox> 控件的异步图像加载功能。  
  
### <a name="to-enable-a-picturebox-control-to-asynchronously-load-an-image"></a>启用 PictureBox 控件以异步加载图像的具体步骤  
  
1. 在窗体中，创建 <xref:System.Windows.Forms.PictureBox> 组件的实例。  
  
2. 将事件处理程序分配给 <xref:System.Windows.Forms.PictureBox.LoadCompleted> 事件。  
  
     检查是否有异步下载期间可能会发生的任何错误。 此时，还检查是否有取消事件。  
  
     [!code-csharp[System.Windows.Forms.PictureBox.LoadAsync#2](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.PictureBox.LoadAsync#2](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/Form1.vb#2)]  
  
     [!code-csharp[System.Windows.Forms.PictureBox.LoadAsync#5](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/Form1.cs#5)]
     [!code-vb[System.Windows.Forms.PictureBox.LoadAsync#5](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/Form1.vb#5)]  
  
3. 将两个按钮 `loadButton` 和 `cancelLoadButton` 添加到窗体。 添加 <xref:System.Windows.Forms.Control.Click> 事件处理程序，以启动和取消下载。  
  
     [!code-csharp[System.Windows.Forms.PictureBox.LoadAsync#3](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/Form1.cs#3)]
     [!code-vb[System.Windows.Forms.PictureBox.LoadAsync#3](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/Form1.vb#3)]  
  
     [!code-csharp[System.Windows.Forms.PictureBox.LoadAsync#4](snippets/component-that-supports-the-event-based-asynchronous-pattern/csharp/Form1.cs#4)]
     [!code-vb[System.Windows.Forms.PictureBox.LoadAsync#4](snippets/component-that-supports-the-event-based-asynchronous-pattern/vb/Form1.vb#4)]  
  
4. 运行您的应用程序。  
  
     随着图像继续下载，可以随意移动窗体，也能最小化和最大化窗体。  
  
## <a name="see-also"></a>另请参阅

- [如何：在后台运行操作](/dotnet/desktop/winforms/controls/how-to-run-an-operation-in-the-background)
- [基于事件的异步模式概述](event-based-asynchronous-pattern-overview.md)
