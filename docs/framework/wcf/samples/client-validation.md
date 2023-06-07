---
title: 客户端验证
ms.date: 03/30/2017
ms.assetid: f0c1f805-1a81-4d0d-a112-bf5e2e87a631
ms.openlocfilehash: dee57e039d4510673b7205fd44fa300b24ac1df5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243308"
---
# <a name="client-validation"></a>客户端验证

服务通常发布元数据以启用客户端代理类型的自动生成和配置。 如果服务不受信任，客户端应用程序应该验证元数据是否符合客户端应用程序有关安全性、事务、服务协定类型等方面的策略。 下面的示例演示如何编写一个客户端终结点行为，用于验证服务终结点以确保可以安全地使用该服务终结点。  
  
 服务公开四个终结点。 第一个终结点使用 WSDualHttpBinding，第二个终结点使用 NTLM 身份验证，第三个终结点启用事务流，第四个终结点使用基于证书的身份验证。  
  
 客户端使用 <xref:System.ServiceModel.Description.MetadataResolver> 类来检索服务的元数据。 客户端强制实施禁用双工绑定、NTLM 身份验证和使用验证行为的事务流的策略。 对于 <xref:System.ServiceModel.Description.ServiceEndpoint> 从服务的元数据导入的每个实例，客户端应用程序在 `InternetClientValidatorBehavior` <xref:System.ServiceModel.Description.ServiceEndpoint> 尝试使用 Windows Communication Foundation (WCF) 客户端连接到终结点之前，会向添加一个终结点行为的实例。 在调用服务上的任何操作之前，会运行该行为的 `Validate` 方法，并通过引发 `InvalidOperationExceptions` 强制实施客户端的策略。  
  
### <a name="to-build-the-sample"></a>生成示例  
  
1. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例  
  
1. 使用管理员权限打开 Visual Studio 开发人员命令提示，并从示例安装文件夹中运行 Setup.bat。 这将安装运行示例所需的所有证书。  
  
2. 从 \service\bin\Debug 运行服务应用程序。  
  
3. 从 \client\bin\Debug 运行客户端应用程序。 客户端活动将显示在客户端控制台应用程序上。  
  
4. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
5. 在运行完该示例后运行 Cleanup.bat 移除证书。 其他安全示例使用相同的证书。  
  
### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在服务器上，在 Visual Studio 开发人员命令提示中，以管理员权限运行，请键入 `setup.bat service` 。 使用参数运行将使用 `setup.bat` `service` 计算机的完全限定的域名创建一个服务证书，并将服务证书导出到名为的文件。  
  
2. 在服务器上，编辑 App.config 以反映新证书名称。 也就是说，将 `findValue` 元素中的属性更改 [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-clientcredentials-element.md) 为计算机的完全限定域名。  
  
3. 将服务目录中的 Service.cer 文件复制到客户端计算机上的客户端目录中。  
  
4. 在客户端上，使用管理员权限打开 Visual Studio 开发人员命令提示，然后键入 `setup.bat client` 。 `setup.bat`使用参数运行将 `client` 创建一个名为 Client.com 的客户端证书，并将客户端证书导出到名为的文件。  
  
5. 在 client.cs 文件中更改 MEX 终结点的地址值和用于设置默认服务器证书的 `findValue` 以与服务的新地址相匹配。 通过使用服务器的完全限定域名替换 localhost 来执行此操作。 Rebuild。  
  
6. 将客户端目录中的 Client.cer 文件复制到服务器上的服务目录中。  
  
7. 在客户端上，在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行 ImportServiceCert.bat。 这会将 Service.cer 文件中的服务证书导入 CurrentUser – TrustedPeople 存储区。  
  
8. 在服务器上，在使用管理员特权打开的 Visual Studio 开发人员命令提示中运行 ImportClientCert.bat。 这会将 Client.cer 文件中的客户端证书导入 LocalMachine - TrustedPeople 存储区。  
  
9. 在服务计算机上，在 Visual Studio 中生成服务项目并运行 service.exe。  
  
10. 在客户端计算机上，运行 client.exe。  
  
    1. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 运行完示例后运行示例文件夹中的 Cleanup.bat。  
  
    > [!NOTE]
    > 此脚本不会在跨计算机运行此示例时移除客户端上的服务证书。 如果你已运行跨计算机使用证书的 WCF 示例，请确保清除已安装在 CurrentUser-TrustedPeople 存储中的服务证书。 为此，请使用以下命令：`certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>. For example: certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`。  
  
## <a name="see-also"></a>另请参阅

- [使用元数据](../feature-details/using-metadata.md)
