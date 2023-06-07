---
title: System.Uri 中的国际资源标识符支持
ms.date: 03/30/2017
ms.assetid: b5e994c3-3535-4aff-8e1b-b69be22e9a22
ms.openlocfilehash: a3670c40a7a78e2ac8b521a4cb95477381848f36
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253357"
---
# <a name="international-resource-identifier-support-in-systemuri"></a>System.Uri 中的国际资源标识符支持

<xref:System.Uri?displayProperty=nameWithType> 类在国际资源标识符 (IRI) 和国际化域名 (IDN) 的支持下已得到扩展。 NET Framework 3.5、3.0 SP1 和 2.0 SP1 提供了这些增强功能。  
  
## <a name="iri-and-idn-support"></a>IRI 和 IDN 支持  

 Web 地址通常使用由一组非常有限的字符组成的统一资源标识符 (URI) 来表示：  
  
- 英文字母表中的大小写 ASCII 字母。  
  
- 从 0 到 9 的数字。  
  
- 少量的其他 ASCII 符号。  
  
 Internet 工程任务组发布 (IETF) 的 RFC 2396 和 RFC 3986 中记录了 URL 的规格。  
  
 随着 Internet 的发展，越来越需要使用英语以外的语言识别资源。 标识符满足了这种需求，并且使得非 ASCII 字符（Unicode 中的字符/ISO 10646 字符集）被称为国际资源标识符（IRI）。 IETF 发布的 RFC 3987 记录了 IRI 的规格。 使用 IRI 允许 URL 包含 Unicode 字符。  
  
 现有的 <xref:System.Uri?displayProperty=nameWithType> 类已扩展为在 RFC 3987 基础上为 IRI 提供支持。 除非当前用户专门启用 IRI，否则他们看不到任何 NET Framework 2.0 行为的改变。 这确保了 NET Framework 以前版本的应用程序兼容性。  
  
 应用程序可指定是否使用应用于域名的国际化域名（IDN）分析，以及是否应该应用 IRI 分析规则。 这可以在 machine.config 或应用配置文件中完成。  
  
 启用 IDN 可以将域名中所有 Unicode 标签转换成标签的 Punycode 等同项。 Punycode 名称只包含 ASCII 字符，并且始终以 xn-- 前缀开头。 这样是为了支持 Internet 上的 DNS 服务器，因为大部分 DNS 服务器仅支持 ASCII 字符（参见 RFC 3940）。  
  
 启用 IRI 和 IDN 会影响 <xref:System.Uri.DnsSafeHost%2A?displayProperty=nameWithType> 属性的值。 启用 IRI 和 IDN 还可能更改 <xref:System.Uri.Equals%2A?displayProperty=nameWithType>、<xref:System.Uri.OriginalString%2A?displayProperty=nameWithType>、<xref:System.Uri.GetComponents%2A?displayProperty=nameWithType> 和 <xref:System.Uri.IsWellFormedOriginalString%2A> 方法的行为。  
  
 <xref:System.GenericUriParser?displayProperty=nameWithType> 类已得到扩展，允许创建支持 IRI 和 IDN 的自定义分析。 通过将枚举中可用值的按位组合传递给 <xref:System.GenericUriParserOptions?displayProperty=nameWithType> 构造函数 <xref:System.GenericUriParser?displayProperty=nameWithType> 来指定 <xref:System.GenericUriParser?displayProperty=nameWithType> 对象的行为。 <xref:System.GenericUriParserOptions.IriParsing?displayProperty=nameWithType> 类型表示分析程序支持 RFC 3987 中为国际资源标识符 (IRI) 指定的分析规则。 是否实际使用 IRI 取决于是否启用 IRI。  
  
 <xref:System.GenericUriParserOptions.Idn?displayProperty=nameWithType> 类型表示分析程序支持主机名的国际化域名 (IDN) 分析。 是否实际使用 IDN 取决于是否启用 IDN。  
  
 启用 IRI 分析后将根据 RFC 3987 中最后的 IRI 规则执行规范化和字符检查。 默认值用于禁用 IRI 分析，所以规范化和字符检查会根据 RFC 2396 和 RFC 3986 完成。  
  
 <xref:System.Uri?displayProperty=nameWithType> 类中的 IRI 和 IDN 处理也可以通过使用 <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> 和 <xref:System.Configuration.IdnElement?displayProperty=nameWithType> 配置集类进行控制。 <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> 设置启用或禁用 <xref:System.Uri?displayProperty=nameWithType> 类中的 IRI 处理。 <xref:System.Configuration.IdnElement?displayProperty=nameWithType> 设置启用或禁用 <xref:System.Uri> 类中的 IDN 处理。 <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> 也可间接控制 IDN。 必须启用 IRI 处理才能进行 IDN 处理。 如果禁用 IRI，IDN 处理将被设置为默认值，这时 NET Framework 2.0 行为用于兼容性，并且 IDN 名称不可用。  
  
 构造第一个 <xref:System.Uri?displayProperty=nameWithType> 类后，将立即读取 <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType> 和 <xref:System.Configuration.IdnElement?displayProperty=nameWithType> 配置类的配置设置。 忽略时间后更改为默认设置。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Configuration.IdnElement?displayProperty=nameWithType>
- <xref:System.Configuration.IriParsingElement?displayProperty=nameWithType>
- <xref:System.Uri?displayProperty=nameWithType>
- <xref:System.Uri.DnsSafeHost%2A?displayProperty=nameWithType>
