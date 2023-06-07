---
title: 在 SQL Server 中编写安全的动态 SQL
ms.date: 03/30/2017
ms.assetid: df5512b0-c249-40d2-82f9-f9a2ce6665bc
ms.openlocfilehash: c598427a17ceb289f75fab481a55016f0efe5624
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91147445"
---
# <a name="writing-secure-dynamic-sql-in-sql-server"></a>在 SQL Server 中编写安全的动态 SQL

SQL 注入是恶意用户输入 Transact-SQL 语句而不是有效输入的过程。 如果输入未经验证直接传递到服务器，并且应用程序无意中执行了注入的代码，则攻击可能会损坏或破坏数据。  
  
 构成 SQL 语句的任何过程都应进行注入漏洞审阅，因为 SQL Server 将执行其接收到的所有语法有效的查询。 一个有经验的、坚定的攻击者甚至可以操作参数化数据。 如果使用动态 SQL，请确保参数化命令，并且切勿直接将参数值包含在查询字符串中。  
  
## <a name="anatomy-of-a-sql-injection-attack"></a>SQL 注入攻击剖析  

 注入过程的工作方式是提前终止文本字符串，然后追加一个新的命令。 由于插入的命令可能在执行前追加其他字符串，因此攻击者将用注释标记“--”来终止注入的字符串。 执行时，此后的文本将被忽略。 可以使用分号 (;) 分隔符插入多个命令。  
  
 只要注入的 SQL 代码语法正确，便无法采用编程方式来检测篡改。 因此，必须验证所有用户输入，并仔细检查在您所用的服务器中执行构造 SQL 命令的代码。 绝不串联未验证的用户输入。 字符串串联是脚本注入的主要输入点。  
  
 下面是一些有用的指导原则：  
  
- 切勿直接从用户输入构建 Transact-SQL 语句；使用存储过程来验证用户输入。  
  
- 通过测试类型、长度、格式和范围来验证用户输入。 使用 Transact-SQL QUOTENAME() 函数转义系统名称，或使用 REPLACE() 函数转义字符串中的任何字符。  
  
- 在应用程序的每一层中实施多层验证。  
  
- 测试输入的大小和数据类型，强制执行适当的限制。 这有助于防止有意造成的缓冲区溢出。  
  
- 测试字符串变量的内容，只接受所需的值。 拒绝包含二进制数据、转义序列和注释字符的输入内容。  
  
- 使用 XML 文档时，根据数据的架构对输入的所有数据进行验证。  
  
- 在多层环境中，所有数据都应该在验证之后才允许进入可信区域。  
  
- 不接受以下来自可构造文件名的字段中的字符串：AUX、CLOCK$、COM1 到 COM8、CON、CONFIG$、LPT1 到 LPT8、NUL 和 PRN。  
  
- 将 <xref:System.Data.SqlClient.SqlParameter> 对象与存储过程和命令一起使用以提供类型检查和长度验证。  
  
- 在客户端代码中使用 <xref:System.Text.RegularExpressions.Regex> 表达式来筛选无效字符。  
  
## <a name="dynamic-sql-strategies"></a>动态 SQL 策略  

 在过程代码中执行动态创建的 SQL 语句会中断所有权链，从而导致 SQL Server 针对动态 SQL 访问的对象检查调用方的权限。  
  
 SQL Server 提供一些方法，用于向用户授予使用存储过程和可执行动态 SQL 的用户定义函数来访问数据的权限。  
  
- 如[在 SQL Server 中使用模拟自定义权限](customizing-permissions-with-impersonation-in-sql-server.md)中所述，将模拟用于 Transact-SQL EXECUTE AS 子句。  
  
- 使用证书对存储过程签名，如[在 SQL Server 中对存储过程签名](signing-stored-procedures-in-sql-server.md)中所述。  
  
### <a name="execute-as"></a>EXECUTE AS  

 EXECUTE AS 子句会将调用方的权限替换为 EXECUTE AS 子句中指定的用户的权限。 嵌套存储过程或触发器会在代理用户的安全上下文下执行。 这可能会破坏依赖行级安全性或需要审核的应用程序。 某些返回用户标识的函数会返回 EXECUTE AS 子句中指定的用户，而不是原始调用方。 仅在执行过程后或发出 REVERT 语句时，执行上下文才会还原为原始调用方。  
  
### <a name="certificate-signing"></a>证书签名  

 执行已使用证书签名的存储过程时，授予证书用户的权限将与调用方的权限合并。 执行上下文保持不变；证书用户不会模拟调用方。 对存储过程进行签名需要执行几个步骤。 每次修改过程时，都必须重新签名。  
  
### <a name="cross-database-access"></a>跨数据库访问  

 在执行动态创建的 SQL 语句的情况下，跨数据库所有权链不起作用。 在 SQL Server 中，可以通过创建一个可访问另一个数据库中数据的存储过程并用两个数据库中都存在的证书为此过程签名来解决这个问题。 这样，用户就可以访问该过程使用的数据库资源，而无需授予他们数据库访问或权限。  
  
## <a name="external-resources"></a>外部资源  

 有关详细信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|SQL Server 联机丛书中的[存储过程](/sql/relational-databases/stored-procedures/stored-procedures-database-engine)和 [SQL 注入](/sql/relational-databases/security/sql-injection)|上述主题描述了如何创建存储过程以及 SQL 注入的工作原理。|  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 安全性概述](overview-of-sql-server-security.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [在 SQL Server 中使用存储过程管理权限](managing-permissions-with-stored-procedures-in-sql-server.md)
- [SQL Server 中的签名存储过程](signing-stored-procedures-in-sql-server.md)
- [在 SQL Server 中使用模拟自定义权限](customizing-permissions-with-impersonation-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
