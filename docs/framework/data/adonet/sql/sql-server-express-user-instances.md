---
title: SQL Server Express 用户实例
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 00c12376-cb26-4317-86ad-e6e9c089be57
ms.openlocfilehash: 401b62f56918e8ac406a5ee2dda2252d328592bc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91147575"
---
# <a name="sql-server-express-user-instances"></a>SQL Server Express 用户实例

Microsoft SQL Server 学习版 (SQL Server Express) 支持用户实例功能，只有在使用用于 SQL Server 的 .NET Framework 数据提供程序 (`SqlClient`) 时该功能才可用。 用户实例是由父实例生成的 SQL Server Express 数据库引擎的单独实例。 用户实例允许非本地计算机管理员的用户附加并连接到 SQL Server Express 数据库。 每个实例在单个用户的安全上下文下运行，每个用户一个实例。  
  
## <a name="user-instance-capabilities"></a>用户实例功能  

 用户实例适用于在最低特权用户帐户下运行 Windows (LUA) 的用户。 每个用户都有 SQL Server 系统管理员 (在 `sysadmin` 其计算机上运行的实例) 权限，而无需以 Windows 管理员身份运行。 以有限权限执行用户实例的软件无法进行系统范围的更改，因为 SQL Server Express 实例是在用户的非管理员 Windows 帐户下运行的，而不是以服务形式运行。 用户实例独立于父实例和该计算机上运行的任何其他用户实例。 在用户实例上运行的数据库仅以单用户模式打开，多个用户无法连接到在用户实例上运行的数据库。 还会为用户实例禁用复制和分布式查询。
  
> [!NOTE]
> 对于已经在自己的计算机上具有管理员权限的用户，或在涉及多个数据库用户的场景中，不需要用户实例。  
  
## <a name="enabling-user-instances"></a>启用用户实例  

 若要生成用户实例，必须运行 SQL Server Express 父实例。 安装 SQL Server Express 后，默认情况下将启用用户实例，且对父实例执行 sp_configure 系统存储过程的系统管理员可以显式启用或禁用用户实例****。  
  
```sql  
-- Enable user instances.  
sp_configure 'user instances enabled','1'
  
-- Disable user instances.  
sp_configure 'user instances enabled','0'  
```  
  
 用于用户实例的网络协议必须为本地命名管道。 用户实例无法在 SQL Server 的远程实例上启动，并且不允许 SQL Server 登录名。  
  
## <a name="connecting-to-a-user-instance"></a>连接到用户实例  

 `User Instance` 和 `AttachDBFilename`<xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> 关键字允许 <xref:System.Data.SqlClient.SqlConnection> 连接到用户实例。 <xref:System.Data.SqlClient.SqlConnectionStringBuilder>`UserInstance` 和 `AttachDBFilename` 属性还支持用户实例。  
  
 请注意下面所示的示例连接字符串：  
  
- `Data Source` 关键字引用生成用户实例的 SQL Server Express 的父实例。 默认实例为 .\sqlexpress。  
  
- 将 `Integrated Security` 设置为 `true`。 若要连接到用户实例，需要 Windows 身份验证；不支持 SQL Server 登录名。  
  
- `User Instance` 设置为 `true`，这将调用一个用户实例。 （默认值为 `false`。）  
  
- `AttachDbFileName` 连接字符串关键字用于附加主数据库文件 (.mdf)，该文件必须包含完整路径名。 `AttachDbFileName` 还与 <xref:System.Data.SqlClient.SqlConnection> 连接字符串内的“extended properties”和“initial file name”键相对应。  
  
- 管道符号中包含的 `|DataDirectory|` 替换字符串是指打开连接的应用程序的数据目录，并提供指示 .mdf 和 .ldf 数据库和日志文件位置的相对路径。 如果要在其他位置查找这些文件，必须提供文件的完整路径。  
  
```text
Data Source=.\\SQLExpress;Integrated Security=true;  
User Instance=true;AttachDBFilename=|DataDirectory|\InstanceDB.mdf;  
Initial Catalog=InstanceDB;  
```  
  
