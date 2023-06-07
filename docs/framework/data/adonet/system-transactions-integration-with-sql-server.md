---
title: System.Transactions 与 SQL Server 的集成
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b555544e-7abb-4814-859b-ab9cdd7d8716
ms.openlocfilehash: 5adf40f96854e08736cdef77300d69e452de5eea
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191679"
---
# <a name="systemtransactions-integration-with-sql-server"></a>System.Transactions 与 SQL Server 的集成

.NET Framework 版本2.0 引入了一个可通过命名空间访问的事务框架 <xref:System.Transactions> 。 此框架以完全集成在 .NET Framework 中的方式公开事务，包括 ADO.NET。  
  
 除可编程性增强功能外， <xref:System.Transactions> ADO.NET 还可协同工作，在处理事务时协调优化。 可提升事务是可以根据需要自动提升为完全分布式事务的轻型（本地）事务。  
  
 从 ADO.NET 2.0 开始， <xref:System.Data.SqlClient> 当你使用 SQL Server 时，支持可提升事务。 可提升的事务不会调用分布式事务增加的系统开销，除非需要增加的系统开销。 可提升事务是自动的，无需开发人员介入。  
  
 仅当使用 .NET Framework 数据提供程序进行 SQL Server () 使用 SQL Server 时，才可以使用可提升事务 `SqlClient` 。  
  
## <a name="creating-promotable-transactions"></a>创建可提升事务  

 SQL Server 的 .NET Framework 提供程序支持可提升事务，这些事务是通过 .NET Framework 命名空间中的类处理的 <xref:System.Transactions> 。 可提升事务通过将分布式事务推迟到需要时再创建，对分布式事务进行优化。 如果只需要一个资源管理器，则不会发生任何分布式事务。  
  
> [!NOTE]
> 在部分信任方案中，将事务提升为分布式事务时，需要 <xref:System.Transactions.DistributedTransactionPermission> 。  
  
## <a name="promotable-transaction-scenarios"></a>可提升事务方案  

 分布式事务由 Microsoft 分布式事务处理协调器 (MS DTC) 管理，该协调程序集成了事务中访问的所有资源管理器，通常会占用大量的系统资源。 可提升事务是事务的一种特殊形式 <xref:System.Transactions> ，有效地将工作委托给简单的 SQL Server 事务。 <xref:System.Transactions>、 <xref:System.Data.SqlClient> 和 SQL Server 协调处理事务时涉及到的工作，根据需要将其提升为完整的分布式事务。  
  
 使用可提升事务的优点是在使用活动 <xref:System.Transactions.TransactionScope> 事务打开某个连接但不打开任何其他连接时，事务作为轻型事务提交，而不引发完全分布式事务的其他系统开销。  
  
### <a name="connection-string-keywords"></a>连接字符串关键字  

 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> 属性支持关键字 `Enlist`，该关键字指示 <xref:System.Data.SqlClient> 是否将检测事务上下文并自动在分布式事务中登记连接。 如果 `Enlist=true`，连接将自动在打开的线程的当前事务上下文中登记。 如果 `Enlist=false`， `SqlClient` 连接不会与分布式事务进行交互。 `Enlist` 的默认值为 true。 如果连接字符串中未指定 `Enlist` ，而在连接打开时检测到一个连接，连接将自动在分布式事务中登记。  
  
 `Transaction Binding` 连接字符串中的 <xref:System.Data.SqlClient.SqlConnection> 关键字控制连接与已登记的 `System.Transactions` 事务的关联。 还可以通过 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.TransactionBinding%2A> 的 <xref:System.Data.SqlClient.SqlConnectionStringBuilder>属性使用。  
  
 下表说明可用的值。  
  
|关键字|说明|  
|-------------|-----------------|  
|Implicit Unbind|默认值。 事务结束时，连接与事务分离，切换回自动提交模式。|  
|Explicit Unbind|事务关闭之前，连接保持附加到事务。 如果关联的事务未处于活动状态或不匹配 <xref:System.Transactions.Transaction.Current%2A>，则连接将失败。|  
  
## <a name="using-transactionscope"></a>使用 TransactionScope  

 <xref:System.Transactions.TransactionScope> 类通过在分布式事务中隐式登记连接，使代码块成为事务代码。 必须在 <xref:System.Transactions.TransactionScope.Complete%2A> 块的结尾调用 <xref:System.Transactions.TransactionScope> 方法，然后再离开该代码块。 离开代码块将调用 <xref:System.Transactions.TransactionScope.Dispose%2A> 方法。 如果引发的异常造成代码离开范围，将认为事务已中止。  
  
 我们建议您使用 `using` 块，以确保在退出 using 代码块时，在 <xref:System.Transactions.TransactionScope.Dispose%2A> 对象上调用 <xref:System.Transactions.TransactionScope> 。 如果无法提交或回滚挂起的事务，可能会对性能造成严重影响，因为 <xref:System.Transactions.TransactionScope> 的默认超时为一分钟。 如果未使用 `using` 语句，必须在 `Try` 块中执行所有工作，并在 <xref:System.Transactions.TransactionScope.Dispose%2A> 块中显式调用 `Finally` 方法。  
  
 如果在 <xref:System.Transactions.TransactionScope>中发生异常，事务将标记为不一致并被弃用。 在 <xref:System.Transactions.TransactionScope> 断开后，事务将回滚。 如果未发生异常，则提交参与的事务。  
  
