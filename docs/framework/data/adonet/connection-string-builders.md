---
title: 连接字符串生成器
description: 了解用于 ADO.NET 中不同提供程序的连接字符串生成器类，它们都继承自 DbConnectionStringBuilder。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.openlocfilehash: e8ab0fa98eeb8ec8966c52c87c4d7aea1fbd8efc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148459"
---
# <a name="connection-string-builders"></a>连接字符串生成器

在早期版本的 ADO.NET 中，不会发生带有串联字符串值的连接字符串的编译时检查，因此在运行时，不正确的关键字生成了 <xref:System.ArgumentException> 。 每个 .NET Framework 数据访问接口支持不同的连接字符串关键字语法，这使得在手动操作时很难构建有效的连接字符串。 为了解决此问题，ADO.NET 2.0 为每个 .NET Framework 数据提供程序引入了新的连接字符串生成器。 每个数据提供程序包括一个从 <xref:System.Data.Common.DbConnectionStringBuilder> 继承的强类型连接字符串生成器类。 下表列出了 .NET Framework 数据提供程序及其关联的连接字符串生成器类。  
  
|提供程序|ConnectionStringBuilder 类|  
|--------------|-----------------------------------|  
|<xref:System.Data.SqlClient>|<xref:System.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType>|  
|<xref:System.Data.OleDb>|<xref:System.Data.OleDb.OleDbConnectionStringBuilder?displayProperty=nameWithType>|  
|<xref:System.Data.Odbc>|<xref:System.Data.Odbc.OdbcConnectionStringBuilder?displayProperty=nameWithType>|  
|<xref:System.Data.OracleClient>|<xref:System.Data.OracleClient.OracleConnectionStringBuilder?displayProperty=nameWithType>|  
  
## <a name="connection-string-injection-attacks"></a>连接字符串注入式攻击  

 当使用动态字符串串联根据用户输入生成连接字符串时，可能发生连接字符串注入式攻击。 如果未验证字符串并且未转义恶意文本或字符，则攻击者可能会访问服务器上的敏感数据或其他资源。 例如，攻击者可以通过提供分号并追加其他值来发起攻击。 连接字符串通过“last one wins”算法分析，恶意的输入被替换为合法的值。  
  
 连接字符串生成器类旨在排除推测，防止出现语法错误和安全漏洞。 它们提供与每个数据提供程序允许的已知键/值对相对应的方法和属性。 每个类都保持一个固定的同义词集合，可以将同义词转换为相应的已知键名。 将执行键/值对的有效性检查，无效对会引发异常。 此外，还会以一种安全方式处理插入的值。  
  
 下面的示例演示 <xref:System.Data.SqlClient.SqlConnectionStringBuilder> 如何处理为 `Initial Catalog` 设置插入的额外值。  
  
```vb  
Dim builder As New System.Data.SqlClient.SqlConnectionStringBuilder  
builder("Data Source") = "(local)"  
builder("Integrated Security") = True  
builder("Initial Catalog") = "AdventureWorks;NewValue=Bad"  
Console.WriteLine(builder.ConnectionString)  
```  
  
```csharp  
System.Data.SqlClient.SqlConnectionStringBuilder builder =  
  new System.Data.SqlClient.SqlConnectionStringBuilder();  
builder["Data Source"] = "(local)";  
builder["integrated Security"] = true;  
builder["Initial Catalog"] = "AdventureWorks;NewValue=Bad";  
Console.WriteLine(builder.ConnectionString);  
```  
  
 输出结果表明，通过用双引号转义该额外值而不作为新的键/值对将其追加到连接字符串，<xref:System.Data.SqlClient.SqlConnectionStringBuilder> 可以正确处理此额外值。  
  
```output  
data source=(local);Integrated Security=True;  
initial catalog="AdventureWorks;NewValue=Bad"  
```  
  
## <a name="building-connection-strings-from-configuration-files"></a>从配置文件生成连接字符串  

 如果事先知道连接字符串的某些元素，则可以将其存储在配置文件中，并在运行时检索它们以构造完整连接字符串。 例如，可能事先知道数据库的名称，但不知道服务器的名称。 或者，您可能希望用户在运行时提供用户名和密码，而不能在连接字符串中插入其他值。  
  
 连接字符串生成器的一个重载构造函数将 <xref:System.String> 作为自变量，这可让你提供部分连接字符串，然后通过用户输入使这部分连接字符串成为完整字符串。 该部分连接字符串可以存储在配置文件中并在运行时进行检索。  
  
> [!NOTE]
> <xref:System.Configuration> 命名空间允许通过编程方式访问配置文件（对 Web 应用程序使用 <xref:System.Web.Configuration.WebConfigurationManager>，对 Windows 应用程序使用 <xref:System.Configuration.ConfigurationManager>）。 有关使用连接字符串和配置文件的详细信息，请参阅 [连接字符串和配置文件](connection-strings-and-configuration-files.md)。  
  
### <a name="example"></a>示例  

 此示例演示如何从配置文件中检索部分连接字符串并通过设置 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> 的 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A>、<xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> 和 <xref:System.Data.SqlClient.SqlConnectionStringBuilder> 属性完成该连接字符串。 配置文件定义如下。  
  
```xml  
<connectionStrings>  
  <clear/>  
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"  
    providerName="System.Data.SqlClient" />  
</connectionStrings>  
```  
  
> [!NOTE]
> 必须在项目中设置对 `System.Configuration.dll` 的引用，才能运行代码。  
  
 [!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlConnectionStringBuilder.UserNamePwd/CS/source.cs#1)]
 [!code-vb[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlConnectionStringBuilder.UserNamePwd/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [连接字符串](connection-strings.md)
- [隐私和数据安全性](privacy-and-data-security.md)
- [ADO.NET 概述](ado-net-overview.md)
