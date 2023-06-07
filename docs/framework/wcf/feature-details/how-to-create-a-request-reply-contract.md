---
title: 如何：创建请求-答复协定
ms.date: 03/30/2017
ms.assetid: 801d90da-3d45-4284-9c9f-56c8aadb4060
ms.openlocfilehash: 506ce527348286bb53223c64245c74e4cb21879a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286547"
---
# <a name="how-to-create-a-request-reply-contract"></a>如何：创建请求-答复协定

请求-答复协定指定返回答复的方法。 必须根据此协定的条款发送答复并与请求相关联。 即使该方法不返回任何答复（采用 C# 语言时，返回 `void`，采用 Visual Basic 语言时，返回 `Sub`），基础结构也将创建一条空消息并将其发送给调用方。 若要防止发送空答复消息，请对操作使用单向协定。  
  
### <a name="to-create-a-request-reply-contract"></a>创建请求-答复协定  
  
1. 用所选编程语言创建接口。  
  
2. 将 <xref:System.ServiceModel.ServiceContractAttribute> 特性应用于该接口。  
  
3. 将 <xref:System.ServiceModel.OperationContractAttribute> 特性应用于客户端可调用的每个方法。  
  
4. 可选。 将 <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> 属性的值设置为 `true`，以防止发送空答复消息。 默认情况下，所有操作均是请求-答复协定。  
  
## <a name="example"></a>示例  

 下面的示例为提供 `Add` 和 `Subtract` 方法的计算器服务定义一个协定。 `Multiply` 方法不是协定的一部分，因为它没有通过 <xref:System.ServiceModel.OperationContractAttribute> 类进行标记，因此不可以由客户端访问。  
  
```csharp
using System.ServiceModel;

[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    // It would be equivalent to write explicitly:
    // [OperationContract(IsOneWay=false)]
    int Add(int a, int b);

    [OperationContract]
    int Subtract(int a, int b);

    int Multiply(int a, int b)
}
```
  
- 有关如何指定操作协定的详细信息，请参阅 <xref:System.ServiceModel.OperationContractAttribute> 类和 <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> 属性。  
  
- 在部署了服务之后，应用 <xref:System.ServiceModel.ServiceContractAttribute> 和 <xref:System.ServiceModel.OperationContractAttribute> 特性会导致在 Web 服务描述语言 (WSDL) 文档中自动生成服务协定定义。 通过将 `?wsdl` 追加到该服务的 HTTP 基址，可下载此文档。 例如 `http://microsoft/CalculatorService?wsdl`  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.OperationContractAttribute>
- [设计服务协定](../designing-service-contracts.md)
- [如何：创建双工协定](how-to-create-a-duplex-contract.md)