> [!NOTE]
> 默认情况下，`TransactionScope` 类创建 <xref:System.Transactions.Transaction.IsolationLevel%2A> 为 `Serializable` 的事务。 根据您的应用程序，您可能希望考虑降低隔离级别，以避免在应用程序中发生争用激烈的情况。  
  
> [!NOTE]
> 我们建议您只在分布式事务中执行更新、插入和删除，因为这些操作会占用大量的数据库资源。 选择语句可能会对数据库资源进行不必要的锁定，在某些方案中，可能需要使用事务进行选择。 任何非数据库工作应在事务范围之外完成，除非工作涉及其他事务化的资源管理器。 尽管事务范围内的异常会使事务无法提交，但是， <xref:System.Transactions.TransactionScope> 类没有规定回滚您的代码在事务本身范围之外所作的任何更改。 如果在事务回滚时需要采取某项措施，必须自己编写 <xref:System.Transactions.IEnlistmentNotification> 接口的实现并显式在事务中登记。  
  
## <a name="example"></a>示例  

 使用 <xref:System.Transactions> 要求具有 System.Transactions.dll 的引用。  
  
 下面的函数演示如何针对包装在 <xref:System.Data.SqlClient.SqlConnection> 块中的两个不同 SQL Server 实例（由两个不同的 <xref:System.Transactions.TransactionScope> 对象表示）创建可提升事务。 该代码使用 <xref:System.Transactions.TransactionScope> 语句创建 `using` 代码块并打开第一个连接，该连接自动在 <xref:System.Transactions.TransactionScope>中登记。 该事务最初作为轻型事务登记，而不是完全分布式事务。 仅当第一个连接中的命令没有引发异常时，才会在 <xref:System.Transactions.TransactionScope> 中登记第二个连接。 打开第二个连接后，事务将自动提升为完全分布式事务。 将会调用 <xref:System.Transactions.TransactionScope.Complete%2A> 方法，仅当未引发异常时，该方法才会提交事务。 如果在 <xref:System.Transactions.TransactionScope> 代码块中的任意位置引发了异常，将不会调用 `Complete` ，当在 <xref:System.Transactions.TransactionScope> 的 `using` 代码块结尾处执行 dispose 后，将回滚分布式事务。  
  
```csharp  
// This function takes arguments for the 2 connection strings and commands in order  
// to create a transaction involving two SQL Servers. It returns a value > 0 if the  
// transaction committed, 0 if the transaction rolled back. To test this code, you can
// connect to two different databases on the same server by altering the connection string,  
// or to another RDBMS such as Oracle by altering the code in the connection2 code block.  
static public int CreateTransactionScope(  
    string connectString1, string connectString2,  
    string commandText1, string commandText2)  
{  
    // Initialize the return value to zero and create a StringWriter to display results.  
    int returnValue = 0;  
    System.IO.StringWriter writer = new System.IO.StringWriter();  
  
    // Create the TransactionScope in which to execute the commands, guaranteeing  
    // that both commands will commit or roll back as a single unit of work.  
    using (TransactionScope scope = new TransactionScope())  
    {  
        using (SqlConnection connection1 = new SqlConnection(connectString1))  
        {  
            try  
            {  
                // Opening the connection automatically enlists it in the
                // TransactionScope as a lightweight transaction.  
                connection1.Open();  
  
                // Create the SqlCommand object and execute the first command.  
                SqlCommand command1 = new SqlCommand(commandText1, connection1);  
                returnValue = command1.ExecuteNonQuery();  
                writer.WriteLine("Rows to be affected by command1: {0}", returnValue);  
  
                // if you get here, this means that command1 succeeded. By nesting  
                // the using block for connection2 inside that of connection1, you  
                // conserve server and network resources by opening connection2
                // only when there is a chance that the transaction can commit.
                using (SqlConnection connection2 = new SqlConnection(connectString2))  
                    try  
                    {  
                        // The transaction is promoted to a full distributed  
                        // transaction when connection2 is opened.  
                        connection2.Open();  
  
                        // Execute the second command in the second database.  
                        returnValue = 0;  
                        SqlCommand command2 = new SqlCommand(commandText2, connection2);  
                        returnValue = command2.ExecuteNonQuery();  
                        writer.WriteLine("Rows to be affected by command2: {0}", returnValue);  
                    }  
                    catch (Exception ex)  
                    {  
                        // Display information that command2 failed.  
                        writer.WriteLine("returnValue for command2: {0}", returnValue);  
                        writer.WriteLine("Exception Message2: {0}", ex.Message);  
                    }  
            }  
            catch (Exception ex)  
            {  
                // Display information that command1 failed.  
                writer.WriteLine("returnValue for command1: {0}", returnValue);  
                writer.WriteLine("Exception Message1: {0}", ex.Message);  
            }  
        }  
  
        // If an exception has been thrown, Complete will not
        // be called and the transaction is rolled back.  
        scope.Complete();  
    }  
  
    // The returnValue is greater than 0 if the transaction committed.  
    if (returnValue > 0)  
    {  
        writer.WriteLine("Transaction was committed.");  
    }  
    else  
    {  
        // You could write additional business logic here, notify the caller by  
        // throwing a TransactionAbortedException, or log the failure.  
        writer.WriteLine("Transaction rolled back.");  
    }  
  
    // Display messages.  
    Console.WriteLine(writer.ToString());  
  
    return returnValue;  
}  
```  
  
