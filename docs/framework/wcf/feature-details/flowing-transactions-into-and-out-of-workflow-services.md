---
title: 使事务流入和流出工作流服务
ms.date: 03/30/2017
ms.assetid: 03ced70e-b540-4dd9-86c8-87f7bd61f609
ms.openlocfilehash: 8764f3c88fc978bc71ff993252b04fe58da4bbc9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96290343"
---
# <a name="flowing-transactions-into-and-out-of-workflow-services"></a>使事务流入和流出工作流服务

工作流服务和客户端可以参与事务。  对于将成为环境事务一部分的服务操作，将 <xref:System.ServiceModel.Activities.Receive> 活动放到 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动内。 由 <xref:System.ServiceModel.Activities.Send> 内的 <xref:System.ServiceModel.Activities.SendReply> 或 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动所做的任何调用也将在环境事务中进行。 工作流客户端应用程序可以通过使用 <xref:System.Activities.Statements.TransactionScope> 活动来创建环境事务，并通过使用该环境事务来调用服务操作。 本主题将指导您创建参与事务的工作流服务和工作流客户端。  
  
> [!WARNING]
> 如果在事务内加载工作流服务实例，且工作流包含 <xref:System.Activities.Statements.Persist> 活动，则工作流实例将被阻止，直到事务超时。  
  
> [!IMPORTANT]
> 无论何时使用 <xref:System.ServiceModel.Activities.TransactedReceiveScope>，都建议将所有接收都置于工作流内的 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动中。  
  
> [!IMPORTANT]
> 如果 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 且消息按错误顺序到达，则在尝试提交第一个不按顺序的消息时，工作流会中止。 必须确保工作流在空闲时始终处于一致的停止点。 这会使您可以在工作流中止时，从以前的持久点重新启动工作流。  
  
### <a name="create-a-shared-library"></a>创建共享库  
  
1. 创建新的空 Visual Studio 解决方案。  
  
2. 添加一个名为 `Common` 的新类库项目。 添加对下列程序集的引用：  
  
    - System.Activities.dll  
  
    - System.ServiceModel.dll  
  
    - System.ServiceModel.Activities.dll  
  
    - System.Transactions.dll  
  
3. 将一个名为 `PrintTransactionInfo` 的新类添加到 `Common` 项目。 此类派生自 <xref:System.Activities.NativeActivity>，并重载 <xref:System.Activities.NativeActivity.Execute%2A> 方法。  
  
    ```csharp
    using System;  
    using System;  
    using System.Activities;  
    using System.Transactions;  
  
    namespace Common  
    {  
        public class PrintTransactionInfo : NativeActivity  
        {  
            protected override void Execute(NativeActivityContext context)  
            {  
                RuntimeTransactionHandle rth = context.Properties.Find(typeof(RuntimeTransactionHandle).FullName) as RuntimeTransactionHandle;  
  
                if (rth == null)  
                {  
                    Console.WriteLine("There is no ambient RuntimeTransactionHandle");  
                }  
  
                Transaction t = rth.GetCurrentTransaction(context);  
  
                if (t == null)  
                {  
                    Console.WriteLine("There is no ambient transaction");  
                }  
                else  
                {  
                    Console.WriteLine("Transaction: {0} is {1}", t.TransactionInformation.DistributedIdentifier, t.TransactionInformation.Status);  
                }  
            }  
        }  
  
    }  
    ```  
  
     这是一个本机活动，用于显示有关环境事务的信息，它将在本主题中使用的服务工作流和客户端工作流中使用。 生成解决方案以使此活动在 **工具箱** 的 "**常用**" 部分中可用。  
  
### <a name="implement-the-workflow-service"></a>实现工作流服务  
  
1. 向项目添加一个名为的新 WCF 工作流服务 `WorkflowService` `Common` 。 为此，请右键单击 `Common` 项目，选择 "**添加**"、"**新建项 ...**"，选择 "**已安装模板**" 下的 "**工作流**"，然后选择 " **WCF 工作流服务**"  
  
     ![添加工作流服务](./media/flowing-transactions-into-and-out-of-workflow-services/add-workflow-service.jpg)  
  
2. 删除默认的 `ReceiveRequest` 和 `SendResponse` 活动。  
  
