---
title: 如何：使用双工协定访问服务
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- duplex contracts [WCF]
ms.assetid: 746a9d64-f21c-426c-b85d-972e916ec6c5
ms.openlocfilehash: bc42792b827b49265a0b1addf959de2fa1a041e3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597210"
---
# <a name="how-to-access-services-with-a-duplex-contract"></a>如何：使用双工协定访问服务

Windows Communication Foundation （WCF）的一项功能是能够创建使用双工消息传递模式的服务。 此模式允许服务通过回调与客户端进行通信。 本主题演示在实现回调接口的客户端类中创建 WCF 客户端的步骤。

双向绑定向服务公开客户端的 IP 地址。 客户端应使用安全来确保仅连接到自己信任的服务。

有关创建基本 WCF 服务和客户端的教程，请参阅[入门教程](../getting-started-tutorial.md)。

## <a name="to-access-a-duplex-service"></a>访问双工服务

1. 创建包含两个接口的服务。 第一个接口用于服务，第二个接口用于回调。 有关创建双工服务的详细信息，请参阅[如何：创建双工协定](how-to-create-a-duplex-contract.md)。

2. 运行服务。

3. 使用 " [svcutil.exe" 元数据实用工具（）](../servicemodel-metadata-utility-tool-svcutil-exe.md)为客户端生成协定（接口）。 有关如何执行此操作的信息，请参阅[如何：创建客户端](../how-to-create-a-wcf-client.md)。

4. 在客户端类中实现回调接口，如下面的示例所示。

    ```csharp
    public class CallbackHandler : ICalculatorDuplexCallback
    {
        public void Result(double result)
        {
            Console.WriteLine("Result ({0})", result);
        }
        public void Equation(string equation)
        {
            Console.WriteLine("Equation({0})", equation);
        }
    }
    ```

    ```vb
    Public Class CallbackHandler
    Implements ICalculatorDuplexCallback
       Public Sub Result (ByVal result As Double)
          Console.WriteLine("Result ({0})", result)
       End Sub
        Public Sub Equation(ByVal equation As String)
            Console.WriteLine("Equation({0})", equation)
        End Sub
    End Class
    ```

5. 创建 <xref:System.ServiceModel.InstanceContext> 类的一个实例。 构造函数需要客户端类的一个实例。

    ```csharp
    InstanceContext site = new InstanceContext(new CallbackHandler());
    ```

    ```vb
    Dim site As InstanceContext = New InstanceContext(new CallbackHandler())
    ```

6. 使用需要对象的构造函数创建 WCF 客户端的实例 <xref:System.ServiceModel.InstanceContext> 。 该构造函数的第二个参数是配置文件中找到的终结点的名称。

    ```csharp
    CalculatorDuplexClient wcfClient = new CalculatorDuplexClient(site, "default");
    ```

    ```vb
    Dim wcfClient As New CalculatorDuplexClient(site, "default")
    ```

7. 根据需要调用 WCF 客户端的方法。

## <a name="example"></a>示例

下面的代码示例演示如何创建一个访问双工协定的客户端类。

[!code-csharp[S_DuplexClients#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_duplexclients/cs/client.cs#1)]
[!code-vb[S_DuplexClients#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_duplexclients/vb/client.vb#1)]

## <a name="see-also"></a>另请参阅

- [入门教程](../getting-started-tutorial.md)
- [如何：创建双工协定](how-to-create-a-duplex-contract.md)
- [ServiceModel 元数据实用工具 (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
- [如何：创建客户端](../how-to-create-a-wcf-client.md)
- [如何：使用 ChannelFactory](how-to-use-the-channelfactory.md)
