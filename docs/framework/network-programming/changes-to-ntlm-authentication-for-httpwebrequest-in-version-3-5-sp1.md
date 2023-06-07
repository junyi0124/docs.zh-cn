---
title: 3\.5 SP1 版本中对 HttpWebRequest 的 NTLM 身份验证的更改
ms.date: 03/30/2017
ms.assetid: 8bf0b428-5a21-4299-8d6e-bf8251fd978a
ms.openlocfilehash: 388e6dc648e1fd68e24a852cb08de107f09f9c9f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "64754876"
---
# <a name="changes-to-ntlm-authentication-for-httpwebrequest-in-version-35-sp1"></a>3\.5 SP1 版本中对 HttpWebRequest 的 NTLM 身份验证的更改

在 .NET Framework 版本 3.5 SP1 及以上版本中做出了安全性更改，这些更改影响以下类处理集成式 Windows 身份验证的方式：<xref:System.Net.HttpWebRequest>、 <xref:System.Net.HttpListener>、 <xref:System.Net.Security.NegotiateStream>以及 System.Net 命名空间中的相关类。 这些更改会影响使用这些类来发出 Web 请求和接收响应的应用程序，这些应用程序使用基于 NTLM 的集成式 Windows 身份验证。 此更改会影响配置为使用集成式 Windows 身份验证的 Web 服务器和客户端应用程序。

## <a name="overview"></a>概述

集成式 Windows 身份验证的设计能使某些凭据响应变得通用，这意味着可以重新使用或转接这些响应。 如果不需要此特殊设计功能，那么身份验证协议应包含目标特定信息和通道特定信息。 然后，服务可提供延伸保护，确保凭据响应包含服务主体名称 (SPN) 等服务特定信息。 凭据交换中含有此信息时，服务就能更好防范恶意使用可能是通过不当方式获取的凭据响应。

<xref:System.Net> 和 <xref:System.Net.Security> 命名空间中的多个组件代表调用应用程序执行集成式 Windows 身份验证。 本部分介绍为扩展 System.Net 组件在使用集成式 Windows 身份验证方面的保护对这些组件做出的更改。

## <a name="changes"></a>Changes

与集成式 Windows 身份验证搭配使用的 NTLM 身份验证过程包括由目标计算机发出并发送回客户端计算机的质询。 计算机接收到它自己产生的质询时，身份验证将失败，除非连接为环回连接（例如 IPv4 地址 127.0.0.1）。

访问在内部 Web 服务器上运行的服务时，通常使用类似于 `http://contoso/service` 或 `https://contoso/service` 的 URL 访问服务。 名称“contoso”通常不是部署了该服务的计算机的计算机名。 <xref:System.Net> 和相关命名空间支持使用 Active Directory、 DNS、 NetBIOS、本地计算机的主机文件（例如，通常为 WINDOWS\system32\drivers\etc\hosts），或本地计算机的 lmhosts 文件（例如，通常为 WINDOWS\system32\drivers\etc\lmhosts）将名称解析为地址。 已解析名称“contoso”，所以发送到“contoso”的请求将发送到相应的服务器计算机。

为大型部署配置时，常见的情况是，将单个虚拟服务器名提供给部署，而客户端应用程序和最终用户从未使用过基础计算机名。 例如，可能会调用服务器 `www.contoso.com`，但在内部网络中只需使用“contoso”。 在客户端 Web 请求中，此名称被称为主机标头。 根据 HTTP 协议所指定，主机请求标头字段指定所请求的资源的 Internet 主机和端口号。 从用户或引用资源提供的原始 URI（通常是 HTTP URL）中获取此信息。 在 .NET Framework 版本 4 中，此信息也可由使用新 <xref:System.Net.HttpWebRequest.Host%2A> 属性的客户端设置。

<xref:System.Net.AuthenticationManager> 类控制由 <xref:System.Net.WebRequest> 衍生类和 <xref:System.Net.WebClient> 类使用的托管身份验证组件（“模块”）。 <xref:System.Net.AuthenticationManager> 类提供一个属性，该属性公开由 URI 字符串变址的 <xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A?displayProperty=nameWithType> 对象，以便应用程序提供要在身份验证期间使用的自定义 SPN 字符串。

当 <xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A> 未设置属性时，3.5 SP1 现在默认指定在 NTLM（NT LAN 管理器）身份验证交换中 的 SPN 的请求 URL 中使用的主机名。 在请求 URL 中使用的主机名可能不同于在客户端请求中的 <xref:System.Net.HttpRequestHeader?displayProperty=nameWithType> 中指定的主机标头。 在请求 URL 中使用的主机名可能不同于服务器的实际主机名、服务器的计算机名、计算机的 IP 地址或环回地址。 在这些情况下，Windows 将无法通过身份验证请求。 要解决此问题，需要通知 Windows 客户端请求中的请求 URL 中使用的主机名（例如“contoso”）实际上是本地计算机的备用名称。

服务器应用程序有多种可行方法可暂时避开此更改所具有的问题。 建议的方法是将请求 URL 中所用主机名映射到服务器上注册表中的 `BackConnectionHostNames` 键。 `BackConnectionHostNames` 注册表项通常用于将主机名映射到环回地址。 下面列出了这些步骤。

要指定将映射到环回地址并且可在本地计算机上连接到网站的主机名，请按以下步骤操作：

1. 单击“开始”、“运行”，键入 regedit，然后单击“确定”。

2. 在注册表编辑器中，找到以下注册表项，然后单击它：

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\MSV1_0`

3. 右键单击“MSV1_0”，指向“新建”，然后单击“多字符串值”。

4. 键入 `BackConnectionHostNames`，然后按 Enter。

5. 右键单击 `BackConnectionHostNames`，然后单击“修改”。

6. 在“数值数据”框中，键入主机名或本地计算机上站点的主机名（请求 URL 中使用的主机名），然后单击“确定”。

7. 退出注册表编辑器，然后重新启动 IISAdmin 服务并运行 IISReset。

如 <https://support.microsoft.com/kb/896861> 中所述，安全级别较低的变通方法是禁用环回检查。 这将禁用反射攻击保护。 因此，最好将一组备用名称限制为仅希望计算机实际使用的那些名称。

## <a name="see-also"></a>另请参阅

- <xref:System.Net.AuthenticationManager.CustomTargetNameDictionary%2A?displayProperty=nameWithType>
- <xref:System.Net.HttpRequestHeader?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest.Host%2A?displayProperty=nameWithType>
