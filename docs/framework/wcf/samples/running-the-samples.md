---
title: 运行 Windows Communication Foundation 示例
ms.date: 03/30/2017
ms.assetid: db8a83da-95c1-4a21-a9d2-48caeb6398ea
ms.openlocfilehash: 3a12128541739ba5c380be2efc291b9b419cab12
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262666"
---
# <a name="running-the-windows-communication-foundation-samples"></a>运行 Windows Communication Foundation 示例

Windows Communication Foundation (WCF) 示例可以在一台计算机或跨计算机配置中运行。 示例在提供时就可用于在单机上运行。 在跨计算机配置中，必须修改示例的配置文件设置。 下面的过程说明如何用同一计算机配置和跨计算机配置来运行示例。 请注意，Internet 信息服务 (IIS) 中承载的服务和自承载示例在步骤上有所不同。 大多数示例承载于 IIS 中，请参见示例自述文件信息以确定示例的承载方式。  
  
 在 Windows Vista 上，不在 IIS 中承载的示例需要提升的权限才能向 Http.sys 注册侦听器。 以服务运行所用的帐户使用 Httpcfg.exe 注册服务侦听地址，或从以管理员特权运行的命令提示符启动服务。  
  
> [!NOTE]
> 在生成或运行任何 WCF 示例之前，请确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
### <a name="to-run-the-sample-on-the-same-machine"></a>在同一计算机上运行示例  
  
1. 如果服务由 IIS 承载，请确保可以通过输入以下地址使用浏览器访问服务： `http://localhost/servicemodelsamples/service.svc` 。 在响应中应显示确认页。 如果未显示 "确认" 页，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
2. 如果服务是自承载的，请从 \service\bin（在语言特定文件夹内）中运行 Service.exe。 服务活动显示在服务控制台窗口上。  
  
3. \\从 \client\bin 的特定于语言的文件夹运行 Client.exe。 客户端活动将显示在客户端控制台窗口上。  
  
4. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-run-the-sample-across-machines"></a>跨计算机运行示例  
  
1. 如果服务在 IIS 中承载：  
  
    1. 在服务计算机上创建一个名为 ServiceModelSamples 的虚拟目录。 [Windows Communication Foundation 示例的一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)中 Setupvroot.bat 包含的批处理文件可用于创建磁盘目录和虚拟目录。  
  
    2. 从 %SystemDrive%\Inetpub\wwwroot\servicemodelsamples 中将服务程序文件复制到服务计算机上的 ServiceModelSamples 虚拟目录中。 确保在 \bin 目录中包括这些文件。  
  
    3. 测试是否可使用浏览器从客户端计算机访问服务。  
  
     如果服务是自承载的：  
  
    1. 在服务计算机上，创建一个保存服务文件的目录。  
  
    2. 将 \service\bin\ 文件夹（在语言特定文件夹内）中的服务程序文件复制到服务计算机上。  
  
    3. 在服务配置文件中，更改终结点定义的地址值以与服务的新地址相匹配。 在地址中将任何对“localhost”的引用替换为一个完全限定的域名。  
  
    4. 在命令提示符处启动 Service.exe。  
  
2. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的客户端程序文件复制到客户端计算机上。  
  
3. 设置终结点地址。  
  
    1. 如果服务未在域帐户下运行，请打开客户端配置文件并更改终结点定义的地址值以与服务的新地址相匹配。 在地址中将任何对“localhost”的引用替换为一个完全限定的域名。  
  
    2. 如果服务正在域帐户下运行，则通过针对服务运行 Svcutil.exe 来重新生成客户端配置。 有关运行 Svcutil.exe 的详细信息，请参阅 [生成 Windows Communication Foundation 示例](building-the-samples.md)。 使用生成的文件来代替示例中的配置文件。 生成的配置文件有附加的标识信息，并包含连接到服务终结点所需的所有设置，即使这些设置是默认设置。 有关标识信息的详细信息，请参阅 [服务标识和身份验证](../feature-details/service-identity-and-authentication.md)和 [\<identity>](../../configure-apps/file-schema/wcf/identity.md) 。  
  
4. 在客户端计算机上，在命令提示符下启动 Client.exe。  
  
### <a name="to-debug-a-service"></a>调试服务  
  
1. 使用 " **生成** " 菜单或 CTRL + SHIFT + B (客户端和服务) 生成解决方案。  
  
2. 如果服务在 IIS 中承载：  
  
    1. 通过输入地址，使用浏览器激活服务 `http://localhost/servicemodelsamples/service.svc` 。  
  
    2. 在解决方案中，选择 " **调试** " 菜单和 " **附加到进程** " 菜单项。  
  
    3. 选择“显示所有用户的进程”复选框  。  
  
    4. 选择主机辅助进程 W3wp.exe 以进行调试（在 Windows XP 上选择 ASPNet_wp.exe on）。  
  
3. 现在可以在服务代码中设置断点并启用针对异常的断点。  
  
4. 右键单击客户端项目项，然后选择 " **调试**" 和 " **启动新实例**"。  
  
### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 出于安全目的，如果服务承载于 IIS 中，请在示例结束后删除虚拟目录定义和在安装步骤中授予的权限。  
  
## <a name="see-also"></a>另请参阅

- [生成 Windows Communication Foundation 示例](building-the-samples.md)
- [WCF 示例的疑难解答提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))