> [!NOTE]
> 还可以使用 <xref:System.Data.SqlClient.SqlConnectionStringBuilder><xref:System.Data.SqlClient.SqlConnectionStringBuilder.UserInstance%2A> 和 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.AttachDBFilename%2A> 属性在运行时生成连接字符串。  
  
### <a name="using-the-124datadirectory124-substitution-string"></a>使用 &#124;DataDirectory&#124; 替换字符串  

 在 ADO.NET 2.0 中，随着 `AttachDbFileName`（包含在管道符号中）替代字符串的引入，对 `|DataDirectory|` 进行了扩展。 `DataDirectory` 与 `AttachDbFileName` 结合使用可指示数据文件的相对路径，从而允许开发人员创建基于数据源的相对路径（而无需指定完整路径）的连接字符串。  
  
 `DataDirectory` 指向的物理位置取决于应用程序的类型。 在此示例中，要附加的 Northwind.mdf 文件位于应用程序的 \app_data 文件夹中。  
  
```text
Data Source=.\\SQLExpress;Integrated Security=true;  
User Instance=true;  
AttachDBFilename=|DataDirectory|\app_data\Northwind.mdf;  
Initial Catalog=Northwind;  
```  
  
 使用 `DataDirectory` 时，生成的文件路径在目录结构中不能比替换字符串指向的目录更高。 例如，如果完全展开的 `DataDirectory` 是 C:\AppDirectory\app_data，则上面显示的示例连接字符串将有效，因为它位于 c:\AppDirectory 之下。 但是，如果尝试将 `DataDirectory` 指定为 `|DataDirectory|\..\data`，将会产生一个错误，因为 \data 不是 \AppDirectory 的子目录。  
  
 如果连接字符串的替换字符串格式不正确，则会引发 <xref:System.ArgumentException>。  
  
> [!NOTE]
> <xref:System.Data.SqlClient> 将替换字符串解析为本地计算机文件系统的完整路径。 因此，不支持远程服务器、HTTP 和 UNC 路径名称。 如果服务器不在本地计算机上，则在打开连接时将引发异常。  
  
 打开 <xref:System.Data.SqlClient.SqlConnection> 时，它将从默认的 SQL Server Express 实例重定向到在调用方帐户下运行的运行时启动实例。  
  
> [!NOTE]
> 可能需要增加 <xref:System.Data.SqlClient.SqlConnection.ConnectionTimeout%2A> 值，因为加载用户实例可能比常规实例需要更长的时间。  
  
 以下代码段将打开一个新的 `SqlConnection`，在控制台窗口中显示连接字符串，然后在退出 `using` 代码块时关闭连接。  
  
```vb  
Private Sub OpenSqlConnection()  
    ' Retrieve the connection string.  
    Dim connectionString As String = GetConnectionString()  
  
    Using connection As New SqlConnection(connectionString)  
        connection.Open()  
        Console.WriteLine("ConnectionString: {0}", _  
           connection.ConnectionString)  
    End Using  
End Sub  
```  
  
```csharp  
private static void OpenSqlConnection()  
{  
    // Retrieve the connection string.  
    string connectionString = GetConnectionString();  
  
    using (SqlConnection connection =
        new SqlConnection(connectionString))  
    {  
        connection.Open();  
        Console.WriteLine("ConnectionString: {0}",
             connection.ConnectionString);  
    }  
}  
```  
  
> [!NOTE]
> 在 SQL Server 中运行的公共语言运行时 (CLR) 代码不支持用户实例。 如果在连接字符串中有 <xref:System.InvalidOperationException> 的 `Open` 上调用 <xref:System.Data.SqlClient.SqlConnection>，将引发 `User Instance=true`。  
  
## <a name="lifetime-of-a-user-instance-connection"></a>用户实例连接的生存期  

 与作为服务运行的 SQL Server 版本不同，SQL Server Express 实例无需手动启动和停止。 用户每次登录并连接到用户实例时，如果用户实例尚未运行，则启动该实例。 用户实例数据库设置了 `AutoClose` 选项，以便在一段非活动期后自动关闭数据库。 在实例的最后一个连接关闭后，启动的 sqlservr.exe 进程将在有限的超时期限内持续运行，因此，如果在超时过期之前打开另一个连接，无需重新启动。 如果在超时期限过期之前没有打开新连接，用户实例将自动关闭。 父实例的系统管理员可为用户实例设置超时期限的持续时间，方法为：使用 sp_configure 更改“用户实例超时”选项   。 默认值为 60 分钟。  
  
