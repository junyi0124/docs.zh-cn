---
title: SQL Server 安全性概述
description: 了解 SQL Server 安全体系结构，以了解哪些特性和功能应对已知的威胁，并预测将来的威胁。
ms.date: 03/30/2017
ms.assetid: ae66dd75-5c16-4cc0-9e12-774dd26d3fb9
ms.openlocfilehash: ba396774e760a550246d0f0507984d3f7212204b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172653"
---
# <a name="overview-of-sql-server-security"></a>SQL Server 安全性概述

具有重叠安全层的全面防御策略是抵御安全威胁的最佳方式。 SQL Server 提供的安全体系结构旨在允许数据库管理员和开发人员创建安全的数据库应用程序并抵御威胁。 通过引入新功能，SQL Server 的每个版本都在先前的 SQL Server 版本基础上得到改善。 但是，安全性并不是现成的。 每个应用程序都具有其独特的安全要求。 开发人员需要了解哪些功能组合最适合抵御已知的威胁，并需要预见未来可能出现的威胁。  
  
 SQL Server 实例包含以服务器为首的实体的分层集合。 每个服务器均包含多个数据库，而每个数据库均包含可保护对象的集合。 每个 SQL Server 安全对象都有可以授予主体的关联 *权限* ，该 *主体*是授予访问 SQL Server 的个人、组或进程。 SQL Server 安全框架通过 *身份验证* 和 *授权*管理对安全实体的访问。  
  
- 身份验证是指通过提交服务器评估的凭据以登录到主体请求访问的 SQL Server 的过程。 身份验证可以确定接受身份验证的用户或进程的标识。  
  
- 授权是确定主体可以访问哪些安全对象资源以及哪些操作允许用于这些资源的过程。  
  
 本节中的主题介绍 SQL Server 安全基础知识，并提供到相关版本 SQL Server 联机丛书中完整文档的链接。  
  
## <a name="in-this-section"></a>本节内容  

 [SQL Server 中的身份验证](authentication-in-sql-server.md)  
 介绍了 SQL Server 登录和身份验证，并收录了其他资源的链接。  
  
 [SQL Server 中的服务器和数据库角色](server-and-database-roles-in-sql-server.md)  
 说明固定服务器和数据库角色、自定义数据库角色和内置帐户，并提供到其他资源的链接。  
  
 [SQL Server 中的所有权和用户架构分离](ownership-and-user-schema-separation-in-sql-server.md)  
 说明对象所属权和用户架构分离，并提供到其他资源的链接。  
  
 [SQL Server 中的授权和权限](authorization-and-permissions-in-sql-server.md)  
 说明使用最低特权原则授予权限并提供到其他资源的链接。  
  
 [SQL Server 中的数据加密](data-encryption-in-sql-server.md)  
 说明 SQL Server 中的数据加密选项并提供到其他资源的链接。  
  
 [SQL Server 中的 CLR 集成安全性](clr-integration-security-in-sql-server.md)  
 提供到 CLR 集成安全资源的链接。  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 安全性](sql-server-security.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
