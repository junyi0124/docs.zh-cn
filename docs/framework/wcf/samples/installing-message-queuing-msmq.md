---
title: 安装“消息队列 (MSMQ)”
description: 了解如何将消息队列4.0 和消息队列3.0 作为一次性安装过程的一部分，与 WFC 示例一起使用。
ms.date: 03/30/2017
ms.assetid: 7ddcd497-3e04-427e-bc04-3610ad98b01e
ms.openlocfilehash: bf33a5cd899bf2d7ef4947c51ac1723c8eb80c29
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281867"
---
# <a name="installing-message-queuing-msmq"></a>安装“消息队列 (MSMQ)”

以下过程介绍如何安装“消息队列 4.0”和“消息队列 3.0”。  
  
> [!NOTE]
> 在 Windows XP 和 Windows Server 2003 中，消息队列4.0 不可用。  
  
#### <a name="to-install-message-queuing-40-on-windows-server-2008-or-windows-server-2008-r2"></a>在 Windows Server 2008 or Windows Server 2008 R2 上安装消息队列 4.0  
  
1. 在服务器管理器中，单击 " **功能**"。  
  
2. 在右侧窗格中的 " **功能摘要**" 下，单击 " **添加功能**"。  
  
3. 在生成的窗口中，展开 " **消息队列**"。  
  
4. 展开 " **消息队列服务**"。  
  
5. 单击 " **目录服务集成** " (对于加入到域) 的计算机，然后单击 " **HTTP 支持**"。  
  
6. 单击 " **下一步**"，然后单击 " **安装**"。  
  
#### <a name="to-install-message-queuing-40-on-windows-7-or-windows-vista"></a>在 Windows 7 或 Windows Vista 上安装消息队列 4.0  
  
1. 打开“控制面板”  
  
2. 单击 " **程序** "，然后在 " **程序和功能**" 下，单击 " **打开和关闭 Windows 功能**"。  
  
3. 展开“Microsoft Message Queue (MSMQ) 服务器”，展开“Microsoft Message Queue (MSMQ) 服务器核心”，然后选中对应于以下要安装的“消息队列”功能的复选框：  
  
    - MSMQ Active Directory 域服务集成（用于加入域的计算机）。  
  
    - MSMQ HTTP 支持。  
  
4. 单击" **确定**"。  
  
5. 如果系统提示您重新启动计算机，请单击 **"确定"** 以完成安装。  
  
#### <a name="to-install-message-queuing-30-on-windows-xp-and-windows-server-2003"></a>在 Windows XP 和 Windows Server 2003 上安装消息队列 3.0  
  
1. 打开“控制面板”  
  
2. 单击 " **添加删除程序** "，然后单击 " **添加 Windows 组件**"。  
  
3. 选择 "消息队列"，然后单击 " **详细信息**"。  
  
    > [!NOTE]
    > 如果运行的是 Windows Server 2003，请选择 "应用程序服务器" 以访问消息队列。  
  
4. 确保在详细信息页上已选中“MSMQ HTTP 支持”选项。  
  
5. 单击 **"确定"** 退出详细信息页，然后单击 " **下一步**"。 完成安装。  
  
6. 如果系统提示您重新启动计算机，请单击 **"确定"** 以完成安装。  
  
## <a name="see-also"></a>另请参阅

- [设置说明](set-up-instructions.md)
