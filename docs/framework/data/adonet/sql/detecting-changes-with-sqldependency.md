---
title: 使用 SqlDependency 检测更改
description: 将 SqlDependency 对象与 SqlCommand 关联以检测查询结果与在 ADO.NET 中最初检索到的结果是否不同。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e6a58316-f005-4477-92e1-45cc2eb8c5b4
ms.openlocfilehash: b196d42477e1778c45df64b1390502645fdd649d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286463"
---
# <a name="detecting-changes-with-sqldependency"></a>使用 SqlDependency 检测更改

<xref:System.Data.SqlClient.SqlDependency> 对象可以与 <xref:System.Data.SqlClient.SqlCommand> 相关联，以便检测查询结果与最初检索到的结果不同的情况。 还可以为 `OnChange` 事件分配一个委托，该事件将在关联命令的结果变更时激发。 在执行命令之前，必须将 <xref:System.Data.SqlClient.SqlDependency> 与命令相关联。 <xref:System.Data.SqlClient.SqlDependency> 的 `HasChanges` 属性还可用于确定自第一次检索数据后，查询结果是否变更。

## <a name="security-considerations"></a>安全注意事项

依赖项基础结构依赖于调用 <xref:System.Data.SqlClient.SqlDependency.Start%2A> 时打开的 <xref:System.Data.SqlClient.SqlConnection>，以便接收已针对给定命令更改基础数据的通知。 客户端启动对 `SqlDependency.Start` 的调用的能力通过使用 <xref:System.Data.SqlClient.SqlClientPermission> 和代码访问安全性特性进行控制。 有关详细信息，请参阅[启用查询通知](enabling-query-notifications.md)和[代码访问安全和 ADO.NET](../code-access-security.md)。

### <a name="example"></a>示例

以下步骤演示了如何声明依赖项、执行命令以及在结果集更改时接收通知：

1. 启动通向服务器的 `SqlDependency` 连接。

2. 创建 <xref:System.Data.SqlClient.SqlConnection> 和 <xref:System.Data.SqlClient.SqlCommand> 对象以连接到服务器并定义 Transact-SQL 语句。

3. 创建新的 `SqlDependency` 对象或使用现有对象，然后将其绑定到 `SqlCommand` 对象。 在内部，这将创建一个 <xref:System.Data.Sql.SqlNotificationRequest> 对象，并根据需要将其绑定到命令对象。 此通知请求包含唯一标识此 `SqlDependency` 对象的内部标识符。 如果客户端侦听器尚未处于活动状态，它还会启动它。

4. 向 `SqlDependency` 对象的 `OnChange` 事件订阅事件处理程序。

5. 使用 `SqlCommand` 对象的任何 `Execute` 方法执行该命令。 因为该命令绑定到通知对象，所以服务器认识到它必须生成一个通知，并且队列信息将指向依赖项队列。

6. 停止与服务器的 `SqlDependency` 连接。

如果任何用户随后更改了基础数据，则 Microsoft SQL Server 将检测到存在此类更改的挂起通知，并发布通知，该通知通过调用 `SqlDependency.Start` 创建的基础 `SqlConnection` 进行处理并转发给客户端。 客户端侦听器接收失效消息。 然后，客户端侦听器查找关联的 `SqlDependency` 对象，并激发 `OnChange` 事件。

下面的代码段显示用于创建示例应用程序的设计模式。

```vb
Sub Initialization()
    ' Create a dependency connection.
    SqlDependency.Start(connectionString, queueName)
End Sub

Sub SomeMethod()
    ' Assume connection is an open SqlConnection.
    ' Create a new SqlCommand object.
    Using command As New SqlCommand( _
      "SELECT ShipperID, CompanyName, Phone FROM dbo.Shippers", _
      connection)

        ' Create a dependency and associate it with the SqlCommand.
        Dim dependency As New SqlDependency(command)
        ' Maintain the refernce in a class member.
        ' Subscribe to the SqlDependency event.
        AddHandler dependency.OnChange, AddressOf OnDependencyChange

        ' Execute the command.
        Using reader = command.ExecuteReader()
            ' Process the DataReader.
        End Using
    End Using
End Sub

' Handler method
Sub OnDependencyChange(ByVal sender As Object, _
    ByVal e As SqlNotificationEventArgs)
    ' Handle the event (for example, invalidate this cache entry).
End Sub

Sub Termination()
    ' Release the dependency
    SqlDependency.Stop(connectionString, queueName)
End Sub
```

```csharp
void Initialization()
{
    // Create a dependency connection.
    SqlDependency.Start(connectionString, queueName);
}

void SomeMethod()
{
    // Assume connection is an open SqlConnection.

    // Create a new SqlCommand object.
    using (SqlCommand command=new SqlCommand(
        "SELECT ShipperID, CompanyName, Phone FROM dbo.Shippers",
        connection))
    {

        // Create a dependency and associate it with the SqlCommand.
        SqlDependency dependency=new SqlDependency(command);
        // Maintain the reference in a class member.

        // Subscribe to the SqlDependency event.
        dependency.OnChange+=new
           OnChangeEventHandler(OnDependencyChange);

        // Execute the command.
        using (SqlDataReader reader = command.ExecuteReader())
        {
            // Process the DataReader.
        }
    }
}

// Handler method
void OnDependencyChange(object sender,
   SqlNotificationEventArgs e )
{
  // Handle the event (for example, invalidate this cache entry).
}

void Termination()
{
    // Release the dependency.
    SqlDependency.Stop(connectionString, queueName);
}
```

## <a name="see-also"></a>另请参阅

- [SQL Server 中的查询通知](query-notifications-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
