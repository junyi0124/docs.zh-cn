---
title: 使用配置文件配置服务
description: 了解 WCF 服务的配置文件如何提供在部署过程中提供终结点和服务行为数据的灵活性。
ms.date: 03/30/2017
helpviewer_keywords:
- configuring services [WCF]
ms.assetid: c9c8cd32-2c9d-4541-ad0d-16dff6bd2a00
ms.openlocfilehash: 25a6891564054878e7bdf7f43d431547ea1dee6c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253344"
---
# <a name="configuring-services-using-configuration-files"></a>使用配置文件配置服务

通过使用配置文件配置 Windows Communication Foundation (WCF) 服务，可在部署时（而不是在设计时）提供终结点和服务行为数据的灵活性。 本主题概述了当前可用的主要技术。  
  
 WCF 服务可使用 .NET Framework 配置技术进行配置。 最常见的情况是，XML 元素添加到承载 WCF 服务的 Internet Information Services (IIS) 站点的 Web.config 文件中。 通过这些元素，可以逐台计算机更改详细信息，例如终结点地址（用于与服务进行通信的实际地址）。 此外，WCF 还包含多个系统提供的元素，可用于快速选择服务的最基本功能。 从 .NET Framework 4 开始，WCF 附带了新的默认配置模型，该模型可简化 WCF 配置要求。 如果你没有为特定服务提供任何 WCF 配置，则运行时将自动使用一些标准终结点和默认绑定/行为配置你的服务。 实际上，编写配置是 WCF 应用程序编程的主要部分。  
  
 有关详细信息，请参阅 [配置服务的绑定](configuring-bindings-for-wcf-services.md)。 有关最常使用的元素的列表，请参阅 [系统提供的绑定](system-provided-bindings.md)。 有关默认终结点、绑定和行为的详细信息，请参阅[简化配置](simplified-configuration.md)和 [WCF 服务的简化配置](./samples/simplified-configuration-for-wcf-services.md)。  
  
> [!IMPORTANT]
> 在部署并行方案（其中部署了服务的两个不同版本）时，必须指定配置文件中引用的程序集的部分名称。 这是因为配置文件将在服务的所有版本间共享，并可在不同版本的 .NET Framework 下运行。  
  
## <a name="systemconfiguration-webconfig-and-appconfig"></a>System.Configuration：Web.config 和 App.config  

 WCF 使用 .NET Framework 的 System.Configu 配置系统。  
  
 在 Visual Studio 中配置服务时，请使用 Web.config 文件或 App.config 文件指定设置。 配置文件名称的选择由为服务选择的宿主环境确定。 如果正在使用 IIS 来承载服务，则使用 Web.config 文件。 如果正在使用任何其他宿主环境，则使用 App.config 文件。  
  
 在 Visual Studio 中，使用名为 App.config 的文件创建最终的配置文件。 实际用于配置的最终名称取决于程序集名称。 例如，名为“Cohowinery.exe”的程序集具有的最终配置文件名称为“Cohowinery.exe.config”。 但是，只需要修改 App.config 文件。 在编译时，对该文件所做的更改会自动应用于最终应用程序配置文件。  
  
 在使用 App.config 文件的过程中，当应用程序启动并应用配置时，文件配置系统会将 App.config 文件与 Machine.config 文件的内容合并。 此机制允许在 Machine.config 文件中定义计算机范围的设置。 可以使用 App.config 文件重写 Machine.config 文件的设置；也可以锁定 Machine.config 文件中的设置以应用它们。 对于 Web.config，配置系统会将应用程序目录之下的所有目录中的 Web.config 文件合并到要应用的配置中。 有关配置和设置优先级的详细信息，请参阅命名空间中的主题 <xref:System.Configuration> 。  
  
## <a name="major-sections-of-the-configuration-file"></a>配置文件的主要部分  

 配置文件中的主要部分包括以下元素。  
  
```xml  
<system.ServiceModel>  
  
   <services>  
   <!-- Define the service endpoints. This section is optional in the new  
    default configuration model in .NET Framework 4. -->  
      <service>  
         <endpoint/>  
      </service>  
   </services>  
  
   <bindings>  
   <!-- Specify one or more of the system-provided binding elements,  
    for example, <basicHttpBinding> -->
   <!-- Alternatively, <customBinding> elements. -->  
      <binding>  
      <!-- For example, a <BasicHttpBinding> element. -->  
      </binding>  
   </bindings>  
  
   <behaviors>  
   <!-- One or more of the system-provided or custom behavior elements. -->  
      <behavior>  
      <!-- For example, a <throttling> element. -->  
      </behavior>  
   </behaviors>  
  
</system.ServiceModel>  
```  
  
