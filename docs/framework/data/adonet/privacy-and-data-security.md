---
title: 隐私和数据安全性
ms.date: 03/30/2017
ms.assetid: 46fa5839-adf7-4c7c-bce3-71e941fa7de9
ms.openlocfilehash: b0721cbc4957dfe64627f3a18064110f96a26f2b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177379"
---
# <a name="privacy-and-data-security"></a>隐私和数据安全性

保护和管理 ADO.NET 应用程序中的敏感信息依赖于用于创建这些信息的基础产品和技术。 ADO.NET 不直接提供用于保护或加密数据的服务。  
  
## <a name="cryptography-and-hash-codes"></a>加密和哈希代码  

 可从 ADO.NET 应用程序使用 .NET Framework <xref:System.Security.Cryptography> 命名空间中的类，以防止未经授权的第三方读取或修改数据。 某些类是非托管 Microsoft CryptoAPI 的包装，而其他一些类则是托管实现。 " [加密服务](../../../standard/security/cryptographic-services.md) " 主题提供了 .NET Framework 中的加密的概述，介绍了如何实现如何，以及如何执行特定的加密任务。  
  
 与加密（允许对数据进行加密然后解密）不同，散列数据是一个单向过程。 如果要通过检查数据是否已更改来避免数据被篡改，哈希数据非常有用：如果输入字符串相同，哈希算法总是生成相同的短输出值，很容易进行比较。 [通过哈希代码确保数据完整性](../../../standard/security/ensuring-data-integrity-with-hash-codes.md) 介绍了如何生成和验证哈希值。  
  
## <a name="encrypting-configuration-files"></a>加密配置文件  

 保护应用程序时，最重要的目标之一是保护对数据源的访问。 如果连接字符串未受保护，那么它就是一个潜在漏洞。 配置文件中保存的连接字符串存储在标准 XML 文件（.NET Framework 已为其定义了一组常用元素）中。 通过受保护的配置，您可以加密配置文件中的敏感信息。 虽然受保护的配置主要是为 ASP.NET 应用程序设计的，但它也可以用于加密 Windows 应用程序中的配置文件部分。 有关详细信息，请参阅 [保护连接信息](protecting-connection-information.md)。  
  
## <a name="securing-string-values-in-memory"></a>保护内存中的字符串值  

 如果 <xref:System.String> 对象包含敏感信息（如密码、信用卡号或个人数据），将存在信息使用完后可能被泄露的风险，因为应用程序无法从计算机内存中删除该数据。  
  
 <xref:System.String> 不可变；其值在创建后无法修改。 看起来要修改字符串值的更改实际上在内存中创建了 <xref:System.String> 对象的一个新实例，以将该数据存储为纯文本。 此外，无法预测何时将从内存中删除字符串实例。 字符串的内存回收并不是根据 .NET 垃圾回收确定的。 如果您的数据确实是敏感数据，则应避免使用 <xref:System.String> 和 <xref:System.Text.StringBuilder> 类。  
  
 <xref:System.Security.SecureString> 类提供了在内存中使用数据保护 API (DPAPI) 加密文本的方法。 这样，就可以在不再需要字符串时将其从内存中删除。 没有任何 `ToString` 方法可快速读取 <xref:System.Security.SecureString> 的内容。 您可以不使用值或通过向其传递一个指向 `SecureString` 对象的数组的指针来初始化 <xref:System.Char> 的新实例。 这样，您就可以使用类的各种方法来处理字符串。
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](securing-ado-net-applications.md)
- [SQL Server 安全性](./sql/sql-server-security.md)
- [ADO.NET 概述](ado-net-overview.md)
