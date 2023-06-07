---
title: 使用弹性堆栈进行日志记录
description: 使用弹性堆栈、Logstash 和 Kibana 进行日志记录
ms.date: 05/13/2020
ms.openlocfilehash: 3f10b0d06c87b7bed6d3e302742b1dc52e2c9d3b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155336"
---
# <a name="logging-with-elastic-stack"></a>使用弹性堆栈进行日志记录

有很多非常好的集中式日志记录工具，从免费的开源工具到成本较高的选项，这些工具的成本会有所不同。 在许多情况下，免费的工具与付费的产品/服务非常好。 一个此类工具是三个开源组件的组合：弹性搜索、Logstash 和 Kibana。

这些工具共同称为弹性堆栈或 ELK 堆栈。

## <a name="elastic-stack"></a>弹性堆栈

弹性堆栈是一个功能强大的选项，可用于从 Kubernetes 群集中收集信息。 Kubernetes 支持将日志发送到 Elasticsearch 终结点，而在 [大多数情况](https://kubernetes.io/docs/tasks/debug-application-cluster/logging-elasticsearch-kibana/)下，你只需设置环境变量，如图7-5 所示：

```kubernetes
KUBE_LOGGING_DESTINATION=elasticsearch
KUBE_ENABLE_NODE_LOGGING=true
```

**图 7-5**。 Kubernetes 的配置变量

这会在群集上安装 Elasticsearch，并将所有群集日志发送到该群集。

![Kibana 仪表板的一个示例，显示针对 Kubernetes ](./media/kibana-dashboard.png)
 **图 7-6**中的日志引入的查询结果。 Kibana 仪表板的一个示例，显示针对引入 from Kubernetes 中的日志查询的结果

## <a name="what-are-the-advantages-of-elastic-stack"></a>弹性堆栈的优点是什么？

弹性堆栈以低成本、可缩放且可缩放的云形式提供集中式日志记录。 它的用户界面可简化数据分析，使你可以从数据中发现见解，而无需使用笨接口。 它支持各种输入，因此，当你的分布式应用程序跨越更多和不同类型的服务时，你可以继续将日志和指标数据馈送到系统中。 弹性堆栈还支持快速搜索（甚至跨大型数据集），这样，即使大型应用程序也可以记录详细数据，并且仍能以高性能的方式对其进行查看。

## <a name="logstash"></a>Logstash

第一个组件为 [Logstash](https://www.elastic.co/products/logstash)。 此工具用于从大量不同的源收集日志信息。 例如，Logstash 可以从磁盘读取日志，还可以从日志记录库（如 [Serilog](https://serilog.net/)）接收消息。 Logstash 可以在日志到达时对其执行一些基本筛选和扩展。 例如，如果日志包含 IP 地址，则可以将 Logstash 配置为执行地理查找，并获取该邮件的国家/地区或甚至是源城市。

Serilog 是用于 .NET 语言的日志记录库，用于实现参数化日志记录。 参数不会生成嵌入字段的文本日志消息，而是将参数保持独立。 这允许更智能的筛选和搜索。 图7-7 显示了用于写入 Logstash 的示例 Serilog 配置。

```csharp
var log = new LoggerConfiguration()
         .WriteTo.Http("http://localhost:8080")
         .CreateLogger();
```

**图 7-7**。 用于将日志信息直接写入 logstash over HTTP 的 Serilog config

Logstash 将使用如图7-8 中所示的配置。

```
input {
    http {
        #default host 0.0.0.0:8080
        codec => json
    }
}

output {
    elasticsearch {
        hosts => "elasticsearch:9200"
        index=>"sales-%{+xxxx.ww}"
    }
}
```

**图 7-8**。 使用 Serilog 中的日志的 Logstash 配置

对于不需要进行大量的日志操作的情况，有一种替代方法 Logstash 称为 [节拍](https://www.elastic.co/products/beats)。 节拍是一系列工具，可以将各种数据从日志收集到网络数据和正常运行时间信息。 许多应用程序将同时使用 Logstash 和节拍。

Logstash 收集了日志后，需要将其放在某个位置。 尽管 Logstash 支持多个不同的输出，但其中一个比较激动人心的输出是弹性搜索。

## <a name="elastic-search"></a>弹性搜索

弹性搜索是一个功能强大的搜索引擎，可以在日志到达时对其进行索引。 它对日志快速运行查询。 弹性搜索可以处理大量日志，在极大情况下，可以在多个节点之间横向扩展。

精心设计为包含参数或通过 Logstash 处理将参数拆分为包含参数的日志消息可以直接查询，因为 Elasticsearch 会保留此信息。

`jill@example.com`在图7-9 中将会搜索访问的前10页的查询。

```json
"query": {
    "match": {
      "user": "jill@example.com"
    }
  },
  "aggregations": {
    "top_10_pages": {
      "terms": {
        "field": "page",
        "size": 10
      }
    }
  }
```

**图 7-9**。 用于查找用户访问的前10页的 Elasticsearch 查询

## <a name="visualizing-information-with-kibana-web-dashboards"></a>通过 Kibana web 仪表板可视化信息

堆栈的最终组件为 Kibana。 此工具用于在 web 仪表板中提供交互式可视化对象。 即使是非技术用户，也可以进行精心设计的仪表板。 驻留在 Elasticsearch 索引中的大部分数据都可以包含在 Kibana 仪表板中。 单个用户的仪表板可能不同，Kibana 通过允许用户特定的仪表板启用此自定义。

## <a name="installing-elastic-stack-on-azure"></a>在 Azure 上安装弹性堆栈

可以通过多种方式在 Azure 上安装弹性堆栈。 与往常一样，可以 [预配虚拟机并直接在其上安装弹性堆栈](/azure/virtual-machines/linux/tutorial-elasticsearch)。 此选项由一些经验丰富的用户首选，因为它提供了最高的可定制性。 在基础结构上部署为服务会带来巨大的管理开销，迫使那些采用该路径的用户获得与基础结构相关的所有任务的所有权（例如，保护计算机并随时保持修补程序的最新）。

开销较少的选项是使用已在其上配置弹性堆栈的众多 Docker 容器之一。 这些容器可以放入现有的 Kubernetes 群集，并与应用程序代码一起运行。 [Sebp/elk](https://elk-docker.readthedocs.io/)容器是一种记录完善且经过测试的弹性堆栈容器。

另一种方法是 [最近发布的 ELK 产品/服务](https://devops.com/logz-io-unveils-azure-open-source-elk-monitoring-solution/)。

## <a name="references"></a>参考

- [在 Azure 上安装弹性堆栈](/azure/virtual-machines/linux/tutorial-elasticsearch)

>[!div class="step-by-step"]
>[上一页](observability-patterns.md)
>[下一页](monitoring-azure-kubernetes.md)