> [!NOTE]
> 绑定部分和行为部分是可选的，只在需要时才包括。  
  
### <a name="the-services-element"></a>\<services>元素  

 `services` 元素包含应用程序承载的所有服务的规范。 从 .NET Framework 4 的简化配置模型开始，此部分是可选的。  
  
 [\<services>](../configure-apps/file-schema/wcf/services.md)  
  
### <a name="the-service-element"></a>\<service>元素  

 每个服务都具有以下属性：  
  
- `name`. 指定提供服务协定的实现的类型。 这是完全限定名称，其中包含命名空间、句点和类型名称。 例如，`"MyNameSpace.myServiceType"`。  
  
- `behaviorConfiguration`. 指定一个在 `behavior` 元素中找到的 `behaviors` 元素的名称。 指定的行为控制操作，例如服务是否允许模拟。 如果它的值是空的，或者未提供任何 `behaviorConfiguration` ，则向服务中添加默认服务行为集。  
  
- [\<service>](../configure-apps/file-schema/wcf/service.md)  
  
### <a name="the-endpoint-element"></a>\<endpoint>元素  

 每个终结点都需要以下属性表示的地址、绑定和协定：  
  
- `address`. 指定服务的统一资源标识符 (URI)，它可以是一个绝对地址，或是一个相对于服务基址给定的地址。 如果设置为空字符串，则指示在创建服务的 <xref:System.ServiceModel.ServiceHost> 时，终结点在指定的基址上可用。  
  
- `binding`. 通常，指定一个类似 <xref:System.ServiceModel.WSHttpBinding>的系统提供的绑定，但也可以指定一个用户定义的绑定。 指定的绑定确定传输协议类型、安全和使用的编码，以及是否支持或启用可靠会话、事务或流。  
  
- `bindingConfiguration`. 如果必须修改绑定的默认值，则可通过在 `binding` 元素中配置相应的 `bindings` 元素来执行此操作。 此属性应赋予与用于更改默认值的 `name` 元素的 `binding` 属性相同的值。 如果未提供任何名称，或者在绑定中未指定任何 `bindingConfiguration` ，则在终结点中使用绑定类型的默认绑定。  
  
- `contract`. 指定定义协定的接口。 这是在由 `name` 元素的 `service` 属性指定的公共语言运行库 (CLR) 类型中实现的接口。  
  
- [\<endpoint>](../configure-apps/file-schema/wcf/endpoint-element.md)  
  
### <a name="the-bindings-element"></a>\<bindings>元素  

 `bindings` 元素包含可由任何服务中定义的任何终结点使用的所有绑定的规范。  
  
 [\<bindings>](../configure-apps/file-schema/wcf/bindings.md)  
  
### <a name="the-binding-element"></a>\<binding>元素  

 在 `binding` 元素中包含的 `bindings` 元素可以是系统提供的绑定之一（请参阅 [System-Provided Bindings](system-provided-bindings.md)），也可以是自定义绑定（请参阅 [Custom Bindings](./extending/custom-bindings.md)）。 `binding` 元素具有 `name` 属性，此属性将绑定与 `bindingConfiguration` 元素的 `endpoint` 属性中指定的终结点相关联。 如果未指定任何名称，则该绑定对应于该绑定类型的默认值。  
  
有关配置服务和客户端的详细信息，请参阅 [配置 WCF 服务](configuring-services.md)。
  
 [\<binding>](../configure-apps/file-schema/wcf/bindings.md)  
  
### <a name="the-behaviors-element"></a>\<behaviors>元素  

 这是定义服务行为的 `behavior` 元素的容器元素。  
  
 [\<behaviors>](../configure-apps/file-schema/wcf/behaviors.md)  
  
### <a name="the-behavior-element"></a>\<behavior>元素  

 每个 `behavior` 元素都由属性标识， `name` 并提供系统提供的行为，如 <`throttling`> 或自定义行为。 如果未提供任何名称，则该行为元素对应于默认服务或终结点行为。  
  
 [\<behavior>](../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md)  
  