3. 将 <xref:System.Activities.Statements.WriteLine> 活动拖放到 `Sequential Service` 活动中。 将文本属性设置为 `"Workflow Service starting ..."`，如下面的示例所示。  
  
     ![向顺序服务活动中添加 WriteLine 活动 (。/media/flowing-transactions-into-and-out-of-workflow-services/add-writeline-sequential-service.jpg)   
  
4. 将 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 拖放到 <xref:System.Activities.Statements.WriteLine> 活动后面。 <xref:System.ServiceModel.Activities.TransactedReceiveScope>可以在 "**工具箱**" 的 "**消息传送**" 部分找到活动。 <xref:System.ServiceModel.Activities.TransactedReceiveScope>活动由两部分组成：**请求** 和 **正文**。 " **请求** " 部分包含 <xref:System.ServiceModel.Activities.Receive> 活动。 **Body** 节包含在收到消息后在事务中执行的活动。  
  
     ![添加 TransactedReceiveScope 活动](./media/flowing-transactions-into-and-out-of-workflow-services/transactedreceivescope-activity.jpg)  
  
5. 选择 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 活动，然后单击 " **变量** " 按钮。 添加以下变量。  
  
     ![向 TransactedReceiveScope 添加变量](./media/flowing-transactions-into-and-out-of-workflow-services/add-transactedreceivescope-variables.jpg)  
  
    > [!NOTE]
    > 可以删除默认情况下在该处的数据变量。 还可以使用现有句柄变量。  
  
6. 将活动拖放到 <xref:System.ServiceModel.Activities.Receive> 活动的 " **请求** " 部分中 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 。 设置以下属性：  
  
    |属性|值|  
    |--------------|-----------|  
    |CanCreateInstance|True（选中复选框）|  
    |OperationName|StartSample|  
    |ServiceContractName|ITransactionSample|  
  
     该工作流应如下所示：  
  
     ![添加 Receive 活动](./media/flowing-transactions-into-and-out-of-workflow-services/add-receive-activity.jpg)  
  
7. 单击活动中的 " **定义 ...** " 链接 <xref:System.ServiceModel.Activities.Receive> ，然后进行以下设置：  
  
     ![设置 Receive 活动的消息设置](./media/flowing-transactions-into-and-out-of-workflow-services/receive-message-settings.jpg)  
  
8. 将 <xref:System.Activities.Statements.Sequence> 活动拖放到 <xref:System.ServiceModel.Activities.TransactedReceiveScope> 的“正文”部分内。 在 <xref:System.Activities.Statements.Sequence> 活动内，拖放两个 <xref:System.Activities.Statements.WriteLine> 活动并设置 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性，如下表所示。  
  
    |活动|值|  
    |--------------|-----------|  
    |第一个 WriteLine|"服务：接收已完成"|  
    |第二个 WriteLine|"Service: Received = " + requestMessage|  
  
     现在，该工作流应如下所示：  
  
     ![添加 WriteLine 活动后的序列](./media/flowing-transactions-into-and-out-of-workflow-services/after-adding-writelines.jpg)  
  
9. 将活动拖放到 `PrintTransactionInfo` 活动的主体中的第二个 <xref:System.Activities.Statements.WriteLine> 活动之后 **Body** <xref:System.ServiceModel.Activities.TransactedReceiveScope> 。  
  
     ![添加 PrintTransactionInfo 后的序列](./media/flowing-transactions-into-and-out-of-workflow-services/after-adding-printtransactioninfo.jpg )  
  
10. 将 <xref:System.Activities.Statements.Assign> 活动拖放到 `PrintTransactionInfo` 活动后面，然后根据下表设置其属性。  
  
    |属性|值|  
    |--------------|-----------|  
    |如果|replyMessage|  
    |值|"Service: Sending reply."|  
  
11. 将 <xref:System.Activities.Statements.WriteLine> 活动拖放到 <xref:System.Activities.Statements.Assign> 活动后面，然后将它的 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 "Service: Begin reply."  
  
     现在，该工作流应如下所示：  
  
     ![添加 Assign 和 WriteLine 后](./media/flowing-transactions-into-and-out-of-workflow-services/after-adding-sbr-writeline.jpg)  
  
12. 右键单击该 <xref:System.ServiceModel.Activities.Receive> 活动，然后选择 " **创建 SendReply** " 并将其粘贴到最后一个 <xref:System.Activities.Statements.WriteLine> 活动后面。 单击活动中的 " **定义 ...** " 链接 `SendReplyToReceive` ，然后进行以下设置。  
  
     ![回复消息设置](./media/flowing-transactions-into-and-out-of-workflow-services/reply-message-settings.jpg)  
  
13. 将活动拖放 <xref:System.Activities.Statements.WriteLine> `SendReplyToReceive` 到活动后面，并将其 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 "Service： Reply sent"。  
  
14. 将 <xref:System.Activities.Statements.WriteLine> 活动拖放到工作流底部，然后将它的 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 "Service: Workflow ends, press ENTER to exit."  
  
     完成的服务工作流应如下所示：  
  
     ![完成服务工作流](./media/flowing-transactions-into-and-out-of-workflow-services/service-complete-workflow.jpg)  
  
### <a name="implement-the-workflow-client"></a>实现工作流客户端  
  
1. 将一个名为 `WorkflowClient` 的新 WCF 工作流应用程序添加到 `Common` 项目。 要执行此操作，请右键单击 `Common` 项目，选择 "**添加**" "**新建项 ...**"，选择 "**已安装模板**" 下的 "**工作流**"，然后选择 "**活动**  
  
     ![添加活动项目](./media/flowing-transactions-into-and-out-of-workflow-services/add-activity-project.jpg)  
  
2. 将 <xref:System.Activities.Statements.Sequence> 活动拖放到设计图面上。  
  
3. 在 <xref:System.Activities.Statements.Sequence> 活动内拖放一个 <xref:System.Activities.Statements.WriteLine> 活动，然后将它的 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 `"Client: Workflow starting"`。 现在，该工作流应如下所示：  
  
     ![添加 WriteLine 活动](./media/flowing-transactions-into-and-out-of-workflow-services/add-writeline-activity.jpg)  
  
4. 将 <xref:System.Activities.Statements.TransactionScope> 活动拖放到 <xref:System.Activities.Statements.WriteLine> 活动后面。  选择 <xref:System.Activities.Statements.TransactionScope> 活动，单击“变量”按钮，然后添加以下变量。  
  
     ![向 TransactionScope 添加变量](./media/flowing-transactions-into-and-out-of-workflow-services/transactionscope-variables.jpg)  
  
5. 将 <xref:System.Activities.Statements.Sequence> 活动拖放到 <xref:System.Activities.Statements.TransactionScope> 活动的正文内。  
  
6. 在 `PrintTransactionInfo` 内拖放 <xref:System.Activities.Statements.Sequence> 活动  
  
7. 将活动拖放 <xref:System.Activities.Statements.WriteLine> `PrintTransactionInfo` 到活动后面，并将其 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 "Client：开始发送"。 现在，该工作流应如下所示：  
  
     ![添加客户端：开始发送活动](./media/flowing-transactions-into-and-out-of-workflow-services/client-add-cbs-writeline.jpg)  
  
8. 将 <xref:System.ServiceModel.Activities.Send> 活动拖放到 <xref:System.Activities.Statements.Assign> 活动后面，并设置以下属性：  
  
    |属性|值|  
    |--------------|-----------|  
    |EndpointConfigurationName|workflowServiceEndpoint|  
    |OperationName|StartSample|  
    |ServiceContractName|ITransactionSample|  
  
     现在，该工作流应如下所示：  
  
     ![设置 Send 活动属性](./media/flowing-transactions-into-and-out-of-workflow-services/client-send-activity-settings.jpg)  
  
9. 单击 " **定义 ...** " 链接，然后进行以下设置：  
  
     ![发送活动的消息设置](./media/flowing-transactions-into-and-out-of-workflow-services/send-message-settings.jpg)  
  
10. 右键单击该 <xref:System.ServiceModel.Activities.Send> 活动，然后选择 " **创建 ReceiveReply**"。 <xref:System.ServiceModel.Activities.ReceiveReply> 活动将自动放在 <xref:System.ServiceModel.Activities.Send> 活动后面。  
  
11. 单击 ReceiveReplyForSend 活动上的“定义...”链接，然后进行以下设置：  
  
     ![设置 ReceiveForSend 消息的设置](./media/flowing-transactions-into-and-out-of-workflow-services/client-reply-message-settings.jpg)  
  
12. 将 <xref:System.Activities.Statements.WriteLine> 活动拖放到 <xref:System.ServiceModel.Activities.Send> 和 <xref:System.ServiceModel.Activities.ReceiveReply> 活动之间，然后将它的 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 "Client: Send complete."  
  
13. 将 <xref:System.Activities.Statements.WriteLine> 活动拖放到 <xref:System.ServiceModel.Activities.ReceiveReply> 活动后面，然后将它的 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 "Client side: Reply received = " + replyMessage  
  
14. 将 `PrintTransactionInfo` 活动拖放到 <xref:System.Activities.Statements.WriteLine> 活动后面。  
  
15. 将 <xref:System.Activities.Statements.WriteLine> 活动拖放到工作流末尾，然后将它的 <xref:System.Activities.Statements.WriteLine.Text%2A> 属性设置为 "Client workflow ends." 完成的客户端工作流应如下图所示。  
  
     ![已完成的客户端工作流](./media/flowing-transactions-into-and-out-of-workflow-services/client-complete-workflow.jpg)  
  
16. 生成解决方案。  
  
### <a name="create-the-service-application"></a>创建服务应用程序  
  
1. 将一个名为 `Service` 的新控制台应用程序项目添加到该解决方案。 添加对下列程序集的引用：  
  
    1. System.Activities.dll  
  
    2. System.ServiceModel.dll  
  
    3. System.ServiceModel.Activities.dll  
  
2. 打开生成的 Program.cs 文件并添加以下代码：  
  
    ```csharp
          static void Main()  
          {  
              Console.WriteLine("Building the server.");  
              using (WorkflowServiceHost host = new WorkflowServiceHost(new DeclarativeServiceWorkflow(), new Uri("net.tcp://localhost:8000/TransactedReceiveService/Declarative")))  
              {
                  //Start the server  
                  host.Open();  
                  Console.WriteLine("Service started.");  
  
                  Console.WriteLine();  
                  Console.ReadLine();  
                  //Shutdown  
                  host.Close();  
              };
          }  
    ```  
  
3. 将以下 app.config 文件添加到项目。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <!-- Copyright © Microsoft Corporation.  All rights reserved. -->  
    <configuration>  
        <system.serviceModel>  
            <bindings>  
                <netTcpBinding>  
                    <binding transactionFlow="true" />  
                </netTcpBinding>  
            </bindings>  
        </system.serviceModel>  
    </configuration>  
    ```  
  
### <a name="create-the-client-application"></a>创建客户端应用程序  
  
1. 将一个名为 `Client` 的新控制台应用程序项目添加到该解决方案。 添加对 System.Activities.dll 的引用。  
  
2. 打开 program.cs 文件并添加以下代码。  
  
    ```csharp
    class Program  
    {  

        private static AutoResetEvent syncEvent = new AutoResetEvent(false);  
  
        static void Main(string[] args)  
        {  
            //Build client  
            Console.WriteLine("Building the client.");  
            WorkflowApplication client = new WorkflowApplication(new DeclarativeClientWorkflow());  
            client.Completed = Program.Completed;  
            client.Aborted = Program.Aborted;  
            client.OnUnhandledException = Program.OnUnhandledException;  
            //Wait for service to start  
            Console.WriteLine("Press ENTER once service is started.");  
            Console.ReadLine();  
  
            //Start the client
            Console.WriteLine("Starting the client.");  
            client.Run();  
            syncEvent.WaitOne();  
  
            //Sample complete  
            Console.WriteLine();  
            Console.WriteLine("Client complete. Press ENTER to exit.");  
            Console.ReadLine();  
        }  
  
        private static void Completed(WorkflowApplicationCompletedEventArgs e)  
        {  
            Program.syncEvent.Set();  
        }  
  
        private static void Aborted(WorkflowApplicationAbortedEventArgs e)  
        {  
            Console.WriteLine("Client Aborted: {0}", e.Reason);  
            Program.syncEvent.Set();  
        }  
  
        private static UnhandledExceptionAction OnUnhandledException(WorkflowApplicationUnhandledExceptionEventArgs e)  
        {  
            Console.WriteLine("Client had an unhandled exception: {0}", e.UnhandledException);  
            return UnhandledExceptionAction.Cancel;  
        }  
    }  
    ```  
  
## <a name="see-also"></a>另请参阅

- [工作流服务](workflow-services.md)
- [Windows Communication Foundation 事务概述](transactions-overview.md)
