---
title: 并行执行
ms.date: 03/30/2017
ms.assetid: 9f9ba96d-9f89-4f65-bb2f-6860879f4393
ms.openlocfilehash: 609fc7b7cefd92e38ecfff54e5ac1651e855e4b7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91188933"
---
# <a name="side-by-side-execution-in-adonet"></a>ADO.NET 中的并行执行

在 .NET Framework 中并行执行的功能是在安装了多个版本的 .NET Framework 的计算机上执行应用程序，以独占方式使用编译应用程序的版本。 有关配置并行执行的详细信息，请参阅并行 [执行](../../deployment/side-by-side-execution.md)。  
  
 使用 .NET Framework 的一种版本编译的应用程序可以在 .NET Framework 的不同版本上运行。 但是，我们建议你为每个已安装的 .NET Framework 版本编译一个应用程序版本，并单独运行。 无论在哪种情况下，都应该注意版本间的 ADO.NET 更改，这些更改可能影响应用程序的向前兼容性或向后兼容性。  
  
## <a name="forward-compatibility-and-backward-compatibility"></a>向前兼容性和向后兼容性  

 向前兼容性意味着应用程序可以使用 .NET Framework 的早期版本进行编译，但仍会在 .NET Framework 的更高版本上成功运行。 为 .NET Framework 版本1.1 编写的 ADO.NET 代码与更高版本是向前兼容的。  
  
 向后兼容性是指将应用程序编译为较新版本的 .NET Framework，但会继续在早期版本的 .NET Framework 上运行，而不会丢失任何功能。 当然，在 .NET Framework 的新版本中引入的功能将不会出现这种情况。  
  
## <a name="the-net-framework-data-provider-for-odbc"></a>ODBC .NET Framework 数据提供程序  

 从版本1.1 开始，ODBC () 的 .NET Framework 数据提供程序 <xref:System.Data.Odbc> 作为 .NET Framework 的一部分包含在内。
  
 如果你的应用程序是为使用 ODBC 数据访问接口来连接到数据源的 .NET Framework 版本1.0 开发的，并且你想要在 .NET Framework 版本1.1 或更高版本上运行该应用程序，则必须将该 ODBC 数据提供程序的命名空间更新为 **system.object**。 然后必须将其重新编译为较新版本的 .NET Framework。  
  
 如果为 .NET Framework 版本2.0 或更高版本开发的应用程序，该应用程序使用 ODBC 数据提供程序连接到您的数据源，并且您想要在 .NET Framework 版本1.0 上运行该应用程序，则必须下载该 ODBC 数据提供程序并将其安装在 .NET Framework 版本1.0 系统上。 然后，必须将该 ODBC 数据提供程序的命名空间更改为 "node.js **"，然后**重新编译该应用程序的 .NET Framework 版本1.0。  
  
## <a name="the-net-framework-data-provider-for-oracle"></a>Oracle .NET Framework 数据提供程序  

 从版本1.1 开始，适用于 Oracle () 的 .NET Framework 数据提供程序 <xref:System.Data.OracleClient> 作为 .NET Framework 的一部分包含在内。
  
 如果为 .NET Framework 版本2.0 或更高版本开发的应用程序，该应用程序使用数据访问接口连接到您的数据源，并且您想要在 .NET Framework 版本1.0 上运行该应用程序，则必须下载数据提供程序并将其安装在 .NET Framework 版本1.0 系统上。  
  
## <a name="code-access-security"></a>代码访问安全性  

 .NET Framework 版本 1.0 (中 .NET Framework 数据访问接口 <xref:System.Data.SqlClient> ， <xref:System.Data.OleDb> 需要) 才能运行 FullTrust 权限。 尝试在具有小于 FullTrust 权限的区域中使用 .NET Framework 版本1.0 中的 .NET Framework k 数据提供程序会导致 <xref:System.Security.SecurityException> 。  
  
 但是，从 .NET Framework 版本2.0 开始，所有 .NET Framework 数据提供程序都可在部分受信任的区域中使用。 此外，.NET Framework 版本1.1 中的 .NET Framework 数据提供程序添加了一项新的安全功能。 通过该功能可以限制特定安全区域中可以使用的连接字符串。 您也可以对特定安全区域禁用空白密码。 有关更多信息，请参见 [Code Access Security and ADO.NET](code-access-security.md)。  
  
 由于每个 .NET Framework 都有一个单独的 Security.config 文件，因此与安全设置不存在兼容性问题。 但是，如果你的应用程序依赖于 .NET Framework 版本1.1 及更高版本中包含的 ADO.NET 的其他安全功能，你将无法将其分发给版本1.0 系统。  
  
## <a name="sqlcommand-execution"></a>SqlCommand 执行  

 从 .NET Framework 版本1.1 开始， <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> 更改了在数据源中执行命令的方式。  
  
 在 .NET Framework 版本1.0 中，在 <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> **sp_executesql** 存储过程的上下文中执行所有命令。 因此，影响连接状态的命令（例如，SET NOCOUNT ON）只应用于当前命令的执行。 对于在连接打开时执行的任何后续命令，连接状态不会被修改。  
  
 在 .NET Framework 版本1.1 及更高版本中， <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> 如果该命令包含参数，则仅在 **sp_executesql** 存储过程的上下文中执行命令，这可提供性能优势。 因此，如果非参数化命令中包含影响连接状态的命令，会修改在连接打开时执行的所有后续命令的连接状态。  
  
 请考虑下面这个在 <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> 调用中执行的批命令。  
  
```sql
SET NOCOUNT ON;  
SELECT * FROM dbo.Customers;  
```  
  
 在 .NET Framework 版本1.1 及更高版本中，在连接打开时执行的任何后续命令都将继续 NOCOUNT。 在 .NET Framework 版本1.0 中，NOCOUNT 仅适用于当前命令执行。  
  
 此更改会影响应用程序的向前和向后兼容性，前提是你依赖于的 <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A> 任一版本 .NET Framework 的行为。  
  
 对于在早期版本和更高版本的 .NET Framework 上运行的应用程序，你可以编写代码以确保无论你运行的版本是什么，行为都是相同的。 如果要确保某个命令修改所有后续命令的连接状态，建议您使用 <xref:System.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> 来执行该命令。 如果要确保某个命令不修改所有后续命令的连接，建议您在命令中包括用于重置连接状态的命令。 例如：  
  
```sql
SET NOCOUNT ON;  
SELECT * FROM dbo.Customers;  
SET NOCOUNT OFF;  
```  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
