---
title: 配置和元数据支持
ms.date: 03/30/2017
ms.assetid: 27c240cb-8cab-472c-87f8-c864f4978758
ms.openlocfilehash: fe7de6fddeccbc83bc81eea69c27c7da5df5c8c3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257992"
---
# <a name="configuration-and-metadata-support"></a>配置和元数据支持

本主题说明如何启用配置和元数据对绑定和绑定元素的支持。  
  
## <a name="overview-of-configuration-and-metadata"></a>配置和元数据概述  

 本主题讨论以下任务，这些任务是 " [开发通道](developing-channels.md) " 任务列表中的可选项1、2和4。  
  
- 启用配置文件对绑定元素的支持。  
  
- 启用配置文件对绑定的支持。  
  
- 导出绑定元素的 WSDL 和策略断言。  
  
- 标识 WSDL 和策略断言以插入或配置你的绑定或绑定元素。  
  
 有关创建用户定义的绑定和绑定元素的信息，请分别参阅 [创建 User-Defined 绑定](creating-user-defined-bindings.md) 和 [创建 BindingElement](creating-a-bindingelement.md)。  
  
## <a name="adding-configuration-support"></a>添加配置支持  

 若要启用配置文件对通道的支持，必须实现两个配置节：<xref:System.ServiceModel.Configuration.BindingElementExtensionElement?displayProperty=nameWithType>（启用配置对绑定元素的支持）以及 <xref:System.ServiceModel.Configuration.StandardBindingElement?displayProperty=nameWithType> 和 <xref:System.ServiceModel.Configuration.StandardBindingCollectionElement%602?displayProperty=nameWithType>（启用配置对绑定的支持）。  
  
 更简单的方法是使用 [ConfigurationCodeGenerator](../samples/configurationcodegenerator.md) 示例工具为绑定和绑定元素生成配置代码。  
  
### <a name="extending-bindingelementextensionelement"></a>扩展 BindingElementExtensionElement  

 下面的示例代码取自 [Transport： UDP](../samples/transport-udp.md) 示例。 `UdpTransportElement` 是一个 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement>，它向配置系统公开 `UdpTransportBindingElement`。 通过若干基本重写，此示例定义配置节名称、绑定元素的类型以及如何创建绑定元素。 然后，用户可以按如下方式注册配置文件中的扩展节。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <extensions>  
      <bindingElementExtensions>  
      <add name="udpTransport" type="Microsoft.ServiceModel.Samples.UdpTransportElement, UdpTransport" />  
      </bindingElementExtensions>  
    </extensions>  
  </system.serviceModel>  
</configuration>  
```  
  
 可以从自定义绑定来引用该扩展以将 UDP 用作传输协议。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <bindings>  
      <customBinding>  
       <binding configurationName="UdpCustomBinding">  
         <udpTransport/>  
       </binding>  
      </customBinding>  
    </bindings>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="adding-configuration-for-a-binding"></a>为绑定添加配置  

 `SampleProfileUdpBindingCollectionElement` 节是一个 <xref:System.ServiceModel.Configuration.StandardBindingCollectionElement%602>，它向配置系统公开 `SampleProfileUdpBinding`。 批量实现委派给从 `SampleProfileUdpBindingConfigurationElement` 派生的 <xref:System.ServiceModel.Configuration.StandardBindingElement>。 `SampleProfileUdpBindingConfigurationElement`具有与的属性相对应的属性 `SampleProfileUdpBinding` ，以及要从绑定中映射的函数 `ConfigurationElement` 。 最后，在 `OnApplyConfiguration` 中重写 `SampleProfileUdpBinding` 方法，如下面的代码示例中所示。  
  
```csharp
protected override void OnApplyConfiguration(string configurationName)  
{  
            if (binding == null)  
                throw new ArgumentNullException("binding");  
  
            if (binding.GetType() != typeof(SampleProfileUdpBinding))  
            {  
                var expectedType = typeof(SampleProfileUdpBinding).AssemblyQualifiedName;
                var typePassedIn = binding.GetType().AssemblyQualifiedName;
                throw new ArgumentException($"Invalid type for binding. Expected type: {expectedType}. Type passed in: {typePassedIn}.");  
            }  
            SampleProfileUdpBinding udpBinding = (SampleProfileUdpBinding)binding;  
  
            udpBinding.OrderedSession = this.OrderedSession;  
            udpBinding.ReliableSessionEnabled = this.ReliableSessionEnabled;  
            udpBinding.SessionInactivityTimeout = this.SessionInactivityTimeout;  
            if (this.ClientBaseAddress != null)  
                   udpBinding.ClientBaseAddress = ClientBaseAddress;  
}  
```  
  
 若要使用配置系统注册此处理程序，请将下一节添加到相关的配置文件中。  
  
```xml  
<configuration>  
  <configSections>  
     <sectionGroup name="system.serviceModel">  
         <sectionGroup name="bindings">  
                 <section name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
         </sectionGroup>  
     </sectionGroup>  
  </configSections>  
