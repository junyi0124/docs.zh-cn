---
title: SQL Server 中的数据加密
ms.date: 03/30/2017
ms.assetid: 83b992f7-b351-4678-b4b9-f4ffd58134cc
ms.openlocfilehash: d0bda11f1a2933d096aa91d2be79d3af35172284
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169526"
---
# <a name="data-encryption-in-sql-server"></a>SQL Server 中的数据加密

SQL Server 提供了使用证书、非对称密钥或对称密钥对数据进行加密和解密的功能。 它在一个内部证书存储中管理所有这些证书或密钥。 该存储使用加密层次结构，可在层次结构中的上一层级别保护证书和密钥。 SQL Server 的此功能区域称为“机密存储”。  
  
 加密功能支持的最快加密模式是对称密钥加密。 此模式适用于处理大量数据。 可以通过证书、密码或其他对称密钥加密对称密钥。  
  
## <a name="keys-and-algorithms"></a>密钥和算法  

 SQL Server 支持多种对称密钥加密算法，包括 DES、Triple DES、RC2、RC4、128 位 RC4、DESX、128 位 AES、192 位 AES 和 256 位 AES。 这些算法使用 Windows 加密 API 实现。  
  
 在数据库连接范围内，SQL Server 可以保留多个开放式对称密钥。 开放式密钥从存储中进行检索，并可用于解密数据。 解密一段数据时，不需要指定要使用的对称密钥。 每个加密的值均包含用于加密该值的密钥的密钥标识符（密钥 GUID）。 如果已解密正确的密钥并且该密钥已开放，则引擎会将加密的字节流匹配到开放式对称密钥。 然后使用此密钥执行解密并返回数据。 如果正确的密钥未开放，则返回 NULL。  
  
 有关演示如何处理数据库中的加密数据的示例，请参阅 [对数据列进行加密](/sql/relational-databases/security/encryption/encrypt-a-column-of-data)。
  
## <a name="external-resources"></a>外部资源  

 有关数据加密的更多信息，请参见以下资源。  
  
|资源|说明|  
|-|-|  
|[SQL Server 加密](/sql/relational-databases/security/encryption/sql-server-encryption)|概述了 SQL Server 中的加密。 本主题包括指向其他文章的链接。|  
|[加密层次结构](/sql/relational-databases/security/encryption/encryption-hierarchy)|概述了 SQL Server 中的加密。 本主题提供指向其他文章的链接。|  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [SQL Server 中的身份验证](authentication-in-sql-server.md)
- [SQL Server 中的服务器和数据库角色](server-and-database-roles-in-sql-server.md)
- [SQL Server 中的所有权和用户架构分离](ownership-and-user-schema-separation-in-sql-server.md)
- [SQL Server 中的授权和权限](authorization-and-permissions-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
