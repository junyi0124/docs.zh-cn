---
title: 自定义消息编码器：压缩编码器
ms.date: 03/30/2017
ms.assetid: 57450b6c-89fe-4b8a-8376-3d794857bfd7
ms.openlocfilehash: db20ec20579d6fcb0ec202920db0d7781b0676df
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600615"
---
# <a name="custom-message-encoder-compression-encoder"></a>自定义消息编码器：压缩编码器

此示例演示如何使用 Windows Communication Foundation （WCF）平台实现自定义编码器。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Compression`

## <a name="sample-details"></a>示例详细信息

该示例由一个客户端控制台程序 (.exe)、一个自承载的服务控制台程序 (.exe) 和一个压缩消息编码器库 (.dll) 组成。 该服务实现定义“请求-答复”通信模式的协定。 该协定由 `ISampleServer` 接口定义，而该接口将公开基本字符串回显操作（`Echo` 和 `BigEcho`）。 客户端向给定的操作发出同步请求，服务通过将该消息回送到客户端来进行回复。 客户端和服务活动显示在控制台窗口中。 提供该示例的目的在于演示如何编写自定义编码器并演示压缩消息对网络的影响。 可以为压缩消息编码器添加计算消息大小和/或处理时间的方法。

> [!NOTE]
> 在 .NET Framework 4 中，如果服务器正在发送压缩响应（使用 GZip 或 Deflate 等算法创建），则会在 WCF 客户端上启用自动解压缩。 如果服务是 Internet 信息服务 (IIS) 中的 Web 承载服务，则可以为该服务配置 IIS 以发送压缩响应。 如果客户端和服务上都需要执行压缩和解压缩，或者如果该服务是自承载服务，则可以使用此示例。

此示例演示如何生成自定义消息编码器并将其集成到 WCF 应用程序中。 库 GZipEncoder.dll 是同时用客户端和服务部署的。 该示例还演示对消息进行压缩所带来的影响。 GZipEncoder.dll 中的代码演示以下操作：

- 生成自定义编码器和编码器工厂。

- 为自定义编码器开发绑定元素。

- 使用自定义绑定配置集成自定义绑定元素。

- 开发自定义配置处理程序以允许对自定义绑定元素进行文件配置。

如上所述，在自定义编码器中实现了多个层。 为了更好地阐释各层之间的关系，下面列出了经过简化的服务启动事件顺序的列表：

1. 服务器启动。

2. 读取配置信息。

    1. 服务配置对自定义配置处理程序进行注册。

    2. 创建并打开服务主机。

    3. 自定义配置元素创建并返回自定义绑定元素。

    4. 自定义绑定元素创建并返回消息编码器工厂。

3. 接收消息。

4. 消息编码器工厂返回消息编码器以读入消息并写出响应。

5. 编码器层是作为类工厂实现的。 对于自定义编码器，只有编码器类工厂才是必须公开的。 在创建 <xref:System.ServiceModel.ServiceHost> 或 <xref:System.ServiceModel.ChannelFactory%601> 对象时，绑定元素会返回工厂对象。 消息编码器可以在缓冲模式或流模式下运行。 此示例对缓冲模式和流模式均进行了演示。

在每个模式下，`ReadMessage` 抽象类都随附了 `WriteMessage` 和 `MessageEncoder` 方法。 大部分编码工作都在这些方法中进行。 此示例包装现有的文本和二进制消息编码器。 这允许该示例将对消息网络表示形式的读写操作委托给内部编码器，并允许压缩编码器对结果进行压缩或解压缩。 由于没有用于消息编码的管道，这是在 WCF 中使用多个编码器的唯一模型。 在对消息进行解压缩之后，所得到的消息将立即向上传递到堆栈，供通道堆栈进行处理。 在压缩过程中，所得到的压缩消息都直接写入所提供的流中。

此示例使用帮助器方法（`CompressBuffer` 和 `DecompressBuffer`）执行从缓冲区到流的转换，以便使用 `GZipStream` 类。

经过缓冲处理的 `ReadMessage` 和 `WriteMessage` 类使用 `BufferManager` 类。 编码器只能通过编码器工厂来访问。 `MessageEncoderFactory` 抽象类提供了一个名为 `Encoder` 的属性和一个名为 `CreateSessionEncoder` 的方法，前者用来访问当前的编码器，后者用来创建支持会话的编码器。 类似的编码器有序而可靠，可以用在通道支持会话的方案中。 对于写入网络中的数据，此方案允许在每个会话中进行优化。 如果这不是所需的，则基方法不应当重载。 `Encoder` 属性提供了一种访问无会话编码器的机制，该属性的值由 `CreateSessionEncoder` 方法的默认实现返回。 由于此示例通过包装现有的编码器来提供压缩，因此 `MessageEncoderFactory` 实现接受一个表示内部编码器工厂的 `MessageEncoderFactory`。

定义编码器和编码器工厂后，可以将其与 WCF 客户端和服务一起使用。 但是，必须将这些编码器添加到通道堆栈中。 您可以从 <xref:System.ServiceModel.ServiceHost> 和 <xref:System.ServiceModel.ChannelFactory%601> 类派生类，并重写 `OnInitialize` 方法以手动添加该编码器工厂。 还可以通过自定义绑定元素来公开编码器工厂。

若要创建新的自定义绑定元素，请从 <xref:System.ServiceModel.Channels.BindingElement> 类派生一个类。 但是，存在多种类型的绑定元素。 为了确保自定义绑定元素被识别为消息编码绑定元素，还必须实现 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement>。 <xref:System.ServiceModel.Channels.MessageEncodingBindingElement> 公开了一种用来新建消息编码器工厂的方法 (`CreateMessageEncoderFactory`)，实现该方法的目的在于返回匹配的消息编码器工厂的实例。 另外，<xref:System.ServiceModel.Channels.MessageEncodingBindingElement> 还有一个指示寻址版本的属性。 由于此示例包装现有的编码器，因此示例实现还会包装现有的编码器绑定元素、将内部编码器绑定元素作为构造函数的参数并通过一个属性来公开它。 下面的示例代码演示 `GZipMessageEncodingBindingElement` 类的实现。

```csharp
public sealed class GZipMessageEncodingBindingElement
                        : MessageEncodingBindingElement //BindingElement
                        , IPolicyExportExtension
{

    //We use an inner binding element to store information
    //required for the inner encoder.
    MessageEncodingBindingElement innerBindingElement;

        //By default, use the default text encoder as the inner encoder.
        public GZipMessageEncodingBindingElement()
            : this(new TextMessageEncodingBindingElement()) { }

    public GZipMessageEncodingBindingElement(MessageEncodingBindingElement messageEncoderBindingElement)
    {
        this.innerBindingElement = messageEncoderBindingElement;
    }

    public MessageEncodingBindingElement InnerMessageEncodingBindingElement
    {
        get { return innerBindingElement; }
        set { innerBindingElement = value; }
    }

    //Main entry point into the encoder binding element.
    // Called by WCF to get the factory that creates the
    //message encoder.
    public override MessageEncoderFactory CreateMessageEncoderFactory()
    {
        return new
GZipMessageEncoderFactory(innerBindingElement.CreateMessageEncoderFactory());
    }

    public override MessageVersion MessageVersion
    {
        get { return innerBindingElement.MessageVersion; }
        set { innerBindingElement.MessageVersion = value; }
    }

    public override BindingElement Clone()
    {
        return new
        GZipMessageEncodingBindingElement(this.innerBindingElement);
    }

    public override T GetProperty<T>(BindingContext context)
    {
        if (typeof(T) == typeof(XmlDictionaryReaderQuotas))
        {
            return innerBindingElement.GetProperty<T>(context);
        }
        else
        {
            return base.GetProperty<T>(context);
        }
    }

    public override IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.BuildInnerChannelFactory<TChannel>();
    }

    public override IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.BuildInnerChannelListener<TChannel>();
    }

    public override bool CanBuildChannelListener<TChannel>(BindingContext context)
    {
        if (context == null)
            throw new ArgumentNullException("context");

        context.BindingParameters.Add(this);
        return context.CanBuildInnerChannelListener<TChannel>();
    }

    void IPolicyExportExtension.ExportPolicy(MetadataExporter exporter, PolicyConversionContext policyContext)
    {
        if (policyContext == null)
        {
            throw new ArgumentNullException("policyContext");
        }
       XmlDocument document = new XmlDocument();
       policyContext.GetBindingAssertions().Add(document.CreateElement(
            GZipMessageEncodingPolicyConstants.GZipEncodingPrefix,
            GZipMessageEncodingPolicyConstants.GZipEncodingName,
            GZipMessageEncodingPolicyConstants.GZipEncodingNamespace));
    }
}
```

请注意，`GZipMessageEncodingBindingElement` 类实现 `IPolicyExportExtension` 接口，以使该绑定元素可以作为元数据中的策略导出，如下面的示例所示。

```xml
<wsp:Policy wsu:Id="BufferedHttpSampleServer_ISampleServer_policy">
    <wsp:ExactlyOne>
      <wsp:All>
        <gzip:text xmlns:gzip=
        "http://schemas.microsoft.com/ws/06/2004/mspolicy/netgzip1" />
       <wsaw:UsingAddressing />
     </wsp:All>
   </wsp:ExactlyOne>
