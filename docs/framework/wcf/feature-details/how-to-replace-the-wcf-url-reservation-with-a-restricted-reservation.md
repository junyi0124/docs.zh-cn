---
title: 如何：用受限预留替换 WCF URL 预留
ms.date: 03/30/2017
ms.assetid: 2754d223-79fc-4e2b-a6ce-989889f2abfa
ms.openlocfilehash: 52dc74ea1f8e86d6a92a2894b888b8d150ebf47c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96276056"
---
# <a name="how-to-replace-the-wcf-url-reservation-with-a-restricted-reservation"></a>如何：用受限预留替换 WCF URL 预留

URL 预留使你能够限制谁可以接收来自某个 URL 或某一组 URL 的消息。 预留由一个 URL 模板、一个访问控制列表 (ACL) 和一组标志组成。 URL 模板定义预留所影响的 URL。 有关如何处理 URL 模板的详细信息，请参阅 [路由传入的请求](/windows/win32/http/routing-incoming-requests)。 ACL 控制哪个用户或用户组允许接收来自指定 URL 的消息。 标志指示预留是赋予用户或用户组直接侦听 URL 的权限，还是将侦听权限委托给其他进程。  
  
 作为默认操作系统配置的一部分，Windows Communication Foundation (WCF) 会为端口80创建一个全局可访问的保留，以使所有用户都能运行使用双 HTTP 绑定进行双工通信的应用程序。 由于此预留的 ACL 适用于所有用户，因此管理员不能显式允许或禁止侦听某个 URL 或某一组 URL 的权限。 本主题介绍如何删除此预留，以及如何重新创建具有受限 ACL 的预留。  
  
在 Windows Vista 或 Windows Server 2008 上，你可以通过输入，从提升的命令提示符查看所有 HTTP URL 保留项 `netsh http show urlacl` 。 下面的示例演示 WCF URL 保留内容应类似于：

```output
Reserved URL : http://+:80/Temporary_Listen_Addresses/  
        User: \Everyone  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;WD)  
```

 当 WCF 应用程序使用 HTTP 双重绑定进行双工通信时，预订由使用的 URL 模板组成。 当通过 HTTP 双重绑定进行通信时，此窗体的 Url 用于 WCF 服务，以将消息发送回 WCF 客户端。 向每个用户授予侦听该 URL 的权限，而不将侦听权限委托给另一个进程。 最后，安全描述符定义语言 (SSDL) 中介绍了该 ACL。 有关 SSDL 的详细信息，请参阅 [ssdl](/windows/win32/secauthz/security-descriptor-definition-language)  
  
## <a name="to-delete-the-wcf-url-reservation"></a>删除 WCF URL 预留  
  
1. 单击 " **开始**"，指向 " **所有程序**"，单击 " **附件**"，右键单击 " **命令提示符** "，然后在出现的上下文菜单中单击 " **以管理员身份运行** "。 在 "用户帐户控制" (UAC) "窗口中单击" **继续** "，该窗口可能会要求权限继续。  
  
2. 在 `netsh http delete urlacl url=http://+:80/Temporary_Listen_Addresses/` 命令提示符窗口中键入。  
  
3. 如果成功删除了预留，将显示以下消息。 **已成功删除 URL 预留**  
  
## <a name="creating-a-new-security-group-and-new-restricted-url-reservation"></a>创建新的安全组和新的受限 URL 预留  

 若要用受限保留项替换 WCF URL 保留项，您必须首先创建一个新的安全组。 可以通过两种方法执行此操作：从命令提示符或从计算机管理控制台。 您只需要执行一次。  
  
### <a name="to-create-a-new-security-group-from-a-command-prompt"></a>从命令提示符创建新的安全组  
  
1. 单击 " **开始**"，指向 " **所有程序**"，单击 " **附件**"，右键单击 " **命令提示符** "，然后在出现的上下文菜单中单击 " **以管理员身份运行** "。 在 "用户帐户控制" (UAC) "窗口中单击" **继续** "，该窗口可能会要求权限继续。  
  
2. 在 `net localgroup "<security group name>" /comment:"<security group description>" /add` 命令提示符下键入。 将替换为 **\<security group name>** 要创建的安全组的名称，并 **\<security group description>** 使用安全组的适当说明进行替换。  
  
3. 如果成功创建了安全组，将显示以下消息。 **命令已成功完成。**  
  
### <a name="to-create-a-new-security-group-from-the-computer-management-console"></a>从计算机管理控制台创建新的安全组  
  
1. 依次单击 " **开始**"、 **"控制面板**"、" **管理工具**"，然后单击 " **计算机管理** " 以打开 "计算机管理" 控制台。 在 "用户帐户控制" (UAC) "窗口中单击" **继续** "，该窗口可能会要求权限继续。  
  
2. 单击 " **系统工具**"，单击 " **本地用户和组**"，右键单击 " **组** " 文件夹，然后在出现的上下文菜单上单击 " **新建组** "。 键入此新安全组所需的 **组名称**、 **说明** 和其他详细信息，然后单击 " **创建** " 按钮创建安全组。  
  
### <a name="to-create-the-restricted-url-reservation"></a>创建受限 URL 预留  
  
1. 单击 " **开始**"，指向 " **所有程序**"，单击 " **附件**"，右键单击 " **命令提示符** "，然后在出现的上下文菜单中单击 " **以管理员身份运行** "。 在 "用户帐户控制" (UAC) "窗口中单击" **继续** "，该窗口可能会要求权限继续。  
  
2. 在 `netsh http add urlacl url=http://+:80/Temporary_Listen_Addresses/ user="<machine name>\<security group name>` 命令提示符下键入。 将替换 **\<machine name>** 为必须在其中创建组的计算机名称，并将替换为 **\<security group name>** 前面创建的安全组的名称。  
  
3. 如果成功创建了预留，将显示以下消息。 **已成功添加 URL 保留** 项。
