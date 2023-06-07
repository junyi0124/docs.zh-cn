---
title: 在 SQL Server 中使用模拟自定义权限
ms.date: 03/30/2017
ms.assetid: dc733d09-1d6d-4af0-9c4b-8d24504860f1
ms.openlocfilehash: 84c5585912ba259fdcefaae186138c4466b16206
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180850"
---
# <a name="customizing-permissions-with-impersonation-in-sql-server"></a>在 SQL Server 中使用模拟自定义权限

许多应用程序都使用存储过程来访问数据，依靠所属权链接来限制对基表的访问。 您可以授予针对存储过程的 EXECUTE 权限，撤消或拒绝针对基表的权限。 如果存储过程和表具有相同的所有者，则 SQL Server 不检查调用方的权限。 但是，如果对象具有不同的所有者或使用动态 SQL，则所属权链接不起作用。  
  
 当调用方对引用的数据库对象没有权限时，您可以在存储过程中使用 EXECUTE AS 子句。 EXECUTE AS 子句的作用是将执行上下文切换到代理用户。 所有代码以及任何对嵌套存储过程或触发器的调用均在代理用户的安全上下文下执行。 仅在执行过程后或发出 REVERT 语句时，执行上下文才会还原为原始调用方。  
  
## <a name="context-switching-with-the-execute-as-statement"></a>使用 EXECUTE AS 语句切换上下文  

 使用 Transact-SQL EXECUTE AS 语句可以通过模拟其他登录名或数据库用户来切换一个语句的执行上下文。 这是一种以其他用户身份测试查询和过程的有用技术。  
  
```sql  
EXECUTE AS LOGIN = 'loginName';  
EXECUTE AS USER = 'userName';  
```  
  
 您必须对要模拟的登录名或用户具有 IMPERSONATE 权限。 所有数据库的 `sysadmin` 及其拥有的数据库中的 `db_owner` 角色成员都隐含具有此权限。  
  
## <a name="granting-permissions-with-the-execute-as-clause"></a>使用 EXECUTE AS 子句授予权限  

 您可以在存储过程、触发器或用户定义函数（内联表值函数除外）的定义头中使用 EXECUTE AS 子句。 这会使过程在 EXECUTE AS 子句中指定的用户名或关键字的上下文中运行。 您可以在数据库中创建一个不映射到登录名的代理用户，仅为其授予对过程所访问的对象的必要权限。 只有在 EXECUTE AS 子句中指定的代理用户才必须对模块所访问的所有对象具有权限。  
  
> [!NOTE]
> 某些操作（如 TRUNCATE TABLE）没有可授予的权限。 通过在过程中合并该语句并指定具有 ALTER TABLE 权限的代理用户，可以将截断表的权限扩展到仅对过程具有 EXECUTE 权限的调用方。  
  
 EXECUTE AS 子句中指定的上下文在过程（包括嵌套的过程和触发器）期间有效。 当执行完成或发出 REVERT 语句时，上下文恢复到调用方。  
  
 在过程中使用 EXECUTE AS 子句包含三个步骤。  
  
1. 在数据库中创建一个不映射到登录名的代理用户。 这不是必需的，但它有助于管理权限。  
  
```sql
CREATE USER proxyUser WITHOUT LOGIN  
```  
  
1. 授予代理用户必要的权限。  
  
2. 将 EXECUTE AS 子句添加到存储过程或用户定义函数中。  
  
```sql
CREATE PROCEDURE [procName] WITH EXECUTE AS 'proxyUser' AS ...  
```  
  
> [!NOTE]
> 要求审核的应用程序可能中断，因为未保留调用方的原始安全上下文。 返回当前用户标识的内置函数（如 SESSION_USER、USER 或 USER_NAME）返回与 EXECUTE AS 子句关联的用户，而不是原始调用方。  
  
### <a name="using-execute-as-with-revert"></a>与 REVERT 一起使用 EXECUTE AS  

 可以使用 Transact-SQL REVERT 语句来返回到原始执行上下文。  
  
 如果变量包含正确的值，则可选子句 WITH NO REVERT COOKIE = @variableName 允许您将执行上下文切换回调用方 @variableName 。 这允许您在使用连接池的环境中将执行上下文切换回调用方。 由于的值 @variableName 仅对 EXECUTE AS 语句的调用方是已知的，因此调用方可以保证调用应用程序的最终用户无法更改执行上下文。 当连接关闭时，它将返回到池中。 有关 ADO.NET 中的连接池的详细信息，请参阅 [ (ADO.NET) SQL Server 连接池 ](../sql-server-connection-pooling.md)。  
  
### <a name="specifying-the-execution-context"></a>指定执行上下文  

 除了指定用户外，还可以将 EXECUTE AS 与以下任意关键字一起使用。  
  
- CALLER。 以 CALLER 身份执行是默认设置；如果未指定其他选项，则过程将在调用方的安全上下文中运行。  
  
- OWNER。 以 OWNER 身份执行将在过程所有者的上下文中执行过程。 如果过程是在 `dbo` 或数据库所有者拥有的架构中创建的，则此过程将以无限制权限执行。  
  
- SELF。 以 SELF 身份执行将在存储过程创建者的安全上下文中执行。 这相当于以指定用户的身份执行，这里的指定用户是指创建或改变过程的人。  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 安全性概述](overview-of-sql-server-security.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [在 SQL Server 中使用存储过程管理权限](managing-permissions-with-stored-procedures-in-sql-server.md)
- [在 SQL Server 中编写安全动态 SQL](writing-secure-dynamic-sql-in-sql-server.md)
- [SQL Server 中的签名存储过程](signing-stored-procedures-in-sql-server.md)
- [使用存储过程修改数据](../modifying-data-with-stored-procedures.md)
- [ADO.NET 概述](../ado-net-overview.md)