</configuration>  
```  
  
 然后，可以从 [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) 配置节引用它。  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <endpoint configurationName="calculator"  
                address="soap.udp://localhost:8001/"
                bindingConfiguration="CalculatorServer"  
                binding="sampleProfileUdpBinding"
                contract= "Microsoft.ServiceModel.Samples.ICalculatorContract">  
      </endpoint>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="adding-metadata-support-for-a-binding-element"></a>为绑定元素添加元数据支持  

 若要将通道集成到元数据系统中，系统必须支持策略的导入和导出。 这将允许 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 等工具生成绑定元素的客户端。  
  
### <a name="adding-wsdl-support"></a>添加 WSDL 支持  

 绑定中的传输绑定元素负责导出和导入元数据中的寻址信息。 当使用 SOAP 绑定时，传输绑定元素还应在元数据中导出正确的传输 URI。 下面的示例代码取自 [Transport： UDP](../samples/transport-udp.md) 示例。  
  
#### <a name="wsdl-export"></a>WSDL 导出  

 若要导出寻址信息，需要 `UdpTransportBindingElement` 实现 <xref:System.ServiceModel.Description.IWsdlExportExtension?displayProperty=nameWithType> 接口。 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportEndpoint%2A?displayProperty=nameWithType> 方法将正确的寻址信息添加到 WSDL 端口中。  
  
```csharp  
if (context.WsdlPort != null)  
{  
    AddAddressToWsdlPort(context.WsdlPort, context.Endpoint.Address, encodingBindingElement.MessageVersion.Addressing);  
}  
```  
  
 当终结点使用 SOAP 绑定时，`UdpTransportBindingElement` 方法的 <xref:System.ServiceModel.Description.IWsdlExportExtension.ExportEndpoint%2A> 实现也会导出传输 URI：  
  
```csharp  
WsdlNS.SoapBinding soapBinding = GetSoapBinding(context, exporter);  
if (soapBinding != null)  
{  
    soapBinding.Transport = UdpPolicyStrings.UdpNamespace;  
}  
```  
  
#### <a name="wsdl-import"></a>WSDL 导入  

 若要扩展 WSDL 导入系统以处理导入地址，请将以下配置添加到 Svcutil.exe 的配置文件，如 Svcutil.exe.config 文件所示：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <wsdlImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </wsdlImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 当运行 Svcutil.exe 时，有两个选项可用来获取 Svcutil.exe 以加载 WSDL 导入扩展：  
  
1. 使用/SvcutilConfig：将 Svcutil.exe 点到配置文件 \<file> 。  
  
2. 将配置节添加到与 Svcutil.exe 处于同一目录的 Svcutil.exe.config 中。  
  
 `UdpBindingElementImporter` 类型实现 <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> 接口。 `ImportEndpoint` 方法从 WSDL 端口导入地址：  
  
```csharp  
BindingElementCollection bindingElements = context.Endpoint.Binding.CreateBindingElements();  
TransportBindingElement transportBindingElement = bindingElements.Find<TransportBindingElement>();  
if (transportBindingElement is UdpTransportBindingElement)  
{  
    ImportAddress(context);  
}  
```  
  
### <a name="adding-policy-support"></a>添加策略支持  

 自定义绑定元素可以在 WSDL 绑定中为服务终结点导出策略断言以表示该绑定元素的功能。 下面的示例代码取自 [Transport： UDP](../samples/transport-udp.md) 示例。  
  
#### <a name="policy-export"></a>策略导出  

 `UdpTransportBindingElement`实现 <xref:System.ServiceModel.Description.IPolicyExportExtension?displayProperty=nameWithType> 以添加对导出策略的支持的类型。 因此，<xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> 在为任何包含它的绑定而生成策略时都包含 `UdpTransportBindingElement`。  
  
 在 <xref:System.ServiceModel.Description.IPolicyExportExtension.ExportPolicy%2A?displayProperty=nameWithType> 中，如果通道处于多路广播模式，添加 UDP 的断言和其他断言。 这是因为，多路广播模式影响通信堆栈的构造方式，因此必须在两端之间进行协调。  
  
```csharp  
ICollection<XmlElement> bindingAssertions = context.GetBindingAssertions();  
XmlDocument xmlDocument = new XmlDocument();  
bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.TransportAssertion, UdpPolicyStrings.UdpNamespace));  
if (Multicast)  
{  
    bindingAssertions.Add(xmlDocument.CreateElement(  
UdpPolicyStrings.Prefix, UdpPolicyStrings.MulticastAssertion,     UdpPolicyStrings.UdpNamespace));  
}  
```  
  
 因为自定义传输绑定元素负责处理寻址，所以 <xref:System.ServiceModel.Description.IPolicyExportExtension?displayProperty=nameWithType> 上的 `UdpTransportBindingElement` 实现还必须处理导出相应的 WS-Addressing 策略断言以指示正在使用的 WS-Addressing 的版本。  
  
```csharp  
AddWSAddressingAssertion(context, encodingBindingElement.MessageVersion.Addressing);  
```  
  
#### <a name="policy-import"></a>策略导入  

 若要扩展策略导入系统，请将以下配置添加到 Svcutil.exe 的配置文件，如 Svcutil.exe.config 文件所示：  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <metadata>  
        <policyImporters>  
          <extension type=" Microsoft.ServiceModel.Samples.UdpBindingElementImporter, UdpTransport" />  
        </policyImporters>  
      </metadata>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
 然后从已注册的类 (<xref:System.ServiceModel.Description.IPolicyImportExtension?displayProperty=nameWithType>) 中实现 `UdpBindingElementImporter`。 在 <xref:System.ServiceModel.Description.IPolicyImportExtension.ImportPolicy%2A?displayProperty=nameWithType> 中，检查适当命名空间中的断言并处理那些生成传输和检查它是否为多路广播的断言。 此外，从绑定断言列表中删除导入程序处理的断言。 同样，当运行 Svcutil.exe 时，有两个用于集成的选项：  
  
1. 使用/SvcutilConfig 将 Svcutil.exe 点指向配置文件： \<file> 。  
  
2. 将配置节添加到与 Svcutil.exe 处于同一目录的 Svcutil.exe.config 中。  
  
### <a name="adding-a-custom-standard-binding-importer"></a>添加自定义标准绑定导入程序  

 默认情况下，Svcutil.exe 和 <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> 类型识别并导入系统提供的绑定。 否则，绑定将作为 <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> 实例而导入。 若要启用 Svcutil.exe 和 <xref:System.ServiceModel.Description.WsdlImporter> 以导入 `SampleProfileUdpBinding`，`UdpBindingElementImporter` 还需充当自定义标准绑定导入程序。  
  
 自定义标准绑定导入程序 `ImportEndpoint` 在接口上实现方法 <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> ，以检查 <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> 从元数据导入的实例，以确定它是否可由特定的标准绑定生成。  
  
```csharp  
if (context.Endpoint.Binding is CustomBinding)  
{  
    Binding binding;  
    if (transportBindingElement is UdpTransportBindingElement)  
    {  
        //if TryCreate is true, the CustomBinding will be replace by a SampleProfileUdpBinding in the  
        //generated config file for better typed generation.  
        if (SampleProfileUdpBinding.TryCreate(bindingElements, out binding))  
        {  
            binding.Name = context.Endpoint.Binding.Name;  
            binding.Namespace = context.Endpoint.Binding.Namespace;  
            context.Endpoint.Binding = binding;  
        }  
    }  
}  
```  
  
 通常，实现自定义标准绑定导入程序包括检查导入的绑定元素的属性以验证是否仅更改了标准绑定可以设置的属性，而所有其他属性都是各自的默认值。 用于实现标准绑定导入程序的基本策略是创建标准绑定的实例，将绑定元素中的属性传播到标准绑定支持的标准绑定实例中，然后将标准绑定中的绑定元素与导入的绑定元素进行比较。
