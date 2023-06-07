---
title: WCF 和国际化域名
ms.date: 03/30/2017
ms.assetid: c8a3e10a-8bc2-4a78-8d86-a562ba6e65fa
ms.openlocfilehash: 2d93bbb0c284c2227a4d03acf1ad9a801df57bd8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281984"
---
# <a name="wcf-and-internationalized-domain-names"></a>WCF 和国际化域名

添加了允许使用带有国际化域名 (IDN) 的 WCF 服务的支持。 国际化域名称是包含非 ASCII 字符的域名。 这种支持包括能够承载具有 IDN 名称的 WCF 服务以及 WCF 客户端，以便与具有 IDN 名称的 Web 服务进行对话。  
  
## <a name="systemuri-and-idn"></a>System.Uri 和 IDN  

 <xref:System.Uri> 具有两个属性：<xref:System.Uri.Host%2A> 和 <xref:System.Uri.DnsSafeHost%2A>。 根据 IDN 配置设置，这些属性包含 Unicode 或 Punycode 值。  
  
 使用下面的 XML 在应用程序的配置文件中启用 IDN  
  
```xml  
<configuration>  
  <uri>  
    <idn enabled="All/AllExceptIntranet/None" />  
  </uri>  
</configuration>  
```  
  
 \<idn>元素包含可设置为下列值之一的已启用属性：  
  
1. "None"  
  
2. "AllExceptIntranet"  
  
3. “全部”  
  
 当 IDN 设置设置为 "None" 时，Uri.dnssafehost 或 Uri 不会执行任何转换。 当 IDN 设置设置为 "All" 时，uri。主机保持 Unicode 和 uri。Uri.dnssafehost 转换为 Punycode。 当 IDN 设置设为 "AllExceptIntranet" 时，uri。对于 internet 地址，Uri.dnssafehost 将转换为 Punycode，并为 intranet 地址保留 Unicode。 此设置对于正确的 DNS 名称解析十分重要。 请注意，无需为 Windows 8 和更新版本配置此设置。  
  
> [!WARNING]
> 您应该永远不会使用 Punycode 对地址进行硬代码。 WCF 将会基于您应用的配置设置为您转换它。  
  
> [!WARNING]
> 将 Unicode 字符添加到 applicationHost.exe.config 时，使用 UTF-8 编码保存文件。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Uri?displayProperty=nameWithType>