> [!NOTE]
> 如果在值大于 0 的连接字符串中使用 `Min Pool Size`，连接池程序将始终保持几个打开的连接，并且用户实例不会自动关闭。  
  
## <a name="how-user-instances-work"></a>用户实例工作方式  

 为每个用户首次生成用户实例时，系统会将 master 和 msdb 系统数据库从模板数据文件夹复制到用户的本地应用程序数据存储库目录下的某个路径中，供用户实例专用   。 此路径通常为 `C:\Documents and Settings\<UserName>\Local Settings\Application Data\Microsoft\Microsoft SQL Server Data\SQLEXPRESS`。 启动用户实例时，tempdb、日志和跟踪文件也将被写入到此目录中  。 为实例生成一个名称，该名称保证对每个用户都是唯一的。  
  
 默认情况下，Windows Builtin\Users 组的所有成员都会被授予连接本地实例的权限，以及对 SQL Server 二进制文件的读取和执行权限。 在对承载用户实例的调用用户的凭据进行验证后，该用户将成为该实例上的 `sysadmin`。 仅为用户实例启用共享内存，这意味着只能在本地计算机上执行操作。  
  
 必须为用户授予对在连接字符串中指定的 .mdf 和 .ldf 文件的读取和写入权限。  
  
> [!NOTE]
> .mdf 和 .ldf 文件分别表示数据库和日志文件。 这两个文件是一个匹配的集，因此在备份和还原操作期间必须小心谨慎。 数据库文件包含有关日志文件的确切版本的信息，如果数据库与错误的日志文件耦合，则该数据库将不会打开。  
  
 若要避免数据损坏，用户实例中的数据库将通过独占访问的方式打开。 如果两个不同的用户实例共享同一台计算机上的同一个数据库，则第一个实例上的用户必须关闭该数据库，然后才能在第二个实例中打开数据库。  
  
## <a name="user-instance-scenarios"></a>用户实例方案  

 用户实例为数据库应用程序开发人员提供了一个 SQL Server 数据存储区，该存储区不依赖于在其开发计算机上具有管理帐户的开发人员。 用户实例基于 Access/Jet 模型，在该模型中，数据库应用程序只连接到一个文件，用户自动对所有数据库对象具有完全权限，而不需要系统管理员干预来授予权限。 它适用于以下情况：用户在最低特权用户帐户 (LUA) 下运行，并且在服务器或本地计算机上没有管理权限，但需要创建数据库对象和应用程序。 用户实例允许用户在运行时创建在用户自己的安全上下文下运行的实例，而不是在具有更高权限的系统服务的安全上下文中运行的实例。  
  
> [!IMPORTANT]
> 用户实例仅应在所有使用它的应用程序完全受信任的场景中使用。  
  
 用户实例场景包括：  
  
- 不需要共享数据的任何单用户应用程序。  
  
- ClickOnce 部署。 如果目标计算机上已安装 .NET Framework 2.0 (或更高版本) 和 SQL Server Express，则非管理员用户可以安装并使用作为 ClickOnce 操作的结果下载的安装包。 请注意，如果 SQL Server Express 是安装的一部分，则管理员必须安装它。 有关详细信息，请参阅 [Windows 窗体的 ClickOnce 部署](/dotnet/desktop/winforms/clickonce-deployment-for-windows-forms)。
  
- 使用 Windows 身份验证的专用 ASP.NET 托管。 单个 SQL Server Express 实例可以承载在 Intranet 上。 应用程序使用 ASPNET Windows 帐户进行连接，而不是使用模拟。 用户实例不应用于第三方或共享托管场景，在这些场景中，所有应用程序将共享同一个用户实例，且不再彼此隔离。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 和 ADO.NET](index.md)
- [连接字符串](../connection-strings.md)
- [连接数据源](../connecting-to-a-data-source.md)
- [ADO.NET 概述](../ado-net-overview.md)
