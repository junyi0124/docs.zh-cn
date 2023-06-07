---
title: 扩展保护策略
ms.date: 03/30/2017
ms.assetid: e2616a10-317e-4c34-8023-0c015a80a82f
ms.openlocfilehash: b513eafecf9b5eaee49b5bc318b3f4bad71213a2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281919"
---
# <a name="extended-protection-policy"></a>扩展保护策略

扩展保护是一种针对中间人 (MITM) 攻击提供保护的安全计划。 MITM 攻击是一种安全威胁，MITM 在该攻击中获取客户端凭据并将其转发给服务器。  
  
## <a name="demonstrates"></a>演示  

 扩展保护  
  
## <a name="discussion"></a>讨论 (Discussion)  

 当应用程序通过 HTTPS 使用 Kerberos、Digest 或 NTLM 进行身份验证时，首先会建立一个传输层安全 (TLS) 通道，然后使用此安全通道进行身份验证。 但是，SSL 生成的会话密钥和身份验证期间生成的会话密钥之间没有任何绑定。 即使传输通道本身是安全的，任何 MITM 也可以在客户端和服务器之间建立自身，然后开始转发来自客户端的请求，因为服务器无法知道安全通道是从客户端还是 MITM 建立的。 此方案中的解决方案是将外部 TLS 通道与内部身份验证通道绑定，从而使服务器可以检测是否存在 MITM。  
  
> [!NOTE]
> 本示例仅当在 IIS 上承载时才可正常运行，而不适用于 Cassini（Visual Studio 开发服务器），因为 Cassini 不支持 HTTPS。  
  
> [!NOTE]
> 此功能当前只在 Windows 7 上可用。 以下步骤特定于 Windows 7。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 从 **"控制面板**"、" **添加/删除程序**"、" **Windows 功能**" 中安装 Internet Information Services。  
  
2. 在 **Windows 功能** 中安装 **windows 身份验证**， **Internet Information Services**， **World Wide Web 服务**，**安全性** 和 **Windows 身份验证**。  
  
3. 在 **Windows 功能** 中安装 **Windows Communication Foundation http 激活**， **Microsoft .NET 框架 3.5.1**，并 **Windows Communication Foundation HTTP 激活**。  
  
4. 本示例要求客户端与服务器建立一个安全通道，因此它要求存在服务器证书，此证书可从 Internet 信息服务 (IIS) 管理器进行安装。  
  
    1. 打开 IIS 管理器。 打开 " **服务器证书**"，当选择根节点 (计算机名称) 时，这些证书将出现在 " **功能视图** " 选项卡中。  
  
    2. 为了测试此示例，可以创建一个自签名证书。 如果不希望 Internet Explorer 提示证书不安全，可将证书安装到受信任的证书根颁发机构存储区中。  
  
5. 打开 "默认网站" 的 " **操作** " 窗格。 单击 " **编辑站点**"、" **绑定**"。 如果 HTTPS 尚不存在，请将其作为类型添加，使用端口号 443。 分配在上一步中创建的 SSL 证书。  
  
6. 生成服务。 这会在 IIS 中创建一个虚拟目录，并根据需要为要进行 Web 承载的服务复制 .dll、.svc 和 .config 文件。  
  
7. 打开 IIS 管理器。 右键单击在上一步中创建的虚拟目录 (**ExtendedProtection**) 。 选择 " **转换为应用程序**"。  
  
8. 在 IIS 管理器中为此虚拟目录打开 " **身份验证** " 模块，并启用 " **Windows 身份验证**"。  
  
9. 为此虚拟目录打开 " **Windows 身份验证**" 下的 **高级设置**，并将其设置为 "**必需**"。  
  
10. 通过从浏览器窗口（提供完全限定的域名）访问 HTTPS URL 可测试服务。 如果想要从远程计算机访问此 URL，请确保为所有传入 HTTP 和 HTTPS 连接打开了防火墙。  
  
11. 打开客户端配置文件，并为客户端或终结点地址特性提供完全限定的域名，以替换 `<<full_machine_name>>`。  
  
12. 运行客户端。 客户端与服务进行通信，这会建立安全通道并使用扩展保护。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Security\ExtendedProtection`
