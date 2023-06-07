---
title: 为 Windows Communication Foundation 服务配置绑定
description: 了解如何通过编辑系统下的项在部署时为 WCF 应用程序配置绑定。System.servicemodel 元素。
ms.date: 03/30/2017
helpviewer_keywords:
- binding configuration [WCF]
ms.assetid: 99a85fd8-f7eb-4a84-a93e-7721b37d415c
ms.openlocfilehash: 60ab04764851ef82a43d5c2050d3bac7b56ce551
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96266722"
---
# <a name="configuring-bindings-for-windows-communication-foundation-services"></a>为 Windows Communication Foundation 服务配置绑定

创建应用程序时，您经常需要将一些决策交给管理员在部署应用程序后制定。 例如，通常没有办法提前知道服务地址或统一资源标识符 (URI)。 最好允许管理员在创建服务后指定地址，而不是对地址进行硬编码。 这种灵活性是通过配置实现的。  
  
> [!NOTE]
> 使用配置的 [元数据实用工具工具 ( # A0) ](servicemodel-metadata-utility-tool-svcutil-exe.md) 与 `/config` 开关一起快速创建配置文件。  
  
## <a name="major-sections"></a>主要部分  

 Windows Communication Foundation (WCF) 配置方案包含以下三个主要部分 (`serviceModel` 、 `bindings` 和 `services`) ：  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <bindings>  
        </bindings>  
        <services>  
        </services>  
        <behaviors>  
        </behaviors>  
    </system.serviceModel>  
</configuration>  
```  
  
### <a name="servicemodel-elements"></a>ServiceModel 元素  

 你可以使用由元素限定的部分 `system.ServiceModel` 来配置具有一个或多个终结点的服务类型以及服务的设置。 然后，可以为每个终结点配置地址、协定以及绑定。 有关终结点的详细信息，请参阅 [创建终结点概述](endpoint-creation-overview.md)。 如果未指定任何终结点，则运行时添加默认终结点。 有关默认终结点、绑定和行为的详细信息，请参阅[简化配置](simplified-configuration.md)和 [WCF 服务的简化配置](./samples/simplified-configuration-for-wcf-services.md)。  
  
 绑定指定传输（HTTP、TCP、管道、消息队列）和协议（安全性、可靠性、事务流）。绑定由绑定元素组成，其中的每个元素都指定终结点与世界的通信方式的一个方面。  
  
 例如，指定 [\<basicHttpBinding>](../configure-apps/file-schema/wcf/basichttpbinding.md) 元素指示使用 HTTP 作为终结点的传输。 这用于在使用此终结点的服务打开时在运行时连接终结点。  
  
 有两种绑定：预定义绑定和自定义绑定。 预定义绑定包含在常见方案中使用的元素的有用组合。 有关 WCF 提供的预定义绑定类型的列表，请参阅 [系统提供的绑定](system-provided-bindings.md)。 如果没有任何预定义绑定集合具有服务应用程序所需的功能的正确组合，那么你可以构造自定义绑定来满足应用程序的需求。 有关自定义绑定的详细信息，请参阅 [\<customBinding>](../configure-apps/file-schema/wcf/custombinding.md) 。  
  
 以下四个示例演示了用于设置 WCF 服务的最常见的绑定配置。  
  
#### <a name="specifying-an-endpoint-to-use-a-binding-type"></a>指定终结点以使用绑定类型  

 第一个示例说明如何指定一个配置了地址、协定和绑定的终结点。  
  
```xml  
<service name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">  
  <!-- This section is optional with the default configuration introduced  
       in .NET Framework 4. -->  
  <endpoint
      address="/HelloWorld2/"  
      contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
      binding="basicHttpBinding" />
</service>  
```  
  
 在本示例中，`name` 属性表示使用该配置的服务类型。 当您用 `HelloWorld` 协定通过代码中创建服务时，它将以示例配置中定义的所有终结点进行初始化。 如果程序集仅实现一个服务协定，则 `name` 可以省略属性，因为该服务使用唯一可用的类型。 该属性采用字符串，该字符串必须遵循以下格式：`Namespace.Class, AssemblyName, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`  
  
 `address` 属性指定其他终结点用于与该服务通信的 URI。 该 URI 可以是绝对路径，也可以是相对路径。 如果提供的是相对地址，则需要主机提供适合于绑定中所使用的传输方案的基址。 如果未配置地址，则假定基址为该终结点的地址。  
  
 `contract` 属性指定该终结点所公开的协定。 服务实现类型必须实现该协定类型。 如果服务实现所实现的是单个协定类型，则可以省略此属性。  
  
 `binding` 属性选择要用于此特定终结点的预定义或自定义绑定。 不显式选择绑定的终结点使用默认绑定选择，即 `BasicHttpBinding`。  
  
#### <a name="modifying-a-predefined-binding"></a>修改预定义绑定  

 在下面的示例中，修改了预定义绑定。 然后可以将该绑定用于配置服务中的任意终结点。 可通过将 <xref:System.ServiceModel.Configuration.IBindingConfigurationElement.ReceiveTimeout%2A> 值设置为 1 秒的方式来修改绑定。 请注意，该属性返回一个 <xref:System.TimeSpan> 对象。  
  
 在绑定部分中可以找到该更改的绑定。 现在，通过在 `binding` 元素中设置 `endpoint` 特性，可以在创建任何终结点时使用该更改的绑定。  
  
> [!NOTE]
> 如果为该绑定指定了特定名称，则服务终结点中指定的 `bindingConfiguration` 必须与其相匹配。  
  
```xml  
<service name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">  
  <endpoint
      address="/HelloWorld2/"  
      contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
      binding="basicHttpBinding" />
</service>  
<bindings>  
    <basicHttpBinding
        receiveTimeout="00:00:01"  
    />  
</bindings>  
```  
  
## <a name="configuring-a-behavior-to-apply-to-a-service"></a>配置要应用于服务的行为  

 在下面的示例中，为该服务类型配置了一个特定的行为。 `ServiceMetadataBehavior`元素用于启用 "使用的[元数据实用工具" 工具 ( # A0) ](servicemodel-metadata-utility-tool-svcutil-exe.md)查询服务并从元数据生成 Web 服务描述语言 (WSDL) 文档。  
  
> [!NOTE]
> 如果为该行为指定了特定名称，则服务或终结点部分中指定的 `behaviorConfiguration` 必须与其相匹配。  
  
```xml  
<behaviors>  
    <behavior>  
        <ServiceMetadata httpGetEnabled="true" />
    </behavior>  
</behaviors>  
<services>  
    <service
       name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">
       <endpoint
          address="http://computer:8080/Hello"  
          contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
          binding="basicHttpBinding" />
    </service>  
</services>  
```  
  
 上面的配置使客户端可以调用并获取“HelloWorld”类型的服务的元数据。  
  
 `svcutil /config:Client.exe.config http://computer:8080/Hello?wsdl`  
  
## <a name="specifying-a-service-with-two-endpoints-using-different-binding-values"></a>使用不同的绑定值指定具有两个终结点的服务  

 这是最后一个示例，在本示例中，为 `HelloWorld` 服务类型配置了两个终结点。 每个端点都使用同一绑定类型的不同自定义  `bindingConfiguration` 属性， (每个端点修改 `basicHttpBinding`) 。  
  
```xml  
<service name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">  
    <endpoint  
        address="http://computer:8080/Hello1"  
        contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
        binding="basicHttpBinding"  
        bindingConfiguration="shortTimeout" />
    <endpoint  
        address="http://computer:8080/Hello2"  
        contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
        binding="basicHttpBinding"  
        bindingConfiguration="Secure" />
</service>  
<bindings>  
    <basicHttpBinding
        name="shortTimeout"  
        timeout="00:00:00:01"
     />  
     <basicHttpBinding
        name="Secure">  
        <Security mode="Transport" />  
     </basicHttpBinding>
</bindings>  
```  
  
 通过添加 `protocolMapping` 部分并按下面的示例所示配置绑定，可以使用默认配置获得相同的行为。  
  
```xml  
<protocolMapping>  
    <add scheme="http" binding="basicHttpBinding" bindingConfiguration="shortTimeout" />  
    <add scheme="https" binding="basicHttpBinding" bindingConfiguration="Secure" />  
</protocolMapping>  
<bindings>  
    <basicHttpBinding
        name="shortTimeout"  
        timeout="00:00:00:01"
     />  
     <basicHttpBinding
        name="Secure" />  
        <Security mode="Transport" />  
</bindings>  
```  
  
## <a name="see-also"></a>另请参阅

- [简化配置](simplified-configuration.md)
- [系统提供的绑定](system-provided-bindings.md)
- [终结点创建概述](endpoint-creation-overview.md)
- [使用绑定配置服务和客户端](using-bindings-to-configure-services-and-clients.md)