## <a name="how-to-use-binding-and-behavior-configurations"></a>如何使用绑定和行为配置  

 使用 WCF，可以在配置中使用引用系统轻松地在终结点之间共享配置。 与绑定相关的配置值在 `bindingConfiguration` 部分的 `<binding>` 元素中进行分组，而不是直接将配置值分配到终结点。 绑定配置是一组命名的绑定设置。 然后，终结点可以通过名称来引用 `bindingConfiguration` 。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
 <system.serviceModel>  
  <bindings>  
    <basicHttpBinding>  
     <binding name="myBindingConfiguration1" closeTimeout="00:01:00" />  
     <binding name="myBindingConfiguration2" closeTimeout="00:02:00" />  
     <binding closeTimeout="00:03:00" />  <!-- Default binding for basicHttpBinding -->  
    </basicHttpBinding>  
     </bindings>  
     <services>  
      <service name="MyNamespace.myServiceType">  
       <endpoint
          address="myAddress" binding="basicHttpBinding"
          bindingConfiguration="myBindingConfiguration1"  
          contract="MyContract"  />  
       <endpoint
          address="myAddress2" binding="basicHttpBinding"
          bindingConfiguration="myBindingConfiguration2"  
          contract="MyContract" />  
       <endpoint
          address="myAddress3" binding="basicHttpBinding"
          contract="MyContract" />  
       </service>  
      </services>  
    </system.serviceModel>  
</configuration>  
```  
  
 在 `name` 元素中设置 `bindingConfiguration` 的 `<binding>` 。 `name`必须是绑定类型（在本例中为[<basicHttpBinding \> ](../configure-apps/file-schema/wcf/basichttpbinding.md)）范围内的唯一字符串，或者是用于引用默认绑定的空值。 通过将 `bindingConfiguration` 属性设置为此字符串，终结点链接到该配置。  
  
 以相同方式实现 `behaviorConfiguration` ，如以下示例中所示。  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="myBehavior">  
           <callbackDebug includeExceptionDetailInFaults="true" />  
         </behavior>  
      </endpointBehaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="true" />  
        </behavior>  
      </serviceBehaviors>  
  
    </behaviors>  
    <services>  
     <service name="NewServiceType">  
       <endpoint
          address="myAddress3" behaviorConfiguration="myBehavior"  
          binding="basicHttpBinding"  
          contract="MyContract" />  
      </service>  
    </services>  
   </system.serviceModel>  
</configuration>  
```  
  
 请注意，向服务中添加了默认服务行为集。 此系统允许终结点共享公共配置而不用重新定义设置。 如果需要计算机范围的作用域，请在 Machine.config 中创建绑定或行为配置。所有 App.config 文件中都提供了配置设置。 通过 [Configuration Editor Tool (SvcConfigEditor.exe)](configuration-editor-tool-svcconfigeditor-exe.md) 可以很方便地创建配置。  
  
## <a name="behavior-merge"></a>行为合并  

 当您需要统一使用一组公共行为时，利用行为合并功能，可更加轻松地管理行为。 此功能允许您在配置层次结构的各个层上指定行为，并使服务能够从配置层次结构的多个层继承行为。 为了演示此功能的工作方式，假定您的 IIS 中包含以下虚拟目录布局：  
  
 `~\Web.config~\Service.svc~\Child\Web.config~\Child\Service.svc`
  
 `~\Web.config`文件包含以下内容：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceDebug includeExceptionDetailInFaults="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 具有一个位于 ~\Child\Web.config 并包含以下内容的子 Web.config：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 位于 ~\Child\Service.svc 的服务的行为如同该服务具有 serviceDebug 和 serviceMetadata 行为一样。 位于 ~\Service.svc 的服务只具有 serviceDebug 行为。 发生的情况是，名称相同的两个行为集合（此情况下为空字符串）将合并。  
  
 你还可以通过使用标记来清除行为集合 \<clear> ，并使用标记从集合中移除各个行为 \<remove> 。 例如，子服务中的以下两个配置结果只具有 serviceMetadata 行为：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <remove name="serviceDebug"/>  
          <serviceMetadata httpGetEnabled="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <clear/>  
          <serviceMetadata httpGetEnabled="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 对无名称行为集合执行行为合并（如上所示），并对命名行为集合执行行为合并。  
  
 行为合并在 IIS 宿主环境中工作，在该环境中 Web.config 文件与根 Web.config 文件和 machine.config 分层。但它也适用于应用程序环境，其中 machine.config 可以与 App.config 文件合并。  
  
 行为合并适用于配置中的终结点行为和服务行为。  
  
 如果子行为集合包含一个已显示在父行为集合中的行为，则子行为将重写父行为。 因此，如果父行为集合具有 `<serviceMetadata httpGetEnabled="False" />` ，而子行为集合具有 `<serviceMetadata httpGetEnabled="True" />` ，则子行为将重写行为集合中的父行为并且 httpGetEnabled 将为 "true"。  
  
## <a name="see-also"></a>另请参阅

- [简化配置](simplified-configuration.md)
- [配置 WCF 服务](configuring-services.md)
- [\<service>](../configure-apps/file-schema/wcf/service.md)
- [\<binding>](../configure-apps/file-schema/wcf/bindings.md)
