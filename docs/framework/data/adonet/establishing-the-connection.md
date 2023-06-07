---
title: 建立连接
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3af512f3-87d9-4005-9e2f-abb1060ff43f
ms.openlocfilehash: bf38475711a193bc69176993154f87d455aefe7d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156467"
---
# <a name="establishing-the-connection"></a>建立连接

要连接到 Microsoft SQL Server，请使用 SQL Server .NET Framework 数据提供程序的 <xref:System.Data.SqlClient.SqlConnection> 对象。 要连接到 OLE DB 数据源，请使用 OLE DB .NET Framework 数据提供程序的 <xref:System.Data.OleDb.OleDbConnection> 对象。 要连接到 ODBC 数据源，请使用 ODBC .NET Framework 数据提供程序的 <xref:System.Data.Odbc.OdbcConnection> 对象。 要连接到 Oracle 数据源，请使用 Oracle .NET Framework 数据提供程序的 <xref:System.Data.OracleClient.OracleConnection> 对象。 为了安全地存储和检索连接字符串，请参阅 [保护连接信息](protecting-connection-information.md)。  
  
## <a name="closing-connections"></a>关闭连接  

 我们建议您在使用完连接时一定要关闭连接，以便连接可以返回池。 如果 Visual Basic 或 C# 的代码中存在 `Using` 块，将自动断开连接，即使发生无法处理的异常。 有关详细信息，请参阅 [Using 语句](../../../csharp/language-reference/keywords/using-statement.md) 和 [using 语句](../../../visual-basic/language-reference/statements/using-statement.md) 。  
  
 也可以使用适合所使用的提供程序的连接对象的 `Close` 或 `Dispose` 方法。 不是显式关闭的连接可能不会添加或返回到池中。 例如，如果连接已超出范围但没有显式关闭，则仅当达到最大池大小而该连接仍然有效时，该连接才会返回到连接池中。 有关详细信息，请参阅 [OLE DB、ODBC 和 Oracle 连接池](ole-db-odbc-and-oracle-connection-pooling.md)。  
  
> [!NOTE]
> 不要在 `Close` 类的 `Dispose` 方法中对 **连接**、 **DataReader**或任何其他托管对象调用或 `Finalize` 。 在终结器中，仅释放类直接拥有的非托管资源。 如果类不拥有任何非托管资源，则不要在类定义中包含 `Finalize` 方法。 有关详细信息，请参阅 [垃圾回收](../../../standard/garbage-collection/index.md)。  
  
> [!NOTE]
> 从连接池中提取连接或将连接返回到连接池时，服务器上不会引发登录和注销事件，这是因为在将连接返回到连接池时实际上并没有将其关闭。 有关详细信息，请参阅 [SQL Server 连接池 (ADO.NET)](sql-server-connection-pooling.md)。  
  
## <a name="connecting-to-sql-server"></a>连接到 SQL Server  

 SQL Server .NET Framework 数据提供程序支持类似于 OLE DB (ADO) 连接字符串格式的连接字符串格式。 有关有效的字符串格式名称和值，请参见 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> 对象的 <xref:System.Data.SqlClient.SqlConnection> 属性。 您也可以使用 <xref:System.Data.SqlClient.SqlConnectionStringBuilder> 类在运行时创建具有有效语法的连接字符串。 有关详细信息，请参阅[连接字符串生成器](connection-string-builders.md)。  
  
 以下代码示例演示如何创建并打开与 SQL Server 数据库的连接。  
  
```vb  
' Assumes connectionString is a valid connection string.  
Using connection As New SqlConnection(connectionString)  
    connection.Open()  
    ' Do work here.  
End Using  
```  
  
```csharp  
// Assumes connectionString is a valid connection string.  
using (SqlConnection connection = new SqlConnection(connectionString))  
{  
    connection.Open();  
    // Do work here.  
}  
```  
  
### <a name="integrated-security-and-aspnet"></a>集成安全性和 ASP.NET  

 SQL Server 集成安全性（也称为受信任连接）有助于在连接到 SQL Server 时提供保护，因为它不会在连接字符串中公开用户 ID 和密码，是对连接进行身份验证的建议方法。 集成安全性使用正在执行的进程的当前安全标识或标记。 对于桌面应用程序，安全标识或标记通常是当前登录的用户的标识。  
  
 ASP.NET 应用程序的安全标识可设置为几个不同的选项之一。 为了更好地了解 ASP.NET 应用程序连接到 SQL Server 时使用的安全标识，请参阅 [ASP.NET 模拟](/previous-versions/aspnet/xh507fc5(v=vs.100))、 [ASP.NET 身份验证](/previous-versions/aspnet/eeyk640h(v=vs.100))和 [如何：使用 Windows 集成安全性访问 SQL Server](/previous-versions/aspnet/bsz5788z(v=vs.100))。  
  
