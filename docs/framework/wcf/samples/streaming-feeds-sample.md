---
title: 流处理源示例
ms.date: 03/30/2017
ms.assetid: 1f1228c0-daaa-45f0-b93e-c4a158113744
ms.openlocfilehash: 735a72cba3c953ea4774d89751dad3216aa44400
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257179"
---
# <a name="streaming-feeds-sample"></a>流处理源示例

此示例演示如何管理包含大量项的联合源。 在服务器上，此示例演示如何延迟在源中创建各个 <xref:System.ServiceModel.Syndication.SyndicationItem> 对象，一直到项就要被写入网络流之前。  
  
 在客户端上，该示例演示如何使用自定义联合源格式化程序读取网络流中的各个项，以使当前读取的源永远不会完全缓冲到内存中。  
  
 为了最充分地演示联合 API 的流处理功能，此示例使用了一个似乎不可能的方案，在这个方案中，服务器公开一个包含无数个项的源。 在这种情况下，服务器继续在源中生成新的项，一直到它认为客户端已从源中读取指定数目（默认为 10）的项为止。 为简单起见，客户端和服务器是在同一个进程中实现的，并且使用一个共享 `ItemCounter` 对象跟踪客户端生成的项数。 `ItemCounter` 类型存在的唯一目的是为了使示例方案能够完全终止，它并不是所演示的模式的核心元素。  
  
 此演示使用关键字构造)  (使用 Visual c # 迭代器 `yield return` 。 有关迭代器的详细信息，请参阅 MSDN 上的 "使用迭代器" 主题。  
  
## <a name="service"></a>服务  

 服务实现包含一个操作的基本 <xref:System.ServiceModel.Web.WebGetAttribute> 协定，如下面的代码所示。  
  
```csharp  
[ServiceContract]  
interface IStreamingFeedService  
{  
    [WebGet]  
    [OperationContract]  
    Atom10FeedFormatter StreamedFeed();  
}  
```  
  
 服务通过使用 `ItemGenerator` 类创建一个可能无限的 <xref:System.ServiceModel.Syndication.SyndicationItem> 实例流（使用迭代器）来实现此协定，如下面的代码所示。  
  
```csharp
class ItemGenerator  
{  
    public IEnumerable<SyndicationItem> GenerateItems()  
    {  
        while (counter.GetCount() < maxItemsRead)  
        {  
            itemsReturned++;  
            yield return CreateNextItem();  
        }  
  
    }  
    ...  
}  
```  
  
 当服务实现创建源时，使用 `ItemGenerator.GenerateItems()` 的输出，而不是使用缓冲的项集合。  
  
```csharp
public Atom10FeedFormatter StreamedFeed()  
{  
    SyndicationFeed feed = new SyndicationFeed("Streamed feed", "Feed to test streaming", null);  
    //Generate an infinite stream of items. Both the client and the service share  
    //a reference to the ItemCounter, which allows the sample to terminate  
    //execution after the client has read 10 items from the stream  
    ItemGenerator itemGenerator = new ItemGenerator(this.counter, 10);  
  
    feed.Items = itemGenerator.GenerateItems();  
    return feed.GetAtom10Formatter();  
}  
```  
  
 因此，项流永远不会完全缓冲到内存中。 你可以通过在方法中的语句上设置断点来观察此行为 `yield return` `ItemGenerator.GenerateItems()` ，并注意在服务返回了方法的结果后首次遇到此断点 `StreamedFeed()` 。  
  
## <a name="client"></a>客户端  

 此示例中的客户端使用自定义 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter> 实现，该实现延迟各个项在源中的具体化，而不是将它们缓冲到内存中。 自定义 `StreamedAtom10FeedFormatter` 实例的使用如下所示。  
  
```csharp  
XmlReader reader = XmlReader.Create("http://localhost:8000/Service/Feeds/StreamedFeed");  
StreamedAtom10FeedFormatter formatter = new StreamedAtom10FeedFormatter(counter);  
  
SyndicationFeed feed = formatter.ReadFrom(reader);  
```  
  
 通常，在从网络中读取源的全部内容并将内容缓冲到内存中之前，并不会返回对 <xref:System.ServiceModel.Syndication.SyndicationFeedFormatter.ReadFrom%28System.Xml.XmlReader%29> 的调用。 但是，`StreamedAtom10FeedFormatter` 对象重写了 <xref:System.ServiceModel.Syndication.Atom10FeedFormatter.ReadItems%28System.Xml.XmlReader%2CSystem.ServiceModel.Syndication.SyndicationFeed%2CSystem.Boolean%40%29> 以返回迭代器，而不是返回缓冲的集合，如下面的代码所示。  
  
```csharp  
protected override IEnumerable<SyndicationItem> ReadItems(XmlReader reader, SyndicationFeed feed, out bool areAllItemsRead)  
{  
    areAllItemsRead = false;  
    return DelayReadItems(reader, feed);  
}  
  
private IEnumerable<SyndicationItem> DelayReadItems(XmlReader reader, SyndicationFeed feed)  
{  
    while (reader.IsStartElement("entry", "http://www.w3.org/2005/Atom"))  
    {  
        yield return this.ReadItem(reader, feed);  
    }  
  
    reader.ReadEndElement();  
}  
```  
  
 因此，在遍历 `ReadItems()` 结果的客户端应用程序准备好使用每个项之前，并不会从网络中读取每个项。 您可以通过在中的语句上设置断点来观察此行为 `yield return` `StreamedAtom10FeedFormatter.DelayReadItems()` ，并注意在调用完成后第一次遇到该断点 `ReadFrom()` 。  
  
 下面的说明演示如何生成并运行示例。 请注意，尽管服务器在客户端读取 10 个项后停止生成项，但输出显示客户端读取的项数远远超过 10 个。 这是因为示例使用的网络绑定以 4 KB 段传输数据。 因此，客户端在有机会读取一个项之前，已收到 4KB 的项数据。 这是正常的行为（以合理大小的段发送经过流处理的 HTTP 数据可以提高性能）。  
  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Syndication\StreamingFeeds`  
  
## <a name="see-also"></a>另请参阅

- [独立诊断源](stand-alone-diagnostics-feed-sample.md)
