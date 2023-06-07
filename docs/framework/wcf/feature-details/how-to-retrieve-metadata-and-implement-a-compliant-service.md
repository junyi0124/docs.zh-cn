---
title: 如何：检索元数据并实现兼容服务
ms.date: 03/30/2017
ms.assetid: f6f3a2b9-c8aa-4b0b-832c-ec2927bf1163
ms.openlocfilehash: 92d1549bfaf1f08c4e4200b5305aa130fdfc152c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276043"
---
# <a name="how-to-retrieve-metadata-and-implement-a-compliant-service"></a>如何：检索元数据并实现兼容服务

通常，设计和实现服务并不是由同一个人完成的。 在交互操作应用程序很重要的环境中，可以用 Web 服务描述语言 (WSDL) 设计或描述协定，而且开发人员必须实现一个与所提供的协定相兼容的服务。 你可能还想要将现有服务迁移到 Windows Communication Foundation (WCF) 但保留有线格式。 此外，双工协定还需要调用方实现一个回调协定。  
  
 在这些情况下，你必须使用 " [ (" 元数据实用工具 "工具) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) (或等效的工具) 以托管语言生成服务协定接口，你可以实现该接口以实现协定的要求。 通常， [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 用于获取与通道工厂或 WCF 客户端类型一起使用的服务协定，以及用于设置正确的绑定和地址的客户端配置文件。 若要使用生成的配置文件，则必须将其更改到服务配置文件中。 您可能还需要修改服务协定。  
  
### <a name="to-retrieve-data-and-implement-a-compliant-service"></a>检索数据并实现兼容服务  
  
1. 使用 "工作的元数据实用工具" 工具对元数据文件或元数据终结点 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) ，以生成代码文件。  
  
2. 搜索输出代码文件中包含相关接口的部分（以防存在多个接口），此接口是用 <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType> 属性标记的。 下面的代码示例显示了由 "工作的 [元数据实用工具" 工具 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)生成的两个接口。 第一个 (`ISampleService`) 是服务协定接口，实现它可创建兼容服务。 第二个 (`ISampleServiceChannel`) 是帮助器接口，客户端使用它可同时扩展服务协定接口和 <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType>，且该接口可用于客户端应用程序。  
  
     [!code-csharp[ClientProxyCodeSample#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/proxycode.cs#2)]  
  
3. 如果 WSDL 未指定所有操作的答复操作，则生成的操作协定可能会将 <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> 属性设置为通配符 (*)。 移除该属性设置。 否则，当您实现服务协定元数据时，将不能为这些操作导出元数据。  
  
4. 实现类上的接口并承载服务。 有关示例，请参阅 [如何：实现服务协定](../how-to-implement-a-wcf-contract.md)，或在下面的 "示例" 部分中查看简单实现。  
  
5. 在 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 生成的客户端配置文件中，将 "配置" 部分更改 [\<client>](../../configure-apps/file-schema/wcf/client.md) 为 "配置" [\<services>](../../configure-apps/file-schema/wcf/services.md) 部分。 （有关生成的客户端应用程序配置文件的示例，请参见下面的“示例”部分。）  
  
6. 在 " [\<services>](../../configure-apps/file-schema/wcf/services.md) 配置" 部分中， `name` 在 " [\<services>](../../configure-apps/file-schema/wcf/services.md) 配置" 部分为服务实现创建一个属性。  
  
7. 将服务的 `name` 属性设置为服务实现的配置名称。  
  
8. 将使用实现的服务协定的终结点配置元素添加到服务配置部分。  
  
## <a name="example"></a>示例  

 下面的代码示例演示了通过运行 " [ ( 的元数据实用工具" 工具 ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 生成的大多数代码文件，并对元数据文件运行了 # A0) 。  
  
 下面的代码演示：  
  
- 实现时符合协定要求的服务协定接口 (`ISampleService`)。  
  
- 客户端所使用的帮助器接口，可用于同时扩展服务协定接口和 <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType>，并可用于客户端应用程序 (`ISampleServiceChannel`)。  
  
- 扩展 <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType> 的帮助器类，可用于客户端应用程序 (`SampleServiceClient`)。  
  
- 从服务生成的配置文件。  
  
- 简单的 `ISampleService` 服务实现。  
  
- 客户端配置文件到服务器端版本的转换。  
  
[!code-csharp[ClientProxyCodeSample#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/proxycode.cs#1)]

[!code-xml[ClientProxyCodeSample#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/client.exe.config#10)]

[!code-csharp[ClientProxyCodeSample#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/hostapplication.cs#5)]

[!code-xml[ClientProxyCodeSample#20](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/hostapplication.exe.config#20)]
  
## <a name="see-also"></a>另请参阅

- [ServiceModel 元数据实用工具 (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
