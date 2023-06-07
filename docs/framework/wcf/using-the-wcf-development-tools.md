---
title: 使用 WCF 开发工具
ms.date: 03/30/2017
ms.assetid: 054adb87-c244-4d5a-83d1-0b2b44bd454b
ms.openlocfilehash: 48733d27a72e864760c371e8b90e0af7b74273a0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273641"
---
# <a name="using-the-wcf-development-tools"></a>使用 WCF 开发工具

本部分介绍可帮助你开发 WCFservice 的 Visual Studio 开发工具。  
  
 您可以使用 Visual Studio 模板作为基础来快速生成自己的服务，然后使用 WCF 服务自动主机和 WCF 测试客户端来调试和测试您的服务。 通过一起使用这些工具，可以快速完美地完成调试和测试过程，无需在早期阶段提交给承载模型。  

 > [!NOTE]
 > 从 Visual Studio 2017 开始，默认情况下不安装 WCF 开发工具。 若要使用这些功能，必须确保在 Visual Studio 安装程序中选择 Windows Communication Foundation 组件。
  
## <a name="the-wcf-developer-tools"></a>WCF 开发人员工具  

 [WCF Visual Studio 模板](wcf-vs-templates.md)  
  
 你可以使用 Visual Studio 中预定义的 Visual Studio 项目和项模板快速生成 WCF 服务和应用程序。  
  
 [WCF 服务主机 (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)  
  
 WCF 服务自动主机 ( # A0) 使你可以启动 Visual Studio 调试器 (F5) 来自动承载和测试已实现的服务。 然后，你可以使用 WCF 测试客户端 ( # A0) 或你自己的客户端来测试服务，以查找并解决任何潜在错误。  
  
 [WCF 测试客户端 (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)  
  
 WCF 测试客户端 ( # A0) 是一个 GUI 工具，它允许输入任意类型的参数、将该输入提交给服务并查看服务发回的响应。 与 WCF 服务自动主机结合使用时，可以提供无缝的服务测试体验。  
  
 [从 XML 生成数据类型类](generating-data-type-classes-from-xml.md)  
  
 可将存储在剪贴板中的 XML 数据粘贴到代码页中。 数据中定义的类将被转换为代码类型。  
  
## <a name="using-the-tools-without-administrator-privilege"></a>在无管理员权限的情况下使用工具  

 为了使无管理员权限的用户能够开发 WCF 服务，在 http://+:8731/Design_Time_Addresses 安装 Visual Studio 的过程中，将为命名空间 "" 创建一个 ACL (访问控制列表) 。 该 ACL 被设置为“(UI)”，这将包括登录到此计算机的所有交互用户。 管理员可以在此 ACL 中添加或移除用户，或者打开其他端口。此 ACL 支持 WCF 或 WF 模板以其各自的默认配置发送和接收数据。 它还使用户可以使用 WCF 服务自动主机 ( # A0) ，而无需授予其管理员权限。  
  
 你可以使用提升的管理员帐户在 Windows Vista 中使用 Netsh.exe 工具来修改访问权限。 下面是使用 Netsh.exe 的示例。  
  
```console  
netsh http add urlacl url=http://+:8001/MyService user=<domain>\<user>  
```  
  
 有关 Netsh.exe 的详细信息，请参阅 [如何使用 Netsh.exe 工具和 Command-Line 开关](/previous-versions/tn-archive/bb490939(v=technet.10))。  
  
## <a name="see-also"></a>另请参阅

- [WCF Visual Studio 模板](wcf-vs-templates.md)
- [WCF 服务主机 (WcfSvcHost.exe)](wcf-service-host-wcfsvchost-exe.md)
- [WCF 测试客户端 (WcfTestClient.exe)](wcf-test-client-wcftestclient-exe.md)
