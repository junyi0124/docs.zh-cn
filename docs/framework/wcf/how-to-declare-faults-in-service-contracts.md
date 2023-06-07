---
title: 如何：在服务协定中声明错误
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e8da98e7-d22f-4f60-ac82-3fb0928a353f
ms.openlocfilehash: 06262d5f698f8898e162e92dad272a7188897af0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240051"
---
# <a name="how-to-declare-faults-in-service-contracts"></a>如何：在服务协定中声明错误

在托管代码中，出现错误条件时引发异常。 但在 Windows Communication Foundation (WCF) 应用程序中，服务协定通过在服务协定中声明 SOAP 错误来指定向客户端返回的错误信息。 有关异常和错误间关系的概述，请参阅 [在协定和服务中指定和处理错误](specifying-and-handling-faults-in-contracts-and-services.md)。

## <a name="create-a-service-contract-that-specifies-a-soap-fault"></a>创建指定 SOAP 错误的服务协定

1. 创建至少包含一个操作的服务协定。 有关示例，请参阅 [如何：定义服务协定](how-to-define-a-wcf-service-contract.md)。

2. 选择可以指定希望向客户端通知的错误条件的操作。 若要确定哪些错误条件将 SOAP 错误返回给客户端，请参阅 [在协定和服务中指定和处理错误](specifying-and-handling-faults-in-contracts-and-services.md)。

3. 为所选操作应用 <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType> 并向构造函数传递可序列化错误类型。 有关创建和使用可序列化类型的详细信息，请参阅 [在服务协定中指定数据传输](./feature-details/specifying-data-transfer-in-service-contracts.md)。 下面的示例演示如何指定 `SampleMethod` 操作可以产生 `GreetingFault`。

     [!code-csharp[FaultContractAttribute#4](~/samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#4)]
     [!code-vb[FaultContractAttribute#4](~/samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#4)]

4. 为向客户端传达错误条件的协定中的所有操作重复步骤 2 和步骤 3。

## <a name="implementing-an-operation-to-return-a-specified-soap-fault"></a>实现返回指定 SOAP 错误的操作

 操作指定可以返回特定 SOAP 错误（例如在前面的过程中）来向调用应用程序传达错误条件后，下一步就是实现该指定。

### <a name="throw-the-specified-soap-fault-in-the-operation"></a>在操作中引发指定的 SOAP 错误

1. 当操作中出现 <xref:System.ServiceModel.FaultContractAttribute> 指定的错误条件时，将引发一个新的 <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType>，其中指定的 SOAP 错误是类型参数。 下面的示例演示如何在前面的过程和以下代码部分中显示的 `GreetingFault` 中引发 `SampleMethod`。

     [!code-csharp[FaultContractAttribute#5](~/samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#5)]
     [!code-vb[FaultContractAttribute#5](~/samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#5)]

## <a name="example"></a>示例

下面的代码示例演示为 `GreetingFault` 操作指定 `SampleMethod` 的单个操作的实现。

[!code-csharp[FaultContractAttribute#1](~/samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#1)]
[!code-vb[FaultContractAttribute#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#1)]

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.FaultContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType>
