---
title: 强类型扩展示例
ms.date: 03/30/2017
ms.assetid: 02220f11-1a83-441c-9e5a-85f9a9367572
ms.openlocfilehash: e5b74188d4c9c333858c60ff95a2a90b0e2e9418
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275926"
---
# <a name="strongly-typed-extensions-sample"></a>强类型扩展示例

此示例使用 <xref:System.ServiceModel.Syndication.SyndicationFeed> 类作为示例。 但是，此示例中演示的模式可用于支持扩展数据的所有 Syndication 类。  
  
 联合对象模型（<xref:System.ServiceModel.Syndication.SyndicationFeed>、<xref:System.ServiceModel.Syndication.SyndicationItem> 和相关类）支持通过使用 <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> 和 <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> 属性对扩展数据进行松散类型的访问。 此示例演示如何通过实现的自定义派生类 <xref:System.ServiceModel.Syndication.SyndicationFeed> 并 <xref:System.ServiceModel.Syndication.SyndicationItem> 使特定于应用程序的扩展成为强类型属性，来提供对扩展数据的强类型访问。  
  
 此示例演示如何实现建议的 Atom Threading Extensions RFC 中定义的一个扩展元素，以作为示例。 此示例仅用于演示，不应作为该建议规范的完整实现。  
  
## <a name="sample-xml"></a>示例 XML  

 下面的 XML 示例显示一个具有附加的 `<in-reply-to>` 扩展元素的 Atom 1.0 项。  
  
```xml  
<entry>  
    <id>tag:example.org,2005:1,2</id>  
    <title type="text">Another response to the original</title>  
    <summary type="text">  
         This is a response to the original entry</summary>  
    <updated>2006-03-01T12:12:13Z</updated>  
    <link href="http://www.example.org/entries/1/2" />  
    <in-reply-to p3:ref="tag:example.org,2005:1"
                 p3:href="http://www.example.org/entries/1"
                 p3:type="application/xhtml+xml"
                 xmlns:p3="http://contoso.org/syndication/thread/1.0"
                 xmlns="http://contoso.org/syndication/thread/1.0">  
      <anotherElement xmlns="http://www.w3.org/2005/Atom">  
                     Some more data</anotherElement>  
      <aDifferentElement xmlns="http://www.w3.org/2005/Atom">  
                     Even more data</aDifferentElement>  
    </in-reply-to>  
</entry>  
```  
  
 `<in-reply-to>`元素指定 (和) 三个必需 `ref` 的属性， `type` `href` 同时还允许存在其他扩展属性和扩展元素。  
  
## <a name="modeling-the-in-reply-to-element"></a>对 In-Reply-To 元素建模  

 在此示例中，`<in-reply-to>` 元素建模为实现 <xref:System.Xml.Serialization.IXmlSerializable> 的 CLR，从而可以与 <xref:System.Runtime.Serialization.DataContractSerializer> 一起使用。 它还实现了一些方法和属性来访问元素的数据，如下面的示例代码所示。  
  
```csharp  
[XmlRoot(ElementName = "in-reply-to", Namespace = "http://contoso.org/syndication/thread/1.0")]  
public class InReplyToElement : IXmlSerializable  
{  
    internal const string ElementName = "in-reply-to";  
    internal const string NsUri =
                  "http://contoso.org/syndication/thread/1.0";  
    private Dictionary<XmlQualifiedName, string> extensionAttributes;  
    private Collection<XElement> extensionElements;  
  
    public InReplyToElement()  
    {  
        this.extensionElements = new Collection<XElement>();  
        this.extensionAttributes = new Dictionary<XmlQualifiedName,
                                                          string>();  
    }  
  
    public Dictionary<XmlQualifiedName, string> AttributeExtensions  
    {  
        get { return this.extensionAttributes; }  
    }  
  
    public Collection<XElement> ElementExtensions  
    {  
        get { return this.extensionElements; }  
    }  
  
    public Uri Href  
    { get; set; }  
  
    public string MediaType  
    { get; set; }  
  
    public string Ref  
    { get; set; }  
  
    public Uri Source  
    { get; set; }  
}  
```  
  
 `InReplyToElement` 类实现所需属性 (Attribute) 的属性 (Property)（`HRef`、`MediaType` 和 `Source`）以及用于保存 <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> 和 <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> 的集合。  
  
 `InReplyToElement` 类实现 <xref:System.Xml.Serialization.IXmlSerializable> 接口，该接口允许直接控制从 XML 读取对象实例和向 XML 写入对象实例的方式。 `ReadXml` 方法首先从传递给它的 `Ref` 读取 `HRef`、`Source`、`MediaType` 和 <xref:System.Xml.XmlReader> 属性的值。 所有未知的属性都存储在 <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> 集合中。 读取所有属性之后，将调用 <xref:System.Xml.XmlReader.ReadStartElement> 使读取器前进到下一个元素。 由于由该类建模的元素没有所需的子元素，因此子元素缓冲到 `XElement` 实例中并存储在 <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> 集合中，如下面的代码所示。  
  