```vb  
' This function takes arguments for the 2 connection strings and commands in order  
' to create a transaction involving two SQL Servers. It returns a value > 0 if the  
' transaction committed, 0 if the transaction rolled back. To test this code, you can
' connect to two different databases on the same server by altering the connection string,  
' or to another RDBMS such as Oracle by altering the code in the connection2 code block.  
Public Function CreateTransactionScope( _  
  ByVal connectString1 As String, ByVal connectString2 As String, _  
  ByVal commandText1 As String, ByVal commandText2 As String) As Integer  
  
    ' Initialize the return value to zero and create a StringWriter to display results.  
    Dim returnValue As Integer = 0  
    Dim writer As System.IO.StringWriter = New System.IO.StringWriter  
  
    ' Create the TransactionScope in which to execute the commands, guaranteeing  
    ' that both commands will commit or roll back as a single unit of work.  
    Using scope As New TransactionScope()  
        Using connection1 As New SqlConnection(connectString1)  
            Try  
                ' Opening the connection automatically enlists it in the
                ' TransactionScope as a lightweight transaction.  
                connection1.Open()  
  
                ' Create the SqlCommand object and execute the first command.  
                Dim command1 As SqlCommand = New SqlCommand(commandText1, connection1)  
                returnValue = command1.ExecuteNonQuery()  
                writer.WriteLine("Rows to be affected by command1: {0}", returnValue)  
  
                ' If you get here, this means that command1 succeeded. By nesting  
                ' the Using block for connection2 inside that of connection1, you  
                ' conserve server and network resources by opening connection2
                ' only when there is a chance that the transaction can commit.
                Using connection2 As New SqlConnection(connectString2)  
                    Try  
                        ' The transaction is promoted to a full distributed  
                        ' transaction when connection2 is opened.  
                        connection2.Open()  
  
                        ' Execute the second command in the second database.  
                        returnValue = 0  
                        Dim command2 As SqlCommand = New SqlCommand(commandText2, connection2)  
                        returnValue = command2.ExecuteNonQuery()  
                        writer.WriteLine("Rows to be affected by command2: {0}", returnValue)  
  
                    Catch ex As Exception  
                        ' Display information that command2 failed.  
                        writer.WriteLine("returnValue for command2: {0}", returnValue)  
                        writer.WriteLine("Exception Message2: {0}", ex.Message)  
                    End Try  
                End Using  
  
            Catch ex As Exception  
                ' Display information that command1 failed.  
                writer.WriteLine("returnValue for command1: {0}", returnValue)  
                writer.WriteLine("Exception Message1: {0}", ex.Message)  
            End Try  
        End Using  
  
        ' If an exception has been thrown, Complete will
        ' not be called and the transaction is rolled back.  
        scope.Complete()  
    End Using  
  
    ' The returnValue is greater than 0 if the transaction committed.  
    If returnValue > 0 Then  
        writer.WriteLine("Transaction was committed.")  
    Else  
        ' You could write additional business logic here, notify the caller by  
        ' throwing a TransactionAbortedException, or log the failure.  
       writer.WriteLine("Transaction rolled back.")  
     End If  
  
    ' Display messages.  
    Console.WriteLine(writer.ToString())  
  
    Return returnValue  
End Function  
```  
  
## <a name="see-also"></a>请参阅

- [事务和并发性](transactions-and-concurrency.md)
- [ADO.NET 概述](ado-net-overview.md)
