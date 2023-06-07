---
title: 关于 System.Net.PeerToPeer.Collaboration 命名空间
ms.date: 03/30/2017
ms.assetid: b5d8c1c1-6844-4947-9759-c7f1b564bded
ms.openlocfilehash: 54eaf1a99aab8b1db61f4b46237d57104c9d1a44
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250705"
---
# <a name="about-the-systemnetpeertopeercollaboration-namespace"></a>关于 System.Net.PeerToPeer.Collaboration 命名空间

<xref:System.Net.PeerToPeer.Collaboration> 命名空间提供用于通过对等协作基础结构实现对等协作活动的类和 API。  
  
## <a name="classes"></a>类  

 在对等协作活动的实现中所使用的主要类包括：  
  
- <xref:System.Net.PeerToPeer.Collaboration.ContactManager>，可用于存储对等联系人。  
  
- <xref:System.Net.PeerToPeer.Collaboration.PeerApplication>，可在其中进行协作，如游戏、聊天客户端或会议解决方案。  
  
- 将在活动中进行协作的对等机。  这些对等机可以表示为 <xref:System.Net.PeerToPeer.Collaboration.PeerContact>、<xref:System.Net.PeerToPeer.Collaboration.PeerNearMe> 或 <xref:System.Net.PeerToPeer.Collaboration.PeerEndPoint> 对象。  
  
- <xref:System.Net.PeerToPeer.Collaboration.PeerCollaboration> 静态类本身，用于指定可用的应用程序和参与其中的对等机。  
  
 <xref:System.Net.PeerToPeer.Collaboration.PeerContact.Invite%2A> 方法用于将对等机邀请至协作会话。  针对向与协作会话关联的应用程序、对象或状态信息发送更新信号的事件，调用对等机可订阅另一个对等机。 状态类指定 <xref:System.Net.PeerToPeer.Collaboration.Peer> 是否可用于协作，<xref:System.Net.PeerToPeer.Collaboration.PeerScope> 类用于指定对等机可允许的参与程度：<xref:System.Net.PeerToPeer.Collaboration.PeerScope.Internet>（全局）、<xref:System.Net.PeerToPeer.Collaboration.PeerScope.NearMe>（子网）或 <xref:System.Net.PeerToPeer.Collaboration.PeerScope.None>。  
  
 协作会话由四个步骤组成：  
  
- 发现。 发现或发布应用程序、对等机和状态信息。  例如，在本地子网上查找安装了相同游戏的其他人。  
  
- 邀请。 发送和接受远程对等机的安全邀请以启动或加入 <xref:System.Net.PeerToPeer.Collaboration.PeerCollaboration> 会话。  
  
- 联系人管理。 将发现的对等机作为联系人添加到 <xref:System.Net.PeerToPeer.Collaboration.ContactManager>。  
  
- 通信。 建立通信后，请使用 <xref:System.Net> API、<xref:System.Net.PeerToPeer> API 或 Windows Communication Foundation 对等通道类来进行多方通信。  
  
 例如，主机对等机启动协作会话并利用 <xref:System.Net.PeerToPeer.Collaboration.ContactManager.CreateContact%2A> 方法将远程对等机和其中一个本地对等机添加到主机对等机的联系人管理中。  这三位用户随后便可参与到他们自己的专用协作会话。  
  
 典型的 P2P 应用程序包括：用于协作进行笔记或白板记录的电话会议、无服务器的聊天应用程序、交互式播发和联机游戏会话。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.PeerToPeer.Collaboration>
