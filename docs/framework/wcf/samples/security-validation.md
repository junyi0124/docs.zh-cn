---
title: 安全性验证
ms.date: 03/30/2017
ms.assetid: 48dcd496-0c4f-48ce-8b9b-0e25b77ffa58
ms.openlocfilehash: 1260aaa756e7be33ce2aa1bcce5fc79be553c990
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262614"
---
# <a name="security-validation"></a>安全验证

此示例演示如何使用自定义行为来验证计算机上的服务，以确保服务符合特定条件。 在此示例中，自定义行为通过以下方法验证服务：扫描服务上的每个终结点，并查看这些终结点是否包含安全的绑定元素。 此示例基于 [入门](getting-started-sample.md)。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
## <a name="endpoint-validation-custom-behavior"></a>终结点验证自定义行为  

 通过将用户代码添加到 `Validate` 接口中包含的 <xref:System.ServiceModel.Description.IServiceBehavior> 方法，可以为某个服务或终结点指定自定义行为，以便执行用户定义的操作。 下面的代码用于遍历服务中包含的每个终结点，这将在其绑定集合中搜索安全的绑定。  
  
```csharp
public void Validate(ServiceDescription serviceDescription,
                                       ServiceHostBase serviceHostBase)  
{  
    // Loop through each endpoint individually, gathering their
    // binding elements.  
    foreach (ServiceEndpoint endpoint in serviceDescription.Endpoints)  
    {  
        secureElementFound = false;  
  
        // Retrieve the endpoint's binding element collection.  
        BindingElementCollection bindingElements =
            endpoint.Binding.CreateBindingElements();  
  
        // Look to see if the binding elements collection contains any
        // secure binding elements. Transport, Asymmetric, and Symmetric
        // binding elements are all derived from SecurityBindingElement.  
        if ((bindingElements.Find<SecurityBindingElement>() != null) || (bindingElements.Find<HttpsTransportBindingElement>() != null) || (bindingElements.Find<WindowsStreamSecurityBindingElement>() != null) || (bindingElements.Find<SslStreamSecurityBindingElement>() != null))  
        {  
            secureElementFound = true;  
        }  
  
    // Send a message to the system event viewer when an endpoint is deemed insecure.  
    if (!secureElementFound)  
        throw new Exception(System.DateTime.Now.ToString() + ": The endpoint \"" + endpoint.Name + "\" has no secure bindings.");  
    }  
}  
```  
  
 通过将下面的代码添加到 Web.config 文件可以添加服务可识别的 `serviceValidate` 行为扩展。  
  
```xml  
<system.serviceModel>  
    <extensions>  
        <behaviorExtensions>  
            <add name="endpointValidate" type="Microsoft.ServiceModel.Samples.EndpointValidateElement, endpointValidate, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null" />  
        </behaviorExtensions>  
    </extensions>
    ...
</system.serviceModel>
```  
  
 将行为扩展添加到服务之后，现在即可将 `endpointValidate` 行为添加到 Web.config 文件的行为列表中，从而添加到服务中。  
  
```xml  
<behaviors>  
    <serviceBehaviors>  
        <behavior name="CalcServiceSEB1">  
            <serviceMetadata httpGetEnabled="true" />  
            <endpointValidate />  
        </behavior>  
    </serviceBehaviors>  
</behaviors>  
```  
  
 添加到 Web.config 文件的行为及其扩展可将行为应用到各个服务，而添加到 Machine.config 文件的行为及其扩展则将行为应用到计算机上的每个活动服务。  
  
> [!NOTE]
> 向所有服务中添加行为时，建议在进行任何更改之前备份 Machine.config 文件。  
  
 现在运行此示例的 client\bin 目录中提供的客户端。 引发异常，并出现以下消息： "无法激活请求的服务 http://localhost/servicemodelsamples/service.svc 。" 这是预期的行为，因为终结点验证行为认为某个终结点不安全，并阻止服务启动。 该行为还会引发一个内部异常，以描述哪个终结点不安全，并在“System.ServiceModel 4.0.0.0”源和“WebHost”类别下的系统事件查看器中写入一则消息。 还可以在此示例中打开对服务的跟踪。 这样可以使用户查看终结点验证行为引发的异常，方法是：使用服务跟踪查看器工具打开生成的服务跟踪。  
  
### <a name="view-failed-endpoint-validation-exception-messages-in-the-event-viewer"></a>查看事件查看器中失败的终结点验证异常消息  
  
1. 单击 " **开始** " 菜单，然后选择 " **运行 ...**"。  
  
2. 键入 `eventvwr` ，然后单击 **“确定”** 。  
  
3. 在事件查看器 "窗口中，单击" **应用程序**"。  
  
4. 在 **应用程序** 窗口中的 "WebHost" 类别下双击最近添加的 "system.servicemodel 4.0.0.0" 事件，以查看不安全的终结点消息。  
  
## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\ServiceValidation`  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
