---
title: 使用活动扩展
ms.date: 03/30/2017
ms.assetid: 500eb96a-c009-4247-b6b5-b36faffdf715
ms.openlocfilehash: 3a9cabda9fe92b2ea4e708da8f853f3029328775
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293281"
---
# <a name="using-activity-extensions"></a>使用活动扩展

活动可与工作流应用程序扩展进行交互，这些扩展允许主机提供未在工作流中显式建模的其他功能。  本主题描述如何创建和使用扩展以计算活动的执行次数。

### <a name="to-use-an-activity-extension-to-count-executions"></a>使用活动扩展来计算执行次数

1. 打开 Visual Studio 2010。 选择 " **新建**"、" **项目**"。 在 **Visual c #** 节点下，选择 " **工作流**"。  从模板列表中选择 " **工作流控制台应用程序** "。 将项目命名为 `Extensions`。 单击“确定”以创建该项目  。

2. `using`在 Program.cs 文件中为 **system.object** 命名空间添加语句。

    ```csharp
    using System.Collections.Generic;
    ```

3. 在 Program.cs 文件中，创建一个名为 **ExecutionCountExtension** 的新类。 下面的代码创建一个在调用其 **Register** 方法时跟踪实例 id 的工作流扩展。

    ```csharp
    // This extension collects a list of workflow Ids
    public class ExecutionCountExtension
    {
        IList<Guid> instances = new List<Guid>();

        public int ExecutionCount
        {
            get
            {
                return this.instances.Count;
            }
        }

        public IEnumerable<Guid> InstanceIds
        {
            get
            {
                return this.instances;
            }
        }

        public void Register(Guid activityInstanceId)
        {
            if (!this.instances.Contains<Guid>(activityInstanceId))
            {
                instances.Add(activityInstanceId);
            }
        }
    }
    ```

4. 创建使用 **ExecutionCountExtension** 的活动。 下面的代码定义一个活动，该活动从运行时检索 **ExecutionCountExtension** 对象，并在执行活动时调用其 **Register** 方法。

    ```csharp
    // Activity that consumes an extension provided by the host. If the extension is available
    // in the context, it will invoke (in this case, registers the Id of the executing workflow)
    public class MyActivity: CodeActivity
    {
        protected override void Execute(CodeActivityContext context)
        {
            ExecutionCountExtension ext = context.GetExtension<ExecutionCountExtension>();
            if (ext != null)
            {
                ext.Register(context.WorkflowInstanceId);
            }

        }
    }
    ```

5. 在 program.cs 文件的 **Main** 方法中实现活动。 以下代码包含用于生成两个不同的工作流、将每个工作流执行多次以及显示扩展中包含的结果数据的方法。

    ```csharp
    class Program
    {
        // Creates a workflow that uses the activity that consumes the extension
        static Activity CreateWorkflow1()
        {
            return new Sequence
            {
                Activities =
                {
                    new MyActivity()
                }
            };
        }

        // Creates a workflow that uses two instances of the activity that consumes the extension
        static Activity CreateWorkflow2()
        {
            return new Sequence
            {
                Activities =
                {
                    new MyActivity(),
                    new MyActivity()
                }
            };
        }

        static void Main(string[] args)
        {
            // create the extension
            ExecutionCountExtension executionCountExt = new ExecutionCountExtension();

            // configure the first invoker and execute 3 times
            WorkflowInvoker invoker = new WorkflowInvoker(CreateWorkflow1());
            invoker.Extensions.Add(executionCountExt);
            invoker.Invoke();
            invoker.Invoke();
            invoker.Invoke();

            // configure the second invoker and execute 2 times
            WorkflowInvoker invoker2 = new WorkflowInvoker(CreateWorkflow2());
            invoker2.Extensions.Add(executionCountExt);
            invoker2.Invoke();
            invoker2.Invoke();

            // show the data in the extension
            Console.WriteLine("Executed {0} times", executionCountExt.ExecutionCount);
            executionCountExt.InstanceIds.ToList().ForEach(i => Console.WriteLine("...{0}", i));
        }
    }
    ```
