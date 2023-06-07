---
title: 自动代理检测
description: 了解自动代理检测，其中系统标识 Web 代理服务器并将其用于代表客户端发送请求。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- automatic proxy detections
- Web Proxy Auto-Discovery
- Web proxy
- detecting proxies automatically
- WebProxy class, automatic proxy detections
- proxies, automatically detecting
- network
- WPAD (Web Proxy Auto-Discovery)
ms.assetid: fcd9c3bd-93de-4c92-8ff3-837327ad18de
ms.openlocfilehash: 8d1b904a8acc6d3960a076c54c2d5f5de54820c0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250627"
---
# <a name="automatic-proxy-detection"></a>自动代理检测

自动代理检测是一个进程，系统使用该进程来标识 Web 代理服务器，并用于代表客户端发送请求。 此功能也称为 Web 代理自动发现 (WPAD)。 启用自动代理检测后，系统会尝试查找代理配置脚本，该脚本负责返回一组可用于请求的代理。 如果找到了代理配置脚本，则会在针对使用 <xref:System.Net.WebProxy> 实例的请求获取代理信息、请求流或响应时，在本地计算机上下载、编译并运行该脚本。  
  
 自动代理检测由 <xref:System.Net.WebProxy> 类执行，且可采用请求级设置、配置文件中的设置和通过 Internet Explorer“局域网(LAN)”对话框指定的设置。  
  
> [!NOTE]
> 可通过以下方式显示 Internet Explorer 的“局域网(LAN)设置”对话框：在 Internet Explorer 主菜单中选择“工具”，然后选择“Internet 选项”  。 接下来，选择“连接”选项卡，然后单击“LAN 设置” 。  
  
 启用自动代理检测后，<xref:System.Net.WebProxy> 类会尝试按如下方式找到代理配置脚本：  
  
1. WinINet `InternetQueryOption` 函数用于查找 Internet Explorer 最近检测到的代理配置脚本。  
  
2. 如果找不到该脚本，<xref:System.Net.WebProxy> 类将使用动态主机配置协议 (DHCP) 查找该脚本。 DHCP 服务器可以采用脚本的位置（主机名）或脚本的完整 URL 进行响应。  
  
3. 如果 DHCP 未标识 WPAD 主机，则查询 DNS 以找到 WPAD 作为其名称或别名的主机。  
  
4. 如果未标识该主机，并且代理配置脚本的位置由 Internet Explorer LAN 设置或配置文件指定，则使用此位置。  
  
> [!NOTE]
> 作为 NT 服务或 ASP.NET 的一部分运行的应用程序使用调用用户的 Internet Explorer 代理服务器设置（如果可用）。 这些设置可能并非对所有服务应用程序都可用。  
  
 代理基于每个 connectoid 进行配置。 connectoid 是网络连接对话框中的一项，可以是物理网络设备（调制解调器或以太网卡）或虚拟接口（例如，通过网络设备运行的 VPN 连接）。 如果 connectoid 发生更改（例如，无线连接更改了访问点，或启用了 VPN），则将再次运行代理检测算法。  
  
 默认情况下，Internet Explorer 代理设置用于检测此代理。 如果应用程序基于非交互式帐户运行（没有用于配置 IE 代理设置的简便方法），或要使用不同于 IE 设置的代理设置，则可以通过创建定义了 [\<defaultProxy> 元素（网络设置）](../configure-apps/file-schema/network/defaultproxy-element-network-settings.md)和 [\<proxy> 元素（网络设置）](../configure-apps/file-schema/network/proxy-element-network-settings.md)元素的配置文件，对代理进行配置。  
  
 对于所创建的请求，可通过将空 <xref:System.Net.WebRequest.Proxy%2A> 用于该请求，禁用请求级别的自动代理检测，如下面的代码示例所示。  
  
```csharp  
public static void DisableForMyRequest (Uri resource)  
{  
    WebRequest request = WebRequest.Create (resource);  
    request.Proxy = null;  
    WebResponse response = request.GetResponse ();  
}  
```  
  
```vb  
Public Shared Sub DisableForMyRequest(ByVal resource As Uri)  
    Dim request As WebRequest = WebRequest.Create(resource)  
    request.Proxy = Nothing  
    Dim response As WebResponse = request.GetResponse()  
    End Sub
```  
  
 没有代理的请求将使用应用程序域的默认代理（通过 <xref:System.Net.WebRequest.DefaultWebProxy%2A> 属性提供）。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.WebProxy>
- <xref:System.Net.WebRequest>
- [\<system.Net> 元素（网络设置）](../configure-apps/file-schema/network/system-net-element-network-settings.md)