## <a name="connecting-to-an-ole-db-data-source"></a>连接到 OLE DB 数据源  

 OLE DB 的 .NET Framework 数据提供程序提供了与使用 OLE DB (通过 SQLOLEDB （使用 **OleDbConnection** 对象的 OLE DB SQL Server 提供程序）公开的数据源的连接。  
  
 对于 OLE DB .NET Framework 数据提供程序，连接字符串格式与 ADO 中使用的连接字符串格式基本相同，但存在以下例外：  
  
- **Provider**关键字是必需的。  
  
- 不支持 **URL**、 **远程提供程序**和 **远程服务器** 关键字。  
  
 有关 OLE DB 连接字符串的更多信息，请参见 <xref:System.Data.OleDb.OleDbConnection.ConnectionString%2A> 主题。 您也可以使用 <xref:System.Data.OleDb.OleDbConnectionStringBuilder> 在运行时创建连接字符串。  
  
> [!NOTE]
> **OleDbConnection**对象不支持设置或检索特定于 OLE DB 提供程序的动态属性。 只支持可在 OLE DB 提供程序连接字符串中传递的属性。  
  
 以下代码示例演示如何创建和打开与 OLE DB 数据源的连接。  
  
```vb  
' Assumes connectionString is a valid connection string.  
Using connection As New OleDbConnection(connectionString)  
    connection.Open()  
    ' Do work here.  
End Using  
```  
  
```csharp  
// Assumes connectionString is a valid connection string.  
using (OleDbConnection connection =
  new OleDbConnection(connectionString))  
{  
    connection.Open();  
    // Do work here.  
}  
```  
  
## <a name="do-not-use-universal-data-link-files"></a>不要使用通用数据链接文件  

 可以 (UDL) 文件中的通用数据链接提供 **OleDbConnection** 的连接信息;但是，应避免这样做。 UDL 文件未加密，会以明文形式公开连接字符串信息。 因为 UDL 文件对您的应用程序来说是一个基于文件的外部资源，所以无法使用 .NET Framework 保护该文件。  
  
## <a name="connecting-to-an-odbc-data-source"></a>连接到 ODBC 数据源  

 适用于 ODBC 的 .NET Framework 数据提供程序提供了使用 **OdbcConnection** 对象与使用 ODBC 公开的数据源的连接。  
  
 对于 ODBC .NET Framework 数据提供程序，连接字符串的格式设计为尽可能与 ODBC 连接字符串的格式相匹配。 您还可以提供一个 ODBC 数据源名称 (DSN)。 有关 **OdbcConnection** 的更多详细信息，请参阅 <xref:System.Data.Odbc.OdbcConnection> 。  
  
 以下代码示例演示如何创建和打开与 ODBC 数据源的连接。  
  
```vb  
' Assumes connectionString is a valid connection string.  
Using connection As New OdbcConnection(connectionString)  
    connection.Open()  
    ' Do work here.  
End Using  
```  
  
```csharp  
// Assumes connectionString is a valid connection string.  
using (OdbcConnection connection =
  new OdbcConnection(connectionString))  
{  
    connection.Open();  
    // Do work here.  
}  
```  
  
## <a name="connecting-to-an-oracle-data-source"></a>连接到 Oracle 数据源  

 用于 Oracle 的 .NET Framework 数据提供程序使用 **OracleConnection** 对象提供与 oracle 数据源的连接。  
  
 对于 Oracle .NET Framework 数据提供程序，连接字符串的格式设计为尽可能与用于 Oracle 的 OLE DB 提供程序 (MSDAORA) 连接字符串格式相匹配。 有关 **OracleConnection**的更多详细信息，请参阅 <xref:System.Data.OracleClient.OracleConnection> 。  
  
 以下代码示例演示如何创建和打开与 Oracle 数据源的连接。  
  
```vb  
' Assumes connectionString is a valid connection string.  
Using connection As New OracleConnection(connectionString)  
    connection.Open()  
    ' Do work here.  
End Using  
```  
  
```csharp  
// Assumes connectionString is a valid connection string.  
using (OracleConnection connection =
  new OracleConnection(connectionString))  
{  
    connection.Open();  
    // Do work here.  
}  
OracleConnection nwindConn = new OracleConnection("Data Source=MyOracleServer;Integrated Security=yes;");  
nwindConn.Open();  
```  
  
## <a name="see-also"></a>请参阅

- [连接数据源](connecting-to-a-data-source.md)
- [连接字符串](connection-strings.md)
- [OLE DB、ODBC 和 Oracle 连接池](ole-db-odbc-and-oracle-connection-pooling.md)
- [ADO.NET 概述](ado-net-overview.md)