```csharp
public void ReadXml(System.Xml.XmlReader reader)  
{  
    bool isEmpty = reader.IsEmptyElement;  
  
    if (reader.HasAttributes)  
    {  
        for (int i = 0; i < reader.AttributeCount; i++)  
        {  
            reader.MoveToNextAttribute();  
  
            if (reader.NamespaceURI == "")  
            {  
                if (reader.LocalName == "ref")  
                {  
                    this.Ref = reader.Value;  
                }  
                else if (reader.LocalName == "href")  
                {  
                    this.Href = new Uri(reader.Value);  
                }  
                else if (reader.LocalName == "source")  
                {  
                    this.Source = new Uri(reader.Value);  
                }  
                else if (reader.LocalName == "type")  
                {  
                    this.MediaType = reader.Value;  
                }  
                else  
                {  
                    this.AttributeExtensions.Add(new
                                 XmlQualifiedName(reader.LocalName,
                                 reader.NamespaceURI),
                                 reader.Value);  
                }  
            }  
        }  
    }  
  
    reader.ReadStartElement();  
  
    if (!isEmpty)  
    {  
        while (reader.IsStartElement())  
        {  
            ElementExtensions.Add(  
                  (XElement) XElement.ReadFrom(reader));  
        }  
        reader.ReadEndElement();  
    }  
}  
```  
  
 在 `WriteXml` 中，`InReplyToElement` 方法首先将 `Ref`、`HRef`、`Source` 和 `MediaType` 属性的值写出为 XML 属性。（`WriteXml` 并不负责编写实际的外部元素本身，该工作是由 `WriteXml` 的调用方完成的。） 它还将 <xref:System.ServiceModel.Syndication.SyndicationFeed.AttributeExtensions%2A> 和 <xref:System.ServiceModel.Syndication.SyndicationFeed.ElementExtensions%2A> 的内容写入编写器，如下面的代码所示。  
  
```csharp
public void WriteXml(System.Xml.XmlWriter writer)  
{  
    if (this.Ref != null)  
    {  
        writer.WriteAttributeString("ref", InReplyToElement.NsUri,
                                            this.Ref);  
    }  
    if (this.Href != null)  
    {  
        writer.WriteAttributeString("href", InReplyToElement.NsUri,
                                                this.Href.ToString());  
    }  
    if (this.Source != null)  
    {  
        writer.WriteAttributeString("source", InReplyToElement.NsUri,
                                              this.Source.ToString());  
    }  
    if (this.MediaType != null)  
    {  
        writer.WriteAttributeString("type", InReplyToElement.NsUri,
                                                    this.MediaType);  
    }  
  
    foreach (KeyValuePair<XmlQualifiedName, string> kvp in
                                             this.AttributeExtensions)  
    {  
        writer.WriteAttributeString(kvp.Key.Name, kvp.Key.Namespace,
                                                   kvp.Value);  
    }  
  
    foreach (XElement element in this.ElementExtensions)  
    {  
        element.WriteTo(writer);  
    }  
}  
```  
  
## <a name="threadedfeed-and-threadeditem"></a>ThreadedFeed 和 ThreadedItem  

 在此示例中，`SyndicationItems` 类对具有 `InReplyTo` 扩展的 `ThreadedItem` 进行建模。 同样，`ThreadedFeed` 类是一个其项是 `SyndicationFeed` 的所有实例的 `ThreadedItem`。  
  
 `ThreadedFeed` 类从 `SyndicationFeed` 继承，并重写 `OnCreateItem` 以返回一个 `ThreadedItem`。 它还实现用于将 `Items` 集合作为 `ThreadedItems` 访问的方法，如下面的代码所示。  
  
```csharp
public class ThreadedFeed : SyndicationFeed  
{  
    public ThreadedFeed()  
    {  
    }  
  
    public IEnumerable<ThreadedItem> ThreadedItems  
    {  
        get  
        {  
            return this.Items.Cast<ThreadedItem>();  
        }  
    }  
  
    protected override SyndicationItem CreateItem()  
    {  
        return new ThreadedItem();  
    }  
}  
```  
  
 类 `ThreadedItem` 继承自 `SyndicationItem` ，并 `InReplyToElement` 作为强类型属性进行。 这样便可以方便地对 `InReplyTo` 扩展数据进行编程访问。 它还实现 `TryParseElement` 和 `WriteElementExtensions`，用于读取和写入其扩展数据，如下面的代码所示。  
  
```csharp
public class ThreadedItem : SyndicationItem  
{  
    private InReplyToElement inReplyTo;  
    // Constructors  
        public ThreadedItem()  
        {  
            inReplyTo = new InReplyToElement();  
        }  
  
        public ThreadedItem(string title, string content, Uri itemAlternateLink, string id, DateTimeOffset lastUpdatedTime) : base(title, content, itemAlternateLink, id, lastUpdatedTime)  
        {  
            inReplyTo = new InReplyToElement();  
        }  
  
    public InReplyToElement InReplyTo  
    {  
        get { return this.inReplyTo; }  
    }  
  
    protected override bool TryParseElement(  
                        System.Xml.XmlReader reader,
                        string version)  
    {  
        if (version == SyndicationVersions.Atom10 &&  
            reader.NamespaceURI == InReplyToElement.NsUri &&  
            reader.LocalName == InReplyToElement.ElementName)  
        {  
            this.inReplyTo = new InReplyToElement();  
  
            this.InReplyTo.ReadXml(reader);  
  
            return true;  
        }  
        else  
        {  
            return base.TryParseElement(reader, version);  
        }  
    }  
  
    protected override void WriteElementExtensions(XmlWriter writer,
                                                 string version)  
    {  
        if (this.InReplyTo != null &&
                     version == SyndicationVersions.Atom10)  
        {  
            writer.WriteStartElement(InReplyToElement.ElementName,
                                           InReplyToElement.NsUri);  
            this.InReplyTo.WriteXml(writer);  
            writer.WriteEndElement();  
        }  
  
        base.WriteElementExtensions(writer, version);  
    }  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Syndication\StronglyTypedExtensions`  
