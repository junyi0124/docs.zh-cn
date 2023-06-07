---
title: 调试工作流
ms.date: 03/30/2017
ms.assetid: b23b4814-ebb1-4c51-b7a9-469f4da7a96d
ms.openlocfilehash: 31c688f5f45b41f337176108486ec2074e1915a7
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90543831"
---
# <a name="debugging-workflows"></a>调试工作流

[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)]提供多个用于从开发环境调试运行的工作流的选项。 可以在设计器、XAML 和代码中调试工作流。

## <a name="debugging-in-the-workflow-designer"></a>在工作流设计器中调试

可以在工作流设计器中对活动设置断点，方法是突出显示活动，按 <kbd>F9</kbd> ，或使用活动的上下文菜单。 然后，当工作流宿主在调试模式下运行时将中断工作流的执行。 在下面的屏幕快照中，工作流执行在断点处暂停。 有关详细信息，请参阅 [用工作流设计器调试工作流](/visualstudio/workflow-designer/debugging-workflows-with-the-workflow-designer)。

## <a name="debugging-in-xaml"></a>在 XAML 中调试

如果在设计器中某个工作流已在断点处暂停，则还可以在 XAML 中调试该工作流。 若要在 XAML 中查看执行点，请在工作流执行暂停时在工作流设计器中选择 " **XAML 视图** "。 通过从解决方案资源管理器重新打开工作流可以将调试切换回设计器。 有关详细信息，请参阅 [如何：用工作流设计器调试 XAML](/visualstudio/workflow-designer/how-to-debug-xaml-with-the-workflow-designer)。

## <a name="debugging-in-code"></a>在代码中调试

若要设置断点，请单击 "代码" 窗格的左边距，或在要设置该断点的行上按 " <kbd>F9</kbd> "。

## <a name="attaching-to-a-workflow-process"></a>附加到工作流过程

工作流调试还支持使用 Visual Studio 的基础结构附加到某个过程。 这使工作流创作者能够调试在另一个主机环境（如 Internet 信息服务 (IIS) 7.0）中运行的工作流。

## <a name="remote-debugging"></a>远程调试

Windows Workflow Foundation (WF) 远程调试与其他 Visual Studio 组件进行远程调试的功能相同。 有关使用远程调试的信息，请参阅 [如何：启用远程调试](/previous-versions/visualstudio/visual-studio-2010/febz73k0(v=vs.100))。

> [!NOTE]
> 如果工作流应用程序针对 x86 体系结构并托管在运行64位操作系统的计算机上，则远程调试将无法工作，除非在远程计算机上安装了 Visual Studio，或者工作流应用程序的目标更改为 " **任意 CPU**"。

## <a name="extending-the-workflow-debugging-service"></a>扩展工作流调试服务

工作流调试器服务现在以公共方式使用，可以使用该服务创建自定义应用程序，如在重新承载的设计器中进行监视、模拟和调试。 有关详细信息，请参阅<xref:System.Activities.Presentation.Debug.DebuggerService>一文。