</wsp:Policy>
```

`GZipMessageEncodingBindingElementImporter` 类实现 `IPolicyImportExtension` 接口，该类导入 `GZipMessageEncodingBindingElement` 的策略。 Svcutil.exe 工具可用于将策略导入到配置文件中以处理 `GZipMessageEncodingBindingElement`，应将下面的代码添加到 Svcutil.exe.config 中。

```xml
<configuration>
  <system.serviceModel>
    <extensions>
      <bindingElementExtensions>
        <add name="gzipMessageEncoding"
          type=
            "Microsoft.ServiceModel.Samples.GZipMessageEncodingElement, GZipEncoder, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      </bindingElementExtensions>
    </extensions>
    <client>
      <metadata>
        <policyImporters>
          <remove type=
"System.ServiceModel.Channels.MessageEncodingBindingElementImporter, System.ServiceModel, Version=3.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
          <extension type=
"Microsoft.ServiceModel.Samples.GZipMessageEncodingBindingElementImporter, GZipEncoder, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        </policyImporters>
      </metadata>
    </client>
  </system.serviceModel>
</configuration>
```

现在压缩编码器具有一个匹配的绑定元素，可以通过编程方式将它挂钩到服务或客户端，方法是构造新的自定义绑定对象并向其中添加自定义绑定元素，如下面的示例代码所示。

```csharp
ICollection<BindingElement> bindingElements = new List<BindingElement>();
HttpTransportBindingElement httpBindingElement = new HttpTransportBindingElement();
GZipMessageEncodingBindingElement compBindingElement = new GZipMessageEncodingBindingElement ();
bindingElements.Add(compBindingElement);
bindingElements.Add(httpBindingElement);
CustomBinding binding = new CustomBinding(bindingElements);
binding.Name = "SampleBinding";
binding.Namespace = "http://tempuri.org/bindings";
```

尽管对于大多数用户方案该操作已足够，但是，如果某个服务是由 Web 承载的，则支持文件配置是至关重要的。 若要支持由 Web 承载的方案，必须开发一个自定义配置处理程序，以便允许在文件中配置自定义绑定元素。

您可以在配置系统的顶部为绑定元素生成一个配置处理程序。 绑定元素的配置处理程序必须从 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> 类派生。 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.BindingElementType?displayProperty=nameWithType>通知配置系统要为此节创建的绑定元素的类型。 `BindingElement` 的所有可设置方面应当作为属性在 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> 派生类中公开。 <xref:System.Configuration.ConfigurationPropertyAttribute>如果缺少属性，则有助于将配置元素特性映射到属性和设置默认值。 在配置中的值加载并应用到属性之后，将调用 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.CreateBindingElement%2A?displayProperty=nameWithType> 方法，该方法将属性转换成绑定元素的具体实例。 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement.ApplyConfiguration%2A?displayProperty=nameWithType>方法用于将派生类的属性转换为 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> 要在新创建的绑定元素上设置的值。

下面的示例代码演示 `GZipMessageEncodingElement` 的实现。

```csharp
public class GZipMessageEncodingElement : BindingElementExtensionElement
{
    public GZipMessageEncodingElement()
    {
    }

//Called by the WCF to discover the type of binding element this
//config section enables
    public override Type BindingElementType
    {
        get { return typeof(GZipMessageEncodingBindingElement); }
    }

