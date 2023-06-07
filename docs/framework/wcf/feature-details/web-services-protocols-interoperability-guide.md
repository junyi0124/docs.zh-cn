---
title: Web 服务协议互操作性指南
ms.date: 03/30/2017
ms.assetid: f2981678-ebdb-433d-899b-467f7df95fb2
ms.openlocfilehash: f35ca629da65af749897d28d28808d06eced7aa8
ms.sourcegitcommit: fe8877e564deb68d77fa4b79f55584ac8d7e8997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90720109"
---
# <a name="web-services-protocols-interoperability-guide"></a>Web 服务协议互操作性指南

Windows Communication Foundation (WCF) 实现了许多 Web 服务协议。 这些协议中有许多都包含大量留给实施者来决定的选项和扩展点。 本文提供 WCF 所实现的 Web 服务协议的列表。 本部分中的其他文章提供每个支持的协议的实现详细信息。

## <a name="web-services-protocols-implemented-by-wcf"></a>WCF 实现的 Web 服务协议

WCF 通过协定功能，通过通道和 Web 服务应用程序协议为 Web 服务 (WS) 基础结构协议提供支持。 通过 XML 架构描述语言 1.0 (XSD) 和 Web 服务描述语言 (WSDL) 1.1 完成应用程序协议的互操作性。

基础结构协议互操作性由 WS-* 规范提供。 WCF 通道提供对许多 WS \* 基础结构协议的支持。 WCF 信道使用绑定元素进行配置。 下表包含 \* 各种 WCF 绑定元素实现的 WS 基础结构协议的完整列表。

<xref:System.ServiceModel.Channels.HttpTransportBindingElement> 支持下表中的规范。

