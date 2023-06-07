---
title: 如何：配置 WCF 客户端以与 WSE3.0 服务进行互操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3dadd7f1-d207-4ea5-a73b-3e8aa44407f8
ms.openlocfilehash: b5284db1329c572bdecf3ef607e697c63835d508
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257517"
---
# <a name="how-to-configure-a-wcf-client-to-interoperate-with-wse30-services"></a>如何：配置 WCF 客户端以与 WSE3.0 服务进行互操作

将 WCF 客户端配置为使用 (规范的8月2004版时，Windows Communication Foundation (WCF) 客户端与 Web 服务增强 3.0 Microsoft .NET 的线路级别兼容。  
  
### <a name="to-configure-a-wcf-client-to-interoperate-with-a-wse-30-web-service"></a>配置 WCF 客户端以与 WSE 3.0 Web 服务进行互操作  
  
1. 运行 "工作网络 [元数据实用工具" 工具 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 为 WSE 3.0 Web 服务创建 WCF 客户端。  
  
     对于 WSE Web 服务，将创建 WCF 客户端类。  
  
     有关创建 WCF 客户端的详细信息，请参阅 [如何：创建客户端](../how-to-create-a-wcf-client.md)。  
  
2. 创建一个类，表示可与 WSE 3.0 Web 服务进行通信的绑定。  
  
     下面的类是 [与 WSE 示例互](/previous-versions/dotnet/netframework-3.5/ms752257(v=vs.90)) 操作的一部分。  
  
    1. 创建一个从 <xref:System.ServiceModel.Channels.Binding> 类派生的类。  
  
         下面的代码示例创建一个从 `WseHttpBinding` 类派生的名为 <xref:System.ServiceModel.Channels.Binding> 的类。  
  
         [!code-csharp[c_WCFClientToWSEService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#1)]
         [!code-vb[c_WCFClientToWSEService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#1)]  
  
    2. 向该类添加指定 WSE 完整声明、是否需要派生密钥、是否使用安全会话、是否需要签名确认以及消息保护设置的属性。  
  
         下面的代码示例定义了 `SecurityAssertion` 、 `RequireDerivedKeys` 、 `EstablishSecurityContext` 和 `MessageProtectionOrder` 属性。 它们指定 WSE 全包式断言、是否需要派生密钥、是否使用安全会话、是否需要签名确认以及消息保护设置。  
  
         [!code-csharp[c_WCFClientToWSEService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#3)]
         [!code-vb[c_WCFClientToWSEService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#3)]  
  
    3. 重写 <xref:System.ServiceModel.Channels.Binding.CreateBindingElements%2A> 方法以设置绑定属性。  
  
         下面的代码示例通过获取 `SecurityAssertion` 和 `MessageProtectionOrder` 属性的值来指定传输协议、消息编码和消息保护设置。  
  
         [!code-csharp[c_WCFClientToWSEService#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/wsehttpbinding.cs#2)]
         [!code-vb[c_WCFClientToWSEService#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/wsehttpbinding.vb#2)]  
  
3. 在客户端应用程序代码中，添加设置绑定属性的代码。  
  
     下面的代码示例指定 WCF 客户端必须使用由 WSE 3.0 `AnonymousForCertificate` 交钥匙安全声明定义的消息保护和身份验证。 此外，还需要安全会话和派生密钥。  
  
     [!code-csharp[c_WCFClientToWSEService#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/client.cs#4)]
     [!code-vb[c_WCFClientToWSEService#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/client.vb#4)]  
  
## <a name="example"></a>示例  

 下面的代码示例定义了一个自定义绑定，该绑定公开对应于 WSE 3.0 完整安全断言的各个属性的属性。 然后，将使用名为的自定义绑定 `WseHttpBinding` 来指定 WCF 客户端的绑定属性。  

[!code-csharp[c_WCFClientToWSEService#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wcfclienttowseservice/cs/client.cs#0)]
[!code-vb[c_WCFClientToWSEService#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wcfclienttowseservice/vb/client.vb#0)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.Binding>
- [与 WSE 互操作](/previous-versions/dotnet/netframework-3.5/ms752257(v=vs.90))
