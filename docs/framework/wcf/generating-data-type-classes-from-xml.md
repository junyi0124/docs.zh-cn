---
title: 从 XML 生成数据类型类
ms.date: 03/30/2017
ms.assetid: e4e5e4e8-527f-44d1-92fa-8904a08784ea
ms.openlocfilehash: a7920a8c8c4f279dd3fc52029c5da5e9b685efe2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238244"
---
# <a name="generating-data-type-classes-from-xml"></a>从 XML 生成数据类型类

.NET Framework 4.5 包含一项新功能，可用于从 XML 生成数据类型类。 本主题介绍如何为 .NET 博客 RSS 源自动生成数据类型。  
  
### <a name="obtaining-the-xml-from-the-net-blog-rss-feed"></a>从 .NET 博客 RSS 源获取 XML  
  
1. 在 Internet Explorer 中，导航到 [.Net 博客 rss 源](https://devblogs.microsoft.com/dotnet/feed/)。  
  
2. 右键单击该页并选择 " **查看源**"。  
  
3. 通过按 **ctrl + A** 选择所有文本，然后按 **ctrl + C** 复制，复制源的文本。  
  
### <a name="creating-the-data-types"></a>创建数据类型  
  
1. 打开要使用代理的代码文件。 此文件应属于 .NET Framework 4.5 项目。  
  
2. 将游标放置于该文件中任何现有类之外的位置中。  
  
3. 选择 " **编辑**"、" **粘贴特殊**"、"将 **XML 粘贴为类**"。  
  
4. 将创建名为、、、和的类 `link` ， `rss` `rssChannel` `rssChannelImage` `rssChannelItem` `rssChannelItemGuid` 其中包含访问 RSS 源中的元素所需的成员。  
  
### <a name="using-the-generated-classes"></a>使用生成的类  
  
1. 一旦生成了类，这些类就可以像任何其他类一样在代码中使用。 下面的代码示例返回 `rssChannelImage` 类的新实例。  
  
    ```csharp
    var channelImage = new rssChannelImage()
    {
        title = "MyImage",
        link = "http://www.contoso.com/images/channelImage.jpg",
        url = "http://www.contoso.com/entries/myEntry.html"
    };  
    ```