|规范/文档|链接|
|-----------------------------|----------|
|HTTP 1.1|[RFC 2616](https://www.rfc-editor.org/rfc/rfc2616.txt)|
|SOAP 1.1 HTTP 绑定|[简单的对象访问协议 (SOAP) 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)，第7部分|
|SOAP 1.2 HTTP 绑定|[SOAP 版本1.2 第2部分：附属 (Second 版) ，第 ](https://www.w3.org/TR/soap12-part2/)7 部分|

<xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> 和 <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> 支持下表中的规范。

|规范/文档|链接|
|-----------------------------|----------|
|XML|[Extensible Markup Language (XML) 1.0 (Fourth Edition)（可扩展标记语言 (XML) 1.0（第四版））](https://www.w3.org/TR/REC-xml/)|
|SOAP 1.1|[Simple Object Access Protocol (SOAP) 1.1（简单对象访问协议 (SOAP) 1.1）](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)|
|SOAP 1.2 核心|[SOAP Version 1.2 Part 1: Messaging Framework (Second Edition)（SOAP 版本 1.2 第 1 部分：消息传递框架（第二版））](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)|
|WS-Addressing 2004/08|[Web Services Addressing (WS-Addressing)（Web 服务寻址 (WS-Addressing)）](https://www.w3.org/Submission/2004/SUBM-ws-addressing-20040810/)|
|W3C Web 服务寻址 1.0 - 核心|[Web 服务寻址 1.0 – 核心（可能为英文网页）](https://www.w3.org/TR/2006/REC-ws-addr-core-20060509/)|
|W3C Web 服务寻址 1.0 - SOAP 绑定|[Web Services Addressing 1.0 - SOAP Binding（Web 服务寻址 1.0 - SOAP 绑定）](https://www.w3.org/TR/2006/REC-ws-addr-soap-20060509/)|
|W3C Web 服务寻址 1.0 - WSDL 绑定*|[Web 服务寻址 1.0 - WSDL 绑定](https://www.w3.org/TR/2006/CR-ws-addr-wsdl-20060529/)|
|W3C Web 服务寻址 1.0 元数据|[Web 服务寻址 1.0 - 元数据](https://www.w3.org/TR/ws-addr-metadata/)|
|WSDL SOAP1.1 绑定|[Web 服务描述语言 (WSDL) 1.1](https://www.w3.org/TR/wsdl/)|
|WSDL SOAP1.2 绑定|[WSDL 1.1 Binding Extension for SOAP 1.2（用于 SOAP 1.2 的 WSDL 1.1 绑定扩展）](https://www.w3.org/Submission/wsdl11soap12/)|

<xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement> 支持下表中的规范。

|规范/文档|链接|
|-----------------------------|----------|
|XOP|[XML-binary Optimized Packaging（XML 二进制优化打包）](https://www.w3.org/TR/xop10/)|
|MTOM + SOAP1.2 绑定|[SOAP Message Transmission Optimization Mechanism（SOAP 消息传输优化机制）](https://www.w3.org/TR/soap12-mtom/)|
|MTOM SOAP 1.1 绑定|[SOAP 1.1 Binding for MTOM 1.0（用于 MTOM 1.0 的 SOAP 1.1 绑定）](https://www.w3.org/Submission/soap11mtom10/)|
|MTOM WS-PolicyAssertions|[MTOM 序列化策略断言 (MTOMPolicy) ](https://www.w3.org/Submission/WS-MTOMPolicy/)|

<xref:System.ServiceModel.Channels.SecurityBindingElement> 支持下表中的规范。

|规范/文档|链接|
|-----------------------------|----------|
|WSS：SOAP 消息安全 1.0|[Web Services 安全性： SOAP 消息安全1。0](https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)|
|WSS：用户名令牌配置文件 1.0|[Web Services 安全性 UsernameToken 配置文件1。0](https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0.pdf)<br /><br /> 要求 Password/@Type = PasswordText (默认值) |
|WSS：X.509 令牌配置文件 1.0|[Web Services Security X.509 Certificate Token Profile（Web 服务安全 X.509 证书令牌配置文件）](https://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0.pdf)|
|WSS：SAML 1.1 令牌配置文件 1.0|[Web Services Security: SAML Token Profile（Web 服务安全：SAML 令牌配置文件）](https://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.0.pdf)|
|WSS：SOAP 消息安全 1.1|[Web Services Security: SOAP Message Security 1.1（Web 服务安全：SOAP 消息安全 1.1）](https://www.oasis-open.org/committees/download.php/16790/wss-v1.1-spec-os-SOAPMessageSecurity.pdf)|
|WSS 用户名令牌配置文件 1.1|[Web Services Security UsernameToken Profile 1.1（Web 服务安全用户名令牌配置文件 1.1）](https://www.oasis-open.org/committees/download.php/16782/wss-v1.1-spec-os-UsernameTokenProfile.pdf)<br /><br /> 不实现基于密码的密钥派生；<br /><br /> 要求 Password/@Type = PasswordText (默认值) |
|WSS：X509 令牌配置文件 1.1|[Web Services Security X.509 Certificate Token Profile 1.1（Web 服务安全 X.509 证书令牌配置文件 1.1）](https://www.oasis-open.org/committees/download.php/16785/wss-v1.1-spec-os-x509TokenProfile.pdf)|
|WSS：Kerberos 令牌配置文件 1.1|[Web Services Security Kerberos Token Profile 1.1（Web 服务安全 Kerberos 令牌配置文件 1.1）](https://www.oasis-open.org/committees/download.php/16788/wss-v1.1-spec-os-KerberosTokenProfile.pdf)|
|WSS：SAML 1.1 令牌配置文件 1.1|[Web Services Security SAML Token Profile 1.1（Web 服务安全 SAML 令牌配置文件 1.1）](https://www.oasis-open.org/committees/download.php/16768/wss-v1.1-spec-os-SAMLTokenProfile.pdf)|
|WS-Secure 对话|[Web 服务安全对话语言](http://specs.xmlsoap.org/ws/2005/02/sc/ws-secureconversation.pdf)|
|WS-Trust 1.4|[Web 服务信任语言（](https://docs.oasis-open.org/ws-sx/ws-trust/200802)|
|WS-SecurityPolicy 2005/07|[Web 服务安全对话语言](http://specs.xmlsoap.org/ws/2005/02/sc/ws-secureconversation.pdf)<br /><br /> 已根据提交到 OASIS WS-SX 技术委员会的勘误表进行了修正。<br /><br /> [ws-sx 消息](https://lists.oasis-open.org/archives/ws-sx/200512/msg00017.html)|
|WS-ReliableMessaging 1.1|[可靠消息传送协议版本 1.1](reliable-messaging-protocol-version-1-1.md)|

<xref:System.ServiceModel.Channels.TransactionFlowBindingElement> 支持下表中的规范。

|规范/文档|链接|
|-----------------------------|----------|
|WS-Coordination|[Web Services Coordination（Web 服务协作）](/previous-versions/ms951231(v=msdn.10))|
|WS-AtomicTransaction|[Web Services Atomic Transaction（Web 服务原子事务）](http://specs.xmlsoap.org/ws/2004/10/wsat/wsat.pdf)|

<xref:System.ServiceModel.Description.MetadataExporter>、<xref:System.ServiceModel.Description.MetadataImporter>、<xref:System.ServiceModel.Description.WsdlExporter>、<xref:System.ServiceModel.Description.WsdlImporter> 和 <xref:System.ServiceModel.Description.MetadataResolver> 类支持以下元数据规范：

- [XML Schema Part 1: Structures Second Edition（XML 架构第 1 部分：结构，第二版）](https://www.w3.org/TR/xmlschema-1/)

- [XML 架构第 2 部分：数据类型，第二版](https://www.w3.org/TR/xmlschema-2/)

- [WSDL 1.1](https://www.w3.org/TR/wsdl/)

- [WS 策略1。2](https://www.w3.org/Submission/2006/SUBM-WS-Policy-20060425/)

- [WS-Policy 1.5](https://www.w3.org/TR/ws-policy/)

- [WS-PolicyAttachment 1.2](https://www.w3.org/Submission/2006/SUBM-WS-PolicyAttachment-20060425/)

- [WS-MetadataExchange 1.1](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)

- [WS-Transfer Get for metadata retrieval（面向元数据检索的 WS-Transfer Get）](https://www.w3.org/Submission/2006/SUBM-WS-Transfer-20060315/)

此外，以下互操作性配置文件通过 WCF 实现：

- [Basic Profile 1.1（基本配置文件 1.1）](http://www.ws-i.org/Profiles/BasicProfile-1.1-2004-08-24.html)

- [Simple SOAP Binding 1.0（简单 SOAP 绑定 1.0）](http://www.ws-i.org/Profiles/SimpleSoapBindingProfile-1.0-2004-08-24.html)

- [Basic Security Profile 1.0 Working Draft（基本安全配置文件 1.0 工作草案）](http://www.ws-i.org/Profiles/BasicSecurityProfile-1.0-2006-03-29.html)

## <a name="see-also"></a>请参阅

- [系统提供的互操作性绑定支持的 Web 服务协议](web-services-protocols-supported-by-system-provided-interoperability-bindings.md)
- [消息协议](messaging-protocols.md)
- [数据协定架构参考](data-contract-schema-reference.md)
- [WSDL 和策略](wsdl-and-policy.md)
- [安全协议](security-protocols.md)
- [可靠消息传送协议版本 1.0](reliable-messaging-protocol-version-1-0.md)
- [可靠消息传送协议版本 1.1](reliable-messaging-protocol-version-1-1.md)
- [事务协议](transaction-protocols.md)
- [上下文交换协议](context-exchange-protocol.md)