    //The only property we need to configure for our binding element is
    //the type of inner encoder to use. Here, we support text and
    //binary.
    [ConfigurationProperty("innerMessageEncoding",
                         DefaultValue = "textMessageEncoding")]
    public string InnerMessageEncoding
    {
        get { return (string)base["innerMessageEncoding"]; }
        set { base["innerMessageEncoding"] = value; }
    }

    //Called by the WCF to apply the configuration settings (the
    //property above) to the binding element
    public override void ApplyConfiguration(BindingElement bindingElement)
    {
        GZipMessageEncodingBindingElement binding =
                (GZipMessageEncodingBindingElement)bindingElement;
        PropertyInformationCollection propertyInfo =
                    this.ElementInformation.Properties;
        if (propertyInfo["innerMessageEncoding"].ValueOrigin !=
                                     PropertyValueOrigin.Default)
        {
            switch (this.InnerMessageEncoding)
            {
                case "textMessageEncoding":
                    binding.InnerMessageEncodingBindingElement =
                      new TextMessageEncodingBindingElement();
                    break;
                case "binaryMessageEncoding":
                    binding.InnerMessageEncodingBindingElement =
                         new BinaryMessageEncodingBindingElement();
                    break;
            }
        }
    }

    //Called by the WCF to create the binding element
    protected override BindingElement CreateBindingElement()
    {
        GZipMessageEncodingBindingElement bindingElement =
                new GZipMessageEncodingBindingElement();
        this.ApplyConfiguration(bindingElement);
        return bindingElement;
    }
}
```

此配置处理程序映射到服务或客户端的 App.config 或 Web.config 中的以下表示形式。

```xml
<gzipMessageEncoding innerMessageEncoding="textMessageEncoding" />
```

若要使用此配置处理程序，必须在元素中注册它， [\<system.serviceModel>](../../configure-apps/file-schema/wcf/system-servicemodel.md) 如下面的示例配置中所示。

```xml
<extensions>
    <bindingElementExtensions>
       <add
           name="gzipMessageEncoding"
           type=
           "Microsoft.ServiceModel.Samples.GZipMessageEncodingElement,
           GZipEncoder, Version=1.0.0.0, Culture=neutral,
           PublicKeyToken=null" />
      </bindingElementExtensions>
</extensions>
```

在运行服务器时，操作请求和响应将显示在控制台窗口中。 在该窗口中按 Enter 可以关闭服务器。

```console
Press Enter key to Exit.

        Server Echo(string input) called:
        Client message: Simple hello

        Server BigEcho(string[] input) called:
        64 client messages
```

在运行客户端时，操作请求和响应将显示在控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端。

```console
Calling Echo(string):
Server responds: Simple hello Simple hello

Calling BigEcho(string[]):
Server responds: Hello 0

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 使用以下命令安装 ASP.NET 4.0：

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

3. 若要生成解决方案，请按照[生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。

4. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageEncoder\Compression`
