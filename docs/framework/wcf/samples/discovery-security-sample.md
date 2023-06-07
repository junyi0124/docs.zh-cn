---
title: 发现安全示例
ms.date: 03/30/2017
ms.assetid: b8db01f4-b4a1-43fe-8e31-26d4e9304a65
ms.openlocfilehash: 0957e8ddaee99e60b205c92319dc2c61e2ab007a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96292631"
---
# <a name="discovery-security-sample"></a>发现安全示例

Discovery 规范不要求参与发现过程的终结点是安全的。 增强发现消息的安全性可缓解各种类型的攻击（消息更改、拒绝服务、重播、欺骗）。 本示例实现自定义通道，这些通道计算和验证使用精简签名格式（在 WS-Discovery 规范的第 8.2 节中进行了介绍）的消息签名。 该示例支持 [2005 发现规范](http://specs.xmlsoap.org/ws/2005/04/discovery/ws-discovery.pdf) 和 [1.1 版本](http://docs.oasis-open.org/ws-dd/discovery/1.1/cs-01/wsdd-discovery-1.1-spec-cs-01.pdf)。  
  
 自定义通道应用于发现和公告终结点的现有通道堆栈顶部。 这样，对于发送的每个消息都应用一个签名标头。 会对接收的消息验证签名，如果签名不匹配或消息没有签名，则丢弃消息。 本示例使用证书对消息进行签名和验证。  
  
## <a name="discussion"></a>讨论 (Discussion)  

 WCF 可扩展，并允许用户根据需要自定义通道。 本示例实现一个生成安全通道的发现安全绑定元素。 安全通道应用并验证消息签名，在当前堆栈顶部应用。  
  
 安全绑定元素生成安全通道工厂和通道侦听器。  
  
## <a name="secure-channel-factory"></a>安全通道工厂  

 安全通道工厂创建输出或双工通道，这些通道向消息标头添加精简签名。 若要使消息尽可能地小，请使用精简签名格式。 精简签名的结构显示在下面的示例中。  
  
```xml  
<d:Security ... >
  [<d:Sig Scheme="xs:anyURI"
         [KeyId="xs:base64Binary"]?  
          Refs="..."  
         [PrefixList]="xs:NMTOKENS"
          Sig="xs:base64Binary"
          ... />]?  
  ...
</d:Security>  
```  
  
> [!NOTE]
> `PrefixList` 已加入 2008 Discovery 版本协议。  
  
 为计算签名，示例确定扩展的签名项。 根据 WS-Discovery 规范的要求，使用 `SignedInfo` 命名空间前缀创建 XML 签名 (`ds`)。 发现和寻址命名空间中的正文和所有标头都在签名中进行引用，因此无法对这些内容进行篡改。 每个引用的元素都使用专用规范化 (<http://www.w3.org/2001/10/xml-exc-c14n#>) 进行转换，然后在)  (计算 sha-1 digest 值 <http://www.w3.org/2000/09/xmldsig#sha1> 。 基于所有引用的元素及其摘要值，将使用 RSA 算法 () 计算签名值 <http://www.w3.org/2000/09/xmldsig#rsa-sha1> 。  
  
 消息使用特定于客户端的证书进行签名。 创建绑定元素时，必须指定存储位置、名称和证书使用者名称。 精简签名中的 `KeyId` 表示签名令牌的密钥标识符，是签名令牌的主题密钥标识符 (SKI) 或者是（如果 SKI 不存在）签名令牌的公钥的 SHA-1 哈希值。  
  
## <a name="secure-channel-listener"></a>安全通道侦听器  

 安全通道侦听器创建输入或双工通道，这些通道验证接收的消息中的精简签名。 为验证签名，使用附加到消息的精简签名中指定的 `KeyId` 从指定存储区中选择证书。 如果消息没有签名或签名检查失败，则丢弃消息。 为使用安全绑定，本示例定义一个工厂，该工厂使用添加的发现安全绑定元素创建自定义 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> 和 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>。 这些安全终结点可以用于发现公告侦听器和可发现服务。  
  
## <a name="sample-details"></a>示例详细信息  

 本示例包含一个库和 4 个控制台应用程序：
  
- **DiscoverySecurityChannels**：公开安全绑定的库。 该库计算并验证传出/传入消息的精简签名。  
  
- **服务**：公开 ICalculatorService 协定的服务公开。 该服务标记为可发现。 用户通过指定证书的存储位置、名称和主题名称或其他唯一标识符来指定用于对消息签名的证书的详细信息，并指定客户端证书（用于检查传入消息的签名的证书）所处的存储区。 基于这些详细信息，生成并使用增加了安全性的 UdpDiscoveryEndpoint。  
  
- **客户端**：此类尝试发现 ICalculatorService 并对服务调用方法。 同样，生成增加了安全性的 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>，并用于对消息进行签名和验证。  
  
- **AnnouncementListener**：一种自承载服务，用于侦听联机和脱机公告，并使用安全公告终结点。  
  
> [!NOTE]
> 如果 Setup.bat 运行多次，则因为存在多个证书，所以证书管理器会提示您选择要添加的证书。 在这种情况下，应中止 Setup.bat 并调用 Cleanup.bat，因为已创建了重复项。 Cleanup.bat 还会提示您选择要删除的证书。 从列表中选择证书并继续执行 Cleanup.bat，直至没有证书剩余。  
  
#### <a name="to-use-this-sample"></a>使用此示例  
  
1. 从 Visual Studio 的开发人员命令提示执行 Setup.bat 脚本。 本示例使用证书对消息进行签名和验证。 该脚本使用 Makecert.exe 创建证书，然后使用 Certmgr.exe 安装这些证书。 该脚本必须使用管理员特权运行。  
  
2. 若要生成并运行该示例，请在 Visual Studio 中打开安全 .sln 文件，然后选择 " **全部重新生成**"。 更新解决方案属性以启动多个项目：为除 DiscoverySecureChannels 以外的所有项目选择 " **启动** "。 正常运行解决方案。  
  
3. 完成示例后，执行 Cleanup.bat 脚本移除为此示例创建的证书。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\DiscoveryScenario`  
