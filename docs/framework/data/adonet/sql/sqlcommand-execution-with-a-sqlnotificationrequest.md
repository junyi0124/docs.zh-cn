---
title: 使用 SqlNotificationRequest 执行 SqlCommand
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1776f48f-9bea-41f6-83a4-c990c7a2c991
ms.openlocfilehash: 3115bfb80d4e5e61ed49da11e36eaa37bc24334f
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70791529"
---
# <a name="sqlcommand-execution-with-a-sqlnotificationrequest"></a>使用 SqlNotificationRequest 执行 SqlCommand

可以配置 <xref:System.Data.SqlClient.SqlCommand>，以便在从服务器获取的数据发生更改时生成通知，且再次执行查询时结果集会不同。 如果要在服务器上使用自定义通知队列或不希望维护实时对象，此功能非常有用。

## <a name="creating-the-notification-request"></a>创建通知请求

你可以使用 <xref:System.Data.Sql.SqlNotificationRequest> 对象来创建通知请求，方法是将其绑定到 `SqlCommand` 对象上。 创建完请求后，将不再需要 `SqlNotificationRequest` 对象。 您可以在队列中查询任何通知，并相应地对找到的通知进行响应。 即使应用程序关闭后再重新启动，也会出现通知。

在执行带有关联通知的命令时，对原始结果集所做的任何更改都会触发向在通知请求中配置的 SQL Server 队列发送消息的操作。

如何轮询 SQL Server 队列和解释该消息是应用程序特定的。 应用程序负责轮询队列并根据消息的内容做出响应。

> [!NOTE]
> 在使用具有 <xref:System.Data.SqlClient.SqlDependency> 的 SQL Server 通知请求时，应创建自己的队列名称，而不是使用默认的服务名称。

<xref:System.Data.Sql.SqlNotificationRequest> 没有新的客户端安全性元素。 这主要是一项服务器功能，服务器创建了用户在请求通知时必须具有的特殊权限。

### <a name="example"></a>示例

下面的代码段演示了如何创建 <xref:System.Data.Sql.SqlNotificationRequest> 并将其与 <xref:System.Data.SqlClient.SqlCommand> 相关联。

```vb
' Assume connection is an open SqlConnection.
' Create a new SqlCommand object.
Dim command As New SqlCommand( _
  "SELECT ShipperID, CompanyName, Phone FROM dbo.Shippers", connection)

' Create a SqlNotificationRequest object.
Dim notifcationRequest As New SqlNotificationRequest()
notificationRequest.id = "NotificationID"
notificationRequest.Service = "mySSBQueue"

' Associate the notification request with the command.
command.Notification = notificationRequest
' Execute the command.
command.ExecuteReader()
' Process the DataReader.
' You can use Transact-SQL syntax to periodically poll the
' SQL Server queue to see if you have a new message.
```

```csharp
// Assume connection is an open SqlConnection.
// Create a new SqlCommand object.
SqlCommand command=new SqlCommand(
 "SELECT ShipperID, CompanyName, Phone FROM dbo.Shippers", connection);

// Create a SqlNotificationRequest object.
SqlNotificationRequest notificationRequest=new SqlNotificationRequest();
notificationRequest.id="NotificationID";
notificationRequest.Service="mySSBQueue";

// Associate the notification request with the command.
command.Notification=notificationRequest;
// Execute the command.
command.ExecuteReader();
// Process the DataReader.
// You can use Transact-SQL syntax to periodically poll the
// SQL Server queue to see if you have a new message.
```

## <a name="see-also"></a>请参阅

- [SQL Server 中的查询通知](query-notifications-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
