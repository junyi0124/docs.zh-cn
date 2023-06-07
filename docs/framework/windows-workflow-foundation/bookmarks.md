---
title: 书签-WF
ms.date: 03/30/2017
ms.assetid: 9b51a346-09ae-455c-a70a-e2264ddeb9e2
ms.openlocfilehash: 7a52823ff68d8f09895bb3a9323a57d3abccd823
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289108"
---
# <a name="bookmarks"></a>书签

书签是使活动能够被动等待输入而无需保持在一个工作流线程上的机制。 如果某个活动发出正在等待刺激的信号，则该活动可以创建书签。 这指示即使返回了当前正在执行的方法（该方法创建了 <xref:System.Activities.Bookmark>），也不应将该活动的执行视为完成。  
  
## <a name="bookmark-basics"></a>书签基础知识  

 <xref:System.Activities.Bookmark> 表示工作流实例中可以在其处继续执行（以及可以通过其传递输入）的点。 通常，为 <xref:System.Activities.Bookmark> 指定一个名称并且外部（主机或扩展）代码负责使用相关数据继续书签。 继续 <xref:System.Activities.Bookmark> 后，工作流运行时会安排与其创建时的 <xref:System.Activities.BookmarkCallback> 关联的 <xref:System.Activities.Bookmark> 委托。  
  
## <a name="bookmark-options"></a>书签选项  

 <xref:System.Activities.BookmarkOptions> 类指定要创建的 <xref:System.Activities.Bookmark> 的类型。 不互相排斥的可能值为 <xref:System.Activities.BookmarkOptions.None>、<xref:System.Activities.BookmarkOptions.MultipleResume> 和 <xref:System.Activities.BookmarkOptions.NonBlocking>。 创建预期只继续一次的 <xref:System.Activities.BookmarkOptions.None> 时，使用默认的 <xref:System.Activities.Bookmark>。 创建可继续多次的 <xref:System.Activities.BookmarkOptions.MultipleResume> 时，使用 <xref:System.Activities.Bookmark>。 创建可能永不继续的 <xref:System.Activities.BookmarkOptions.NonBlocking> 时，使用 <xref:System.Activities.Bookmark>。 与使用默认的 <xref:System.Activities.BookmarkOptions> 创建的书签不同，<xref:System.Activities.BookmarkOptions.NonBlocking> 书签不会阻止某个活动完成。  
  
## <a name="bookmark-resumption"></a>书签继续  

 位于工作流之外的代码可以使用其中一个 <xref:System.Activities.WorkflowApplication.ResumeBookmark%2A> 重载继续书签。 在本示例中，将创建一个 `ReadLine` 活动。 执行时，`ReadLine` 活动创建 <xref:System.Activities.Bookmark>，注册回调，然后等待 <xref:System.Activities.Bookmark> 继续。 继续后，`ReadLine` 活动将随 <xref:System.Activities.Bookmark> 传递的数据赋给其 <xref:System.Activities.Activity%601.Result%2A> 实参。  
  
```csharp  
public sealed class ReadLine : NativeActivity<string>  
{  
    [RequiredArgument]  
    public  InArgument<string> BookmarkName { get; set; }  
  
    protected override void Execute(NativeActivityContext context)  
    {  
        // Create a Bookmark and wait for it to be resumed.  
        context.CreateBookmark(BookmarkName.Get(context),
            new BookmarkCallback(OnResumeBookmark));  
    }  
  
    // NativeActivity derived activities that do asynchronous operations by calling
    // one of the CreateBookmark overloads defined on System.Activities.NativeActivityContext
    // must override the CanInduceIdle property and return true.  
    protected override bool CanInduceIdle  
    {  
        get { return true; }  
    }  
  
    public void OnResumeBookmark(NativeActivityContext context, Bookmark bookmark, object obj)  
    {  
        // When the Bookmark is resumed, assign its value to  
        // the Result argument.  
        Result.Set(context, (string)obj);  
    }  
}  
```  
  
 在本示例中，将要创建一个工作流，该工作流使用 `ReadLine` 活动来收集用户名称并将其显示在控制台窗口中。 宿主应用程序将执行收集输入的实际工作并通过继续 <xref:System.Activities.Bookmark> 将输入传递给该工作流。  
  
```csharp  
Variable<string> name = new Variable<string>  
{  
    Name = "name"  
};  
  
Activity wf = new Sequence  
{  
    Variables =  
    {  
        name  
    },  
    Activities =  
    {  
        new WriteLine()  
        {  
            Text = "What is your name?"  
        },  
        new ReadLine()  
        {  
            BookmarkName = "UserName",  
            Result = name  
        },  
        new WriteLine()  
        {  
            Text = new InArgument<string>((env) => "Hello, " + name.Get(env))  
        }  
    }  
};  
  
AutoResetEvent syncEvent = new AutoResetEvent(false);  
  
// Create the WorkflowApplication using the desired  
// workflow definition.  
WorkflowApplication wfApp = new WorkflowApplication(wf);  
  
// Handle the desired lifecycle events.  
wfApp.Completed = delegate(WorkflowApplicationCompletedEventArgs e)  
{  
    // Signal the host that the workflow is complete.  
    syncEvent.Set();  
};  
  
// Start the workflow.  
wfApp.Run();  
  
// Collect the user's name and resume the bookmark.  
// Bookmark resumption only occurs when the workflow  
// is idle. If a call to ResumeBookmark is made and the workflow  
// is not idle, ResumeBookmark blocks until the workflow becomes  
// idle before resuming the bookmark.  
wfApp.ResumeBookmark("UserName", Console.ReadLine());  
  
// Wait for Completed to arrive and signal that  
// the workflow is complete.  
syncEvent.WaitOne();  
```  
  
 执行 `ReadLine` 活动时，该活动创建一个名为 <xref:System.Activities.Bookmark> 的 `UserName`，然后等待书签继续。 宿主收集所需的数据，然后继续 <xref:System.Activities.Bookmark>。 工作流继续，显示该名称，然后完成。 请注意，不需要与继续书签有关的任何同步代码。 <xref:System.Activities.Bookmark> 只能在工作流空闲时继续，如果工作流不空闲，则会调用 <xref:System.Activities.WorkflowApplication.ResumeBookmark%2A> 块，直到工作流空闲。  
  
## <a name="bookmark-resumption-result"></a>书签继续结果  

 <xref:System.Activities.WorkflowApplication.ResumeBookmark%2A> 返回一个 <xref:System.Activities.BookmarkResumptionResult> 枚举值，以指示书签继续请求的结果。 可能的返回值为 <xref:System.Activities.BookmarkResumptionResult.Success>、<xref:System.Activities.BookmarkResumptionResult.NotReady> 和 <xref:System.Activities.BookmarkResumptionResult.NotFound>。 宿主和扩展可以使用此值来确定如何继续执行。
