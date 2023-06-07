---
title: 如何：将服务名字对象与元数据交换协定一起使用
ms.date: 03/30/2017
ms.assetid: c41a07e5-cb9d-45d6-9ea4-34511e227faf
ms.openlocfilehash: 194caf6a48e64a4358a77ecd514dda456cc35e0b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265955"
---
# <a name="how-to-use-a-service-moniker-with-metadata-exchange-contracts"></a>如何：将服务名字对象与元数据交换协定一起使用

开发一些新的 WCF 服务后，你可能会决定希望能够从脚本或 Visual Basic 6.0 应用程序调用这些服务。 一种方法是生成 WCF 客户端程序集，使用 COM 注册该程序集，将程序集安装到 GAC 中，然后从 Visual Basic 代码引用 COM 类型。 分发应用程序时，还必须分发 WCF 客户端程序集。 然后，用户必须使用 COM 注册 WCF 客户端程序集，并且将该程序集放置在 GAC 中。 WCF COM 互操作还允许您在不依赖 WCF 客户端程序集的情况下进行相同的服务调用。 利用 WCF 名字对象，您可以从任何与 COM 兼容的语言中调用任何 WCF 服务 (Visual Basic、VBScript、Visual Basic for Applications (VBA) 等) 上，通过指定服务标记使用的元数据交换 (Mex) 终结点 URI 来提取有关服务的类型信息。 本主题说明如何使用指定 Mex 终结点的 WCF 名字对象调用入门 WCF 示例。  
  
> [!NOTE]
> WCF 客户端程序集定义的类型永远不会实际实例化。 该程序集只用于元数据。  
  
### <a name="using-the-service-moniker-with-a-mex-address"></a>使用带有 Mex 地址的服务标记  
  
1. 生成入门示例，并使用 Internet Explorer 浏览到其 URL (`http://localhost/ServiceModelSamples/Service.svc`) ，以确保服务正常工作。  
  
2. 生成 Visual Basic 脚本或包含以下代码的 Visual Basic 应用程序：  
  
    ```vb
    monString = "service:mexaddress=http://localhost/ServiceModelSamples/Service.svc/MEX"  
    monString = monString + ", address=http://localhost/ServiceModelSamples/Service.svc"  
    monString = monString + ", contract=ICalculator, contractNamespace=http://Microsoft.ServiceModel.Samples"  
    monString = monString + ", binding=WSHttpBinding_ICalculator, bindingNamespace=http://Microsoft.ServiceModel.Samples"  
  
    Set calc = GetObject(monString)  
    MsgBox calc.Add(3, 4)  
    ```  
  
3. 运行 Visual Basic 应用程序或脚本。  
  
    > [!NOTE]
    > 您所调用的服务必须公开标记的 Mex 终结点，以便能够从服务中读取元数据。 有关详细信息，请参阅 [如何：使用配置文件发布服务的元数据](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)。  
  
    > [!NOTE]
    > 如果标记格式不正确，或如果服务不可用，则对 `GetObject` 的调用将返回一个错误，指示“语法无效”。  如果您收到此错误，请确保所使用的标记正确无误且服务可用。  
  
## <a name="see-also"></a>另请参阅

- [如何：使用未注册的 Windows Communication Foundation 服务名字对象](use-the-wcf-service-moniker-without-registration.md)
- [如何：将服务名字对象用于 WSDL 协定](how-to-use-a-service-moniker-with-wsdl-contracts.md)
