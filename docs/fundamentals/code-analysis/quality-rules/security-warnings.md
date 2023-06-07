---
title: '安全规则 (代码分析) '
description: 了解代码分析安全规则。
ms.date: 10/02/2019
ms.topic: reference
f1_keywords:
- vs.codeanalysis.securityrules
helpviewer_keywords:
- security [Visual Studio ALM], Enterprise Templates
- security rules
- managed code analysis rules, security rules
- rules, security
author: gewarren
ms.author: gewarren
ms.openlocfilehash: e907905b065d786fc8b3c370fb2d2e2b19e62a2b
ms.sourcegitcommit: 5114e7847e0ff8ddb8c266802d47af78567949cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2020
ms.locfileid: "96591071"
---
# <a name="security-rules"></a>安全规则

安全规则支持更安全的库和应用程序。 这些规则可帮助防止程序中出现安全漏洞。 如果您禁用这些规则中的任何一种，您应该清楚地标记代码中的原因，并向您的开发项目通知指定的安全官员。

## <a name="in-this-section"></a>在本节中

|规则|描述|
|----------|-----------------|
|[CA2100:检查 SQL 查询是否存在安全漏洞](ca2100.md)|一个方法使用按该方法的字符串参数生成的字符串设置 System.Data.IDbCommand.CommandText 属性。 此规则假定字符串参数中包含用户输入。 基于用户输入生成的 SQL 命令字符串易于受到 SQL 注入式攻击。|
|[CA2109:检查可见的事件处理程序](ca2109.md)|检测到公共事件处理方法或受保护事件处理方法。 除非绝对必要，否则不应公开事件处理方法。|
|[CA2119:密封满足私有接口的方法](ca2119.md)|可继承的公共类型为 internal（在 Visual Basic 中为 Friend）接口提供可重写的方法实现。 若要修复与此规则的冲突，请禁止方法在程序集外重写。|
|[CA2153：避免处理损坏状态异常](ca2153.md)|[损坏状态异常 (CSE)](/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions) 指示进程中存在内存损坏。 如果攻击者可以将攻击放置到损坏的内存区域，则捕获它们（而非允许进程崩溃）可能导致安全漏洞。|
|[CA2300：请勿使用不安全的反序列化程序 BinaryFormatte](ca2300.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2301：在未先设置 BinaryFormatter.Binder 的情况下，请不要调用 BinaryFormatter.Deserialize](ca2301.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2302：在调用 BinaryFormatter.Deserialize 之前，确保设置 BinaryFormatter.Binder](ca2302.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2305：请勿使用不安全的反序列化程序 LosFormatter](ca2305.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2310：请勿使用不安全的反序列化程序 NetDataContractSerializer](ca2310.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2311：在未先设置 NetDataContractSerializer.Binder 的情况下，请不要反序列化](ca2311.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2312：确保在反序列化之前设置 NetDataContractSerializer.Binder](ca2312.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2315：请勿使用不安全的反序列化程序 ObjectStateFormatter](ca2315.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2321：请勿使用 SimpleTypeResolver 对 JavaScriptSerializer 进行反序列化](ca2321.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2322：确保在反序列化之前没有使用 SimpleTypeResolver 初始化 JavaScriptSerializer](ca2322.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2326：请勿使用 None 以外的 TypeNameHandling 值](ca2326.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2327：不要使用不安全的 JsonSerializerSettings](ca2327.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2328：确保 JsonSerializerSettings 是安全的](ca2328.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2329：不要使用不安全的配置反序列化 JsonSerializer](ca2329.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2330：在反序列化时确保 JsonSerializer 具有安全配置](ca2330.md)|反序列化不受信任的数据时，不安全的反会很容易 攻击者可能会修改序列化的数据，使其包含意外类型，以注入具有恶意副作用的对象。|
|[CA2350:确保 DataTable.ReadXml() 的输入受信任](ca2350.md)|<xref:System.Data.DataTable>使用不受信任的输入反序列化时，攻击者可以创建恶意输入来执行拒绝服务攻击。 可能存在未知的远程代码执行漏洞。|
|[CA2351:确保 DataSet.ReadXml() 的输入受信任](ca2351.md)|<xref:System.Data.DataSet>使用不受信任的输入反序列化时，攻击者可以创建恶意输入来执行拒绝服务攻击。 可能存在未知的远程代码执行漏洞。|
|[CA2352:可序列化类型中的不安全 DataSet 或 DataTable 容易受到远程代码执行攻击](ca2352.md)|标记为的类或结构 <xref:System.SerializableAttribute> 包含 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 字段或属性，但不具有 <xref:System.CodeDom.Compiler.GeneratedCodeAttribute> 。|
|[CA2353:可序列化类型中的不安全 DataSet 或 DataTable](ca2353.md)|用 XML 序列化特性或数据协定特性标记的类或结构包含 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 字段或属性。|
|[CA2354:反序列化对象图中的不安全 DataSet 或 DataTable 可能容易受到远程代码执行攻击](ca2354.md)|使用序列化进行反序列化 <xref:System.Runtime.Serialization.IFormatter?displayProperty=nameWithType> ，强制转换类型的对象图可以包括 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 。|
|[CA2355:反序列化对象图中的不安全 DataSet 或 DataTable](ca2355.md)|当强制转换或指定类型的对象图可以包含或时进行反序列化 <xref:System.Data.DataSet> <xref:System.Data.DataTable> 。|
|[CA2356： web 反序列化对象图中的不安全数据集或 DataTable](ca2356.md)|具有或的方法 <xref:System.Web.Services.WebMethodAttribute?displayProperty=nameWithType> <xref:System.ServiceModel.OperationContractAttribute?displayProperty=nameWithType> 具有可引用或的参数 <xref:System.Data.DataSet> <xref:System.Data.DataTable> 。|
|[CA2361：请确保包含 DataSet.ReadXml() 的自动生成的类没有与不受信任的数据一起使用](ca2361.md)|<xref:System.Data.DataSet>使用不受信任的输入反序列化时，攻击者可以创建恶意输入来执行拒绝服务攻击。 可能存在未知的远程代码执行漏洞。|
|[CA2362：自动生成的可序列化类型中不安全的数据集或数据表易受远程代码执行攻击](ca2362.md)|当反序列化的不受信任的输入 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 并且反序列化的对象图包含 <xref:System.Data.DataSet> 或时 <xref:System.Data.DataTable> ，攻击者可以创建恶意有效负载来执行远程代码执行攻击。|
|[CA3001：查看 SQL 注入漏洞的代码](ca3001.md)|使用不受信任的输入和 SQL 命令时，请注意 SQL 注入攻击。 SQL 注入攻击可以执行恶意的 SQL 命令，从而降低应用程序的安全性和完整性。|
|[CA3002：查看 XSS 漏洞的代码](ca3002.md)|处理 web 请求中的不受信任输入时，请注意跨站点脚本 (XSS) 攻击。 XSS 攻击会将不受信任的输入注入原始 HTML 输出，使攻击者可以执行恶意脚本或恶意修改网页中的内容。|
|[CA3003:查看文件路径注入漏洞的代码](ca3003.md)|当处理 web 请求中的不受信任输入时，请注意在指定文件路径时使用用户控制输入。|
|[CA3004：查看信息泄露漏洞的代码](ca3004.md)|泄露异常信息可让攻击者深入了解应用程序的内部机制，从而帮助攻击者找到其他漏洞来利用这些漏洞。|
|[CA3006：查看进程命令注入漏洞的代码](ca3006.md)|使用不受信任的输入时，请注意命令注入攻击。 命令注入攻击可以在基础操作系统上执行恶意命令，从而危及服务器的安全和完整性。|
|[CA3007：查看公开重定向漏洞的代码](ca3007.md)|使用不受信任的输入时，请注意开放重定向漏洞。 攻击者可以利用开放的重定向漏洞来使用您的网站来获得合法 URL 的外观，但将不受信任的访问者重定向到仿冒网站或其他恶意网页。|
|[CA3008：查看 XPath 注入漏洞的代码](ca3008.md)|使用不受信任的输入时，请注意 XPath 注入式攻击。 使用不受信任输入构造 XPath 查询时，攻击者可能会恶意地操作查询以返回意外的结果，并可能会泄漏所查询的 XML 的内容。|
|[CA3009：查看 XML 注入漏洞的代码](ca3009.md)|使用不受信任的输入时，请注意 XML 注入攻击。|
|[CA3010：查看 XAML 注入漏洞的代码](ca3010.md)|使用不受信任的输入时，请注意 XAML 注入攻击。 XAML 是一种直接表示对象实例化和执行的标记语言。 这意味着在 XAML 中创建的元素可以与系统资源交互 (例如，网络访问和文件系统 IO) 。|
|[CA3011：查看 DLL 注入漏洞的代码](ca3011.md)|使用不受信任的输入时，请注意加载不受信任的代码。 如果你的 web 应用程序加载不受信任的代码，攻击者可能能够将恶意 Dll 注入到你的进程中并执行恶意代码。|
|[CA3012：查看正则表达式注入漏洞的代码](ca3012.md)|使用不受信任的输入时，请注意 regex 注入式攻击。 攻击者可以使用 regex 注入来恶意地修改正则表达式，以使 regex 与意外结果匹配，或使 regex 消耗过多的 CPU，导致拒绝服务攻击。|
|[CA3061：请勿按 URL 添加架构](ca3061.md)|不要使用 Add 方法的 unsafe 重载，因为这可能会导致危险的外部引用。|
|[CA3075:不安全的 DTD 处理](ca3075.md)|如果使用不安全的 DTDProcessing 实例或引用外部实体源，分析器可能会接受不受信任的输入并将敏感信息泄露给攻击者。|
|[CA3076:不安全的 XSLT 脚本执行](ca3076.md)|如果 (XSLT) 在 .NET 应用程序不安全地中执行可扩展样式表语言转换，则处理器可能会解析可能向攻击者泄露敏感信息的不受信任的 URI 引用，从而导致拒绝服务和跨站点攻击。|
|[CA3077:API 设计、XML 文档和 XML 文本读取器中的不安全处理](ca3077.md)|当设计派生自 XMLDocument 和 XMLTextReader 的 API 时，请注意 DtdProcessing。 当引用或解析外部实体源或设置 XML 中的不安全值时，使用不安全的 DTDProcessing 实例可能会导致信息泄露。|
|[CA3147:使用 ValidateAntiForgeryToken 标记谓词处理程序](ca3147.md)|设计 ASP.NET MVC 控制器时，请注意跨站点请求伪造攻击。 跨站点请求伪造攻击可以将经过身份验证的用户的恶意请求发送到 ASP.NET MVC 控制器。|
|[CA5350:请勿使用弱加密算法](ca5350.md)|出于多种原因，现今使用弱加密算法和哈希函数，但不应使用它们来保证保密性或它们所保护的数据的完整性。 当此规则在代码中找到 TripleDES、SHA1、或 RIPEMD160 算法时，此规则将触发。|
|[CA5351：不要使用损坏的加密算法](ca5351.md)|损坏的加密算法不安全，强烈建议不要使用。 当此规则在代码中找到 MD5 哈希算法，或者 DES 或 RC2 加密算法时，此规则将触发。|
|[CA5358：请勿使用不安全的密码模式](ca5358.md)|请勿使用不安全的密码模式|
|[CA5359:请勿禁用证书验证](ca5359.md)|证书可以帮助验证服务器的身份。 客户端应验证服务器证书，以确保将请求发送到目标服务器。 如果 Servicepointmanager.servercertificatevalidationcallback 始终返回 `true` ，则任何证书将通过验证。|
|[CA5360:在反序列化中不要调用危险的方法](ca5360.md)|不受信任的反序列化是指使用不受信任的数据来滥用应用程序的逻辑，导致拒绝服务 (DoS) 攻击，甚至是在反序列化时执行任意代码时出现的漏洞。 当应用程序对其控制的不受信任的数据进行反序列化时，通常可能会有恶意用户滥用这些反序列化功能。 具体而言，就是在反序列化过程中调用危险方法。 成功的反反序列化攻击可能会允许攻击者发起攻击，如 DoS 攻击、身份验证绕过和远程代码执行。|
|[CA5361：不禁止 SChannel 使用强加密](ca5361.md)|设置 `Switch.System.Net.DontEnableSchUseStrongCrypto` 为 `true` 受损传出传输层安全性 (TLS) 连接中使用的加密。 较弱的加密可能会危及应用程序与服务器之间通信的机密性，使攻击者更容易窃听敏感数据。|
|[CA5362:反序列化对象图中存在潜在引用循环](ca5362.md)|如果反序列化不受信任的数据，则处理反序列化对象图的任何代码都需要处理引用循环，而不会进入无限循环。 这包括作为反序列化回调一部分的代码和在反序列化完成后处理对象图的代码。 否则，攻击者可能会对包含引用周期的恶意数据执行拒绝服务攻击。|
|[CA5363：请勿禁用请求验证](ca5363.md)|请求验证是 ASP.NET 中的一项功能，用于检查 HTTP 请求并确定这些请求是否包含可能导致注入攻击（包括跨站点脚本）的潜在危险内容。|
|[CA5364：不使用已弃用的安全协议](ca5364.md)|传输层安全 (TLS) 保护计算机之间的通信，最常见的是通过超文本传输协议安全 (HTTPS) 。 较早的 TLS 协议版本不如 TLS 1.2 和 TLS 1.3 安全，更有可能出现新的漏洞。 避免旧协议版本来最大程度地降低风险。|
|[CA5365:请勿禁用 HTTP 头检查](ca5365.md)|HTTP 标头检查启用在响应标头中找到的回车符和换行符（\r 和 \n）的编码。 这种编码有助于避免注入攻击，攻击者利用该应用程序回显了标头中包含的不受信任的数据。|
|[CA5366:将 XmlReader 用于数据集读取 XML](ca5366.md)|使用 <xref:System.Data.DataSet> 读取包含不受信任数据的 XML 可能会加载危险的外部引用，应使用 <xref:System.Xml.XmlReader> 具有安全解析程序或禁用 DTD 处理的进行限制。|
|[CA5367:请勿序列化具有 Pointer 字段的类型](ca5367.md)|此规则检查是否存在具有指针字段或属性的可序列化类。 无法序列化的成员可以是指针，如使用标记的静态成员或字段 <xref:System.NonSerializedAttribute> 。|
|[CA5368:针对派生自 Page 的类设置 ViewStateUserKey](ca5368.md)|设置 <xref:System.Web.UI.Page.ViewStateUserKey> 属性可帮助您阻止对应用程序的攻击，方法是允许您为个别用户分配视图状态变量的标识符，使攻击者无法使用该变量生成攻击。 否则，会出现跨站点请求伪造的漏洞。|
|[CA5369：将 XmlReader 用于反序列化](ca5369.md)|处理不受信任的 DTD 和 XML 架构可能会启用加载危险的外部引用，这应通过使用具有安全解析程序的 XmlReader 或禁用 DTD 和 XML 内联架构处理来限制。|
|[CA5370：将 XmlReader 用于验证读取器](ca5370.md)|处理不受信任的 DTD 和 XML 架构可能会启用加载危险的外部引用。 可以通过将 XmlReader 与安全解析程序结合使用，或者禁用 DTD 和 XML 内联架构处理来限制这种危险加载。|
|[CA5371：将 XmlReader 用于架构读取](ca5371.md)|处理不受信任的 DTD 和 XML 架构可能会启用加载危险的外部引用。 将 XmlReader 用于安全解析程序或 DTD 和 XML 内联架构处理将会限制这种情况。|
|[CA5372：将 XmlReader 用于 XPathDocument](ca5372.md)|处理来自不受信任数据的 XML 可能会加载危险的外部引用，这可以通过使用具有安全解析程序的 XmlReader 或禁用 DTD 处理来限制。|
|[CA5373：请勿使用已过时的密钥派生功能](ca5373.md)|此规则检测弱密钥派生方法和的调用 <xref:System.Security.Cryptography.PasswordDeriveBytes?displayProperty=fullName> `Rfc2898DeriveBytes.CryptDeriveKey` 。 <xref:System.Security.Cryptography.PasswordDeriveBytes?displayProperty=fullName> 使用弱算法 PBKDF1。|
|[CA5374:请勿使用 XslTransform](ca5374.md)|此规则检查 <xref:System.Xml.Xsl.XslTransform?displayProperty=nameWithType> 是否在代码中实例化。 <xref:System.Xml.Xsl.XslTransform?displayProperty=nameWithType> 现已过时，不应使用。|
|[CA5375:请勿使用帐户共享访问签名](ca5375.md)|帐户 SAS 可委派对 blob 容器、表、队列以及服务 SAS 不允许的文件共享上的读取、写入和删除操作的访问权限。 但是，它不支持容器级别的策略，并且具有较低的灵活性和对所授予权限的控制权限。 一旦恶意用户获得，您的存储帐户就会容易泄露。|
|[CA5376:使用 SharedAccessProtocol HttpsOnly](ca5376.md)|SAS 是不能以纯文本形式在 HTTP 上传输的敏感数据。|
|[CA5377:使用容器级别访问策略](ca5377.md)|容器级别的访问策略可以随时修改或撤消。 它提供了更大的灵活性并控制授予的权限。|
|[CA5378：不禁用 ServicePointManagerSecurityProtocols](ca5378.md)|设置 `Switch.System.ServiceModel.DisableUsingServicePointManagerSecurityProtocols` 以 `true` 限制 Windows Communication FRAMEWORK (WCF) 传输层安全 (tls) 与使用 tls 1.0 的连接。 TLS 版本将不推荐使用。|
|[CA5379：确保密钥派生函数算法足够强](ca5379.md)|<xref:System.Security.Cryptography.Rfc2898DeriveBytes>类默认使用 <xref:System.Security.Cryptography.HashAlgorithmName.SHA1> 算法。 应指定要在具有 <xref:System.Security.Cryptography.HashAlgorithmName.SHA256> 或更高版本的构造函数的某些重载中使用的哈希算法。 请注意， <xref:System.Security.Cryptography.Rfc2898DeriveBytes.HashAlgorithm> 属性仅具有 `get` 访问器，没有 `overriden` 修饰符。|
|[CA5380：请勿将证书添加到根存储中](ca5380.md)|此规则检测将证书添加到 "受信任的根证书颁发机构" 证书存储中的代码。 默认情况下，"受信任的根证书颁发机构" 证书存储区配置为满足 Microsoft 根证书计划要求的一组公共 Ca。|
|[CA5381：请确保证书未添加到根存储中](ca5381.md)|此规则检测可能会将证书添加到 "受信任的根证书颁发机构" 证书存储中的代码。 默认情况下，"受信任的根证书颁发机构" 证书存储区配置了一组公共证书颁发机构 (Ca) 满足 Microsoft 根证书计划的要求。|
|[CA5382:在 ASP.NET Core 中使用安全 Cookie](ca5382.md)|通过 HTTPS 提供的应用程序必须使用安全 cookie，这向浏览器指示，cookie 只应使用传输层安全性 (TLS) 传输。|
|[CA5383:确保在 ASP.NET Core 中使用安全 Cookie](ca5383.md)|通过 HTTPS 提供的应用程序必须使用安全 cookie，这向浏览器指示，cookie 只应使用传输层安全性 (TLS) 传输。|
|[CA5384:不使用数字签名算法(DSA)](ca5384.md)|DSA 是弱非对称加密算法。|
|[CA5385:设置具有足够密钥大小的 Rivest–Shamir–Adleman (RSA)算法](ca5385.md)|小于2048位的 RSA 密钥更容易受到暴力破解攻击。|
|[CA5386：避免对 SecurityProtocolType 值进行硬编码](ca5386.md)|传输层安全 (TLS) 保护计算机之间的通信，最常见的是通过超文本传输协议安全 (HTTPS) 。 协议 1.1 1.0 版本不推荐使用，而 TLS 1.2 和 TLS 1.3 是最新的。 未来，TLS 1.2 和 TLS 1.3 可能已弃用。 若要确保应用程序的安全性，请避免硬编码协议版本，并以至少 .NET Framework v 4.7.1 为目标。|
|[CA5387:请勿使用迭代计数不足的弱密钥派生功能](ca5387.md)|此规则检查是否生成了的加密密钥 <xref:System.Security.Cryptography.Rfc2898DeriveBytes> ，迭代次数小于100000。 较高的迭代次数可帮助缓解字典攻击，尝试猜测生成的加密密钥。|
|[CA5388:使用弱密钥派生功能时，请确保迭代计数足够大](ca5388.md)|此规则检查是否生成的加密密钥 <xref:System.Security.Cryptography.Rfc2898DeriveBytes> 带有小于100000的迭代次数。 较高的迭代次数可帮助缓解字典攻击，尝试猜测生成的加密密钥。|
|[CA5389：请勿将存档项的路径添加到目标文件系统路径中](ca5389.md)|文件路径可以是相对路径，并且可能会导致文件系统在预期的文件系统目标路径外进行访问，从而导致恶意配置更改并通过 "等待" 等方法执行远程代码。|
|[CA5390:请勿编码加密密钥](ca5390.md)|若要成功使用对称算法，密钥必须仅对发送方和接收方是已知的。 当某个密钥是硬编码的，很容易发现它。 即使已编译的二进制文件，恶意用户也可以轻松地将其提取出来。 私钥泄露后，可以直接解密密码文本，而不会再对其进行保护。|
|[CA5391:在 ASP.NET Core MVC 控制器中使用防伪造令牌](ca5391.md)|处理 `POST` 、、 `PUT` `PATCH` 或 `DELETE` 请求而不验证防伪令牌可能易受到跨站点请求伪造攻击。 跨站点请求伪造攻击可以将经过身份验证的用户的恶意请求发送到 ASP.NET Core MVC 控制器。|
|[CA5392:对 P/Invoke 使用 DefaultDllImportSearchPaths 属性](ca5392.md)|默认情况下，使用探测的 P/Invoke 函数包含 <xref:System.Runtime.InteropServices.DllImportAttribute> 多个目录，其中包含要加载的库的当前工作目录。 对于某些应用程序，这可能是一个安全问题，导致 DLL 劫持。|
|[CA5393:请勿使用不安全的 DllImportSearchPath 值](ca5393.md)|默认 DLL 搜索目录和程序集目录中可能存在恶意 DLL。 或者，根据应用程序的运行位置，应用程序目录中可能存在恶意的 DLL。|
|[CA5394:请勿使用不安全的随机性](ca5394.md)|使用加密弱伪随机数生成器可以允许攻击者预测将生成的安全敏感值。|
|[CA5395:缺少操作方法的 HttpVerb 属性](ca5395.md)|用于创建、编辑、删除或以其他方式修改数据的所有操作方法都需要通过跨站点请求伪造攻击的防伪属性进行保护。 执行 GET 操作应该是不会产生副作用的安全操作，并且不会修改您的持久数据。|
|[CA5396:将 HttpCookie 的 HttpOnly 设置为 true](ca5396.md)|作为深层防御措施，请确保将安全敏感的 HTTP cookie 标记为 HttpOnly。 这表明 web 浏览器应禁止脚本访问 cookie。 注入的恶意脚本是偷窃 cookie 的常见方法。|
|[CA5397：不使用已弃用的 SslProtocols 值](ca5397.md)|传输层安全 (TLS) 保护计算机之间的通信，最常见的是通过超文本传输协议安全 (HTTPS) 。 较早的 TLS 协议版本不如 TLS 1.2 和 TLS 1.3 安全，更有可能出现新的漏洞。 避免旧协议版本来最大程度地降低风险。|
|[CA5398：避免硬编码的 SslProtocols 值](ca5398.md)|传输层安全 (TLS) 保护计算机之间的通信，最常见的是通过超文本传输协议安全 (HTTPS) 。 协议 1.1 1.0 版本不推荐使用，而 TLS 1.2 和 TLS 1.3 是最新的。 未来，TLS 1.2 和 TLS 1.3 可能已弃用。 若要确保应用程序的安全性，请避免硬编码协议版本。|
|[CA5399:绝对禁用 HttpClient 证书吊销列表检查](ca5399.md)|吊销的证书不再受信任。 攻击者可以使用它来传递某些恶意数据或偷窃 HTTPS 通信中的敏感数据。|
|[CA5400:确保未禁用 HttpClient 证书吊销列表检查](ca5400.md)|吊销的证书不再受信任。 攻击者可以使用它来传递某些恶意数据或偷窃 HTTPS 通信中的敏感数据。|
|[CA5401:不要将 CreateEncryptor 与非默认 IV 结合使用](ca5401.md)|对称加密应始终使用不可重复的初始化向量来防止字典攻击。|
|[CA5402:将 CreateEncryptor 与默认 IV 结合使用](ca5402.md)|对称加密应始终使用不可重复的初始化向量来防止字典攻击。|
|[CA5403：请勿硬编码证书](ca5403.md)|`data` `rawData` <xref:System.Security.Cryptography.X509Certificates.X509Certificate> 或构造函数的或参数 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 是硬编码的。|
