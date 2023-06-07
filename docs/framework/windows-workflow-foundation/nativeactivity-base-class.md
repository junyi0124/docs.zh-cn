---
title: NativeActivity 基类
ms.date: 03/30/2017
ms.assetid: 254a4c50-425b-426d-a32f-0f7234925bac
ms.openlocfilehash: d875f62dacadb2baf6b5d7e93ddb2933aed9cdb0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96274954"
---
# <a name="nativeactivity-base-class"></a>NativeActivity 基类

<xref:System.Activities.NativeActivity> 是一个带有受保护的构造函数的抽象类。 与 <xref:System.Activities.CodeActivity> 一样，<xref:System.Activities.NativeActivity> 用于通过实现 <xref:System.Activities.NativeActivity.Execute%2A> 方法来编写命令性行为。 与 <xref:System.Activities.CodeActivity> 不同的是，通过传递给 <xref:System.Activities.NativeActivity> 方法的 <xref:System.Activities.NativeActivityContext> 对象，<xref:System.Activities.NativeActivity.Execute%2A> 有权访问工作流运行时的所有公开的功能。

## <a name="using-nativeactivitycontext"></a>使用 NativeActivityContext

 通过使用类型为 <xref:System.Activities.NativeActivity.Execute%2A> 的 `context` 参数的成员，可从 <xref:System.Activities.NativeActivityContext> 方法中访问工作流运行时的功能。 可通过 <xref:System.Activities.NativeActivityContext> 使用的功能如下：

- 获取并设置自变量和变量。

- 使用 <xref:System.Activities.NativeActivityContext.ScheduleActivity%2A> 安排子活动。

- 使用 <xref:System.Activities.NativeActivityContext.Abort%2A> 中止活动执行。

- 使用 <xref:System.Activities.NativeActivityContext.CancelChild%2A> 和 <xref:System.Activities.NativeActivityContext.CancelChildren%2A> 取消子级执行。

- 使用 <xref:System.Activities.NativeActivityContext.CreateBookmark%2A>、<xref:System.Activities.NativeActivityContext.RemoveBookmark%2A> 和 <xref:System.Activities.NativeActivityContext.ResumeBookmark%2A> 等方法访问活动书签。

- 使用 <xref:System.Activities.CodeActivityContext.Track%2A> 的自定义跟踪功能。

- 使用 <xref:System.Activities.CodeActivityContext.GetProperty%2A> 和 <xref:System.Activities.NativeActivityContext.GetValue%2A> 访问活动的执行属性和值属性。

- 使用 <xref:System.Activities.NativeActivityContext.ScheduleAction%2A> 和 <xref:System.Activities.NativeActivityContext.ScheduleFunc%2A> 安排活动操作和功能。

### <a name="to-create-a-custom-activity-that-inherits-from-nativeactivity"></a>创建从 NativeActivity 继承的自定义活动

1. OpenVisual Studio 2010。

2. 依次选择 " **文件**"、" **新建**" 和 " **项目**"。 在 "**项目类型**" 窗口中的 " **Visual c #** " 下选择 " **Workflow 4.0** "，然后选择 " **v2010** " 节点。 在 "**模板**" 窗口中选择 "**活动库**"。 将新项目命名为 HelloActivity。

3. 右键单击击 helloactivity 项目中的 Activity1，然后选择 " **删除**"。

4. 右键单击 "击 helloactivity" 项目，然后依次选择 " **添加**"、" **类**"。 将新类命名为 HelloActivity.cs。

5. 在 HelloActivity.cs 文件中，添加以下 `using` 指令。

    ```csharp
    using System.Activities;
    using System.Activities.Statements;
    ```

6. 通过向类声明中添加一个基类使新类从 <xref:System.Activities.NativeActivity> 继承。

    ```csharp
    class HelloActivity : NativeActivity
    ```

7. 通过添加 <xref:System.Activities.NativeActivity.Execute%2A> 方法来向类中添加功能。

    ```csharp
    protected override void Execute(NativeActivityContext context)
    {
        Console.WriteLine("Hello World!");
    }
    ```

8. 重写 <xref:System.Activities.NativeActivity.CacheMetadata%2A> 方法并且调用相应的 Add 方法，以便让工作流运行时了解自定义活动的变量、自变量、子级和委托。 有关更多信息，请参见 <xref:System.Activities.NativeActivityMetadata> 类。

9. 使用 <xref:System.Activities.NativeActivityContext> 对象来计划书签。 有关如何创建、计划和恢复书签的详细信息，请参见 <xref:System.Activities.WorkflowApplicationIdleEventArgs.Bookmarks%2A>。

    ```csharp
    protected override void Execute(NativeActivityContext context)
        {
            // Create a Bookmark and wait for it to be resumed.
            context.CreateBookmark(BookmarkName.Get(context),
                new BookmarkCallback(OnResumeBookmark));
        }
    ```
