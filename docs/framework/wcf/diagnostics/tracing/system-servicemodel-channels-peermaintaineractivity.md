---
title: System.ServiceModel.Channels.PeerMaintainerActivity
ms.date: 03/30/2017
ms.assetid: ef28d086-d7fb-4e81-82e9-45a54647783b
ms.openlocfilehash: c1e82611bb776531ad3e3d60283d970602eb7f15
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293021"
---
# <a name="systemservicemodelchannelspeermaintaineractivity"></a>System.ServiceModel.Channels.PeerMaintainerActivity

PeerMaintainer 模块正在执行特定操作（详细信息包含在跟踪消息正文中）。  
  
## <a name="description"></a>描述  

 此跟踪在各种 PeerMaintainer 操作期间发生。  
  
 PeerMaintainer 是 PeerNode 的内部组件。 每隔一分钟，或者每当接收到 32 条消息，该组件就向它的邻居发送一条 LinkUtility 消息，该消息中包含有关交换了多少条消息以及其中有多少条有用消息（不重复也未经篡改的消息）的统计信息。 这有助于确定特定邻居的链接利用率。 该维护组件大约每五分钟检查一次邻居连接的运行状况。 如果邻居连接的数目超过理想的数值，该维护组件将去除用处最小的一些连接。 如果连接数目不足，该维护组件将获取新的连接。  
  
## <a name="see-also"></a>另请参阅

- [跟踪](index.md)
- [使用跟踪来排除应用程序故障](using-tracing-to-troubleshoot-your-application.md)
- [管理和诊断](../index.md)
