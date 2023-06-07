---
title: 自定义筛选器
ms.date: 03/30/2017
ms.assetid: 97cf247d-be0a-4057-bba9-3be5c45029d5
ms.openlocfilehash: b5ec07c2e2a77c7de8b240d21b1eb54bf858b43b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258421"
---
# <a name="custom-filters"></a>自定义筛选器

借助自定义筛选器，您可以定义无法通过系统提供的消息筛选器实现的匹配逻辑。 例如，您可以创建这样的自定义筛选器：该筛选器散列特定的消息元素，然后检查元素值以确定该筛选器应返回 true 还是 false。  
  
## <a name="implementation"></a>实现  

 自定义筛选器是 <xref:System.ServiceModel.Dispatcher.MessageFilter> 抽象基类的实现。 当实现自定义筛选器时，构造函数可以选择接受一个字符串参数。 该参数包含传递到 MessageFilter 构造函数的配置信息，以便提供筛选器为执行匹配而在运行时需要的任何值或配置。 例如，该参数可用于提供筛选器在要进行求值的消息中查找的值。 下面的示例演示接受字符串参数的自定义消息筛选器的基本实现：  
  
```csharp  
public class MyMessageFilter: MessageFilter  
{  
    string filterData;  
    public MyMessageFilter(string filterData)  
    {  
        if(string.IsNullOrEmpty(filterData)  
            throw new ArgumentNullException("filterData");  
        this.filterData=filterData;  
    }  
    public override bool Match(System.ServiceModel.Channels.Message message)  
    {  
        ...  
        return retValue;  
    }  
    public override bool Match(System.ServiceModel.Channels.MessageBuffer buffer)  
    {  
        ...  
        return retValue;  
    }  
}  
```  
  
> [!NOTE]
> 在实际实现中，Match 方法 (s) 包含将检查消息以确定此消息筛选器是否应返回 **true** 或 **false** 的逻辑。  
  
### <a name="performance"></a>性能  

 实现自定义筛选器时，应考虑筛选器完成消息评估所需要的最长时间，这十分重要。 由于某个消息可能需通过多个筛选器进行评估才能找到匹配项，因此应确保客户端请求在评估完所有筛选器之前不会超时，这十分重要。 因此，自定义筛选器只应包含评估消息的内容或特性所需的代码，以便确定消息是否匹配筛选条件。  
  
 一般而言，在实现自定义筛选器时，应避免以下情况：  
  
- IO，如将数据保存到磁盘或数据库。  
  
- 不必要的处理，如循环访问文档中的多个记录。  
  
- 阻止操作，如涉及获取共享资源上的锁的调用或涉及对数据库执行查找的调用。  
  
 在生产环境中使用自定义筛选器之前，应运行性能测试以确定筛选器评估消息所花费的平均时间长度。 与筛选器表中使用的其他筛选器的平均处理时间相结合，您便可精确确定客户端应用程序应指定的最大超时值。  
  
## <a name="usage"></a>使用情况  

 若要将自定义筛选器与路由服务一起使用，必须通过指定 "自定义" 类型的新筛选器项，将消息筛选器的完全限定类型名称和程序集的名称添加到筛选器表中。  与其他 MessageFilter 一样，您可以指定将传递到自定义筛选器的构造函数中的字符串 filterData。  
  
 下面的示例演示如何对路由服务使用自定义筛选器：  
  
```xml  
<!--ROUTING SECTION -->  
<routing>  
  <filters>  
    <filter name="CustomFilter1" filterType="Custom"
            customType="CustomAssembly.MyMessageFilter,
            CustomAssembly" filterData="custom data" />  
  </filters>  
  <filterTables>  
    <table name="routingTable1">  
      <filters>  
        <add filterName="CustomFilter1" endpointName="CalculatorService" />  
      </filters>  
    </table>  
  </filterTables>  
</routing>  
```  
  
```csharp  
RoutingConfiguration rc = new RoutingConfiguration();  
List<ServiceEndpoint> endpointList = new List<ServiceEndpoint>();  
endpointList.Add(client);  
rc.FilterTable.Add(new MyMessageFilter("CustomData"), endpointList);  
```
