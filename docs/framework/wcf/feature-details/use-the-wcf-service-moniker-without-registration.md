---
title: 如何：使用未注册的 Windows Communication Foundation 服务名字对象
ms.date: 03/30/2017
helpviewer_keywords:
- COM [WCF], service monikers without registration
ms.assetid: ee3cf5c0-24f0-4ae7-81da-73a60de4a1a8
ms.openlocfilehash: 41761313fae68a1a348a73f104e21dc19e07eb65
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293502"
---
# <a name="how-to-use-the-windows-communication-foundation-service-moniker-without-registration"></a>如何：使用未注册的 Windows Communication Foundation 服务名字对象

若要连接到 Windows Communication Foundation (WCF) 服务并与之通信，WCF 客户端应用程序必须具有服务地址、绑定配置和服务协定的详细信息。  
  
 WCF 服务标记通常会在先前注册所需的属性类型之前获得所需的协定，但可能会出现这种情况。 不进行注册时，标记可以通过使用 `wsdl` 参数、通过元数据交换或通过使用 `mexAddress` 参数来获取 Web 服务定义语言 (WSDL) 文档形式的协定定义。  
  
 这将启用诸如分发 Excel 电子表格（其中，某些单元格值通过 Web 服务交互进行计算）之类的方案。 在此方案中，在可能打开该文档的所有客户端上注册服务协定程序集也许并不可行。 `wsdl` 参数或 `mexAddress` 参数将启用独立的解决方案。  
  
> [!NOTE]
> 必须使用相互身份验证来防止篡改或伪造请求和响应。 具体而言，就是保证要做出响应的元数据交换终结点是预期的可信方，这一点对于客户端相当重要。  
  
## <a name="example"></a>示例  

 此示例演示具有 MEX 协定的服务标记的用法。 具有以下协定的服务通过 wsHttpBinding 公开。  
  
```csharp
using System.ServiceModel;  
  
// ...
  
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Demo")]  
public interface IAffiliate  
{  
    [OperationContract]  
    bool NewAffiliate(string ID, string company, string fullname, string accountsCode);  
    [OperationContract]  
    bool RemoveAffiliate(string ID);  
    [OperationContract]  
    double RevenueCheckMonthly(ref string ID);  
    [OperationContract]  
    double RevenueCheckTotal(ref string ID);  
}  
```  
  
 若要为远程服务构造 WCF 客户端，可以使用以下示例标记字符串。  
  
```
service4:mexAddress="http://servername/Affiliates/service.svc/mex",  
address="http://servername/Affiliates/service.svc",  
contract=IAffiliate, contractNamespace=http://Microsoft.ServiceModel.Demo,  
binding=WSHttpBinding_IAffiliate, bindingNamespace=http://tempuri.org/  
```  
  
 在客户端应用程序执行过程中，客户端将使用提供的 `WS-MetadataExchange` 执行 `mexAddress`。 这可能会返回多个服务的地址、绑定和协定详细信息。 `address`、`contract`、`contractNamespace`、`binding` 和 `bindingNamespace` 参数用于标识所需的服务。 匹配这些参数后，名字对象将构造具有相应协定定义的 WCF 客户端，然后就可以使用 WCF 客户端发出调用，就像类型化协定一样。  
  
> [!NOTE]
> 如果标记格式不正确或者服务不可用，则对 `GetObject` 的调用将返回一个错误，指示“语法无效”。 如果您收到此错误，请确保所使用的标记正确无误且服务可用。  
  
## <a name="see-also"></a>另请参阅

- [如何：注册和配置服务名字对象](how-to-register-and-configure-a-service-moniker.md)
