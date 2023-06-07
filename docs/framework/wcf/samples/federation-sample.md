---
title: 联合示例
ms.date: 03/30/2017
ms.assetid: 7e9da0ca-e925-4644-aa96-8bfaf649d4bb
ms.openlocfilehash: 22d405620a77285ebe7a68fc151a8e8611df9b4d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283271"
---
# <a name="federation-sample"></a>联合示例

本示例演示联合安全。  
  
## <a name="sample-details"></a>示例详细信息  

 Windows Communication Foundation (WCF) 提供对通过部署联合安全体系结构的支持 `wsFederationHttpBinding` 。 `wsFederationHttpBinding` 提供安全可靠并且可互操作的绑定，该绑定中使用 HTTP 作为请求/回复通信的基础传输机制，并使用文本/XML 作为编码的联网格式。 有关 WCF 中的联合的详细信息，请参阅 [联合](../feature-details/federation.md)。  
  
 此方案包括 4 个部分：  
  
- BookStore 服务  
  
- BookStore STS  
  
- HomeRealm STS  
  
- BookStore 客户端  
  
 BookStore 服务支持两个操作：`BrowseBooks` 和 `BuyBook`。 它允许匿名访问 `BrowseBooks` 操作，但需要经过身份验证才能访问 `BuyBooks` 操作。 身份验证采用由 BookStore STS 颁发的令牌的形式。 BookStore 服务的配置文件使用 `wsFederationHttpBinding` 将客户端指向 BookStore STS。  
  
```xml  
<wsFederationHttpBinding>  
<!-- This is the Service binding for the BuyBooks endpoint. It redirects clients to the BookStore STS -->  
    <binding name='BuyBookBinding'>  
        <security mode="Message">  
            <message>  
                <issuerMetadata  
  address='http://localhost/FederationSample/BookStoreSTS/STS.svc/mex' >  
                    <identity>  
                        <dns value ='BookStoreSTS.com'/>  
                    </identity>  
                </issuerMetadata>  
            </message>  
        </security>  
    </binding>  
</wsFederationHttpBinding>  
```  
  
 BookStore STS 然后要求客户端使用 HomeRealm STS 颁发的令牌进行身份验证。 同样，BookStore STS 的配置文件使用 `wsFederationHttpBinding` 将客户端指向 HomeRealm STS。  
  
```xml  
<wsFederationHttpBinding>  
 <!-- This is the binding for the clients requesting tokens from this STS. It redirects clients to the HomeRealm STS -->  
    <binding name='BookStoreSTSBinding'>  
        <security mode='Message'>  
            <message>  
                <issuerMetadata  
 address='http://localhost/FederationSample/HomeRealmSTS/STS.svc/mex' >  
                    <identity>  
                        <dns value ='HomeRealmSTS.com' />  
                    </identity>  
                </issuerMetadata>  
            </message>  
        </security>  
    </binding>  
</wsFederationHttpBinding>  
```  
  
 访问 `BuyBook` 操作时的事件序列如下：  
  
1. 客户端使用 Windows 凭据向 HomeRealm STS 进行身份验证。  
  
2. HomeRealm STS 颁发一个令牌，该令牌可用于向 BookStore STS 进行身份验证。  
  
3. 客户端使用 HomeRealm STS 颁发的令牌向 BookStore STS 进行身份验证。  
  
4. BookStore STS 颁发一个令牌，该令牌可用于向 BookStore 服务进行身份验证。  
  
5. 客户端使用 BookStore STS 颁发的令牌向 BookStore 服务进行身份验证。  
  
6. 客户端访问 `BuyBook` 操作。  
  
 有关如何设置和运行此示例的信息，请参见以下说明。  
  
> [!NOTE]
> 若要运行此示例，您必须具有 **wwwroot** 目录的写入权限。  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 打开 SDK 命令窗口。 在示例路径中运行 Setup.bat。 这将创建示例所需的虚拟目录，并使用相应的权限安装所需的证书。  
  
    > [!NOTE]
    > Setup.bat 批处理文件设计为通过 Windows SDK 命令提示运行。 这要求 MSSDK 环境变量指向 SDK 的安装目录。 将在 Windows SDK 命令提示中自动设置此环境变量。 在 Windows Vista 上，必须确保已安装 IIS 6.0 管理兼容性，因为安装程序将使用 IIS 管理员脚本。 在 Windows Vista 上运行安装脚本需要管理员权限。  
  
2. 在 Visual Studio 中打开 FederationSample，并从 "**生成**" 菜单中选择 "**生成解决方案**"。 这将生成通用项目文件、Bookstore 服务、Bookstore STS、HomeRealm STS 并将它们部署到 IIS 中。 还将生成 Bookstore 客户端应用程序，并将可执行文件 BookStoreClient.exe 放入 FederationSample\BookStoreClient\bin\Debug 文件夹。  
  
3. 双击 BookStoreClient.exe。 将显示 BookStoreClient 窗口。  
  
4. 可以通过单击 " **浏览书籍**" 浏览书店中提供的书籍。  
  
5. 若要购买特定书籍，请在列表中选择该书籍，然后单击 " **购买书籍**"。 该应用程序将启动，并使用 Windows 身份验证和 HomeRealm 安全令牌服务进行身份验证。  
  
     此示例配置为允许用户购买价格等于或低于 15 美元的图书。 尝试购买价格超过 15 美元的图书将导致客户端从 BookStore 服务获得“拒绝访问”消息。  
  
    > [!NOTE]
    > 此示例不会在用户购买图书后更新其信用限额。 您可以在用户的（固定）信用限额内反复购买图书。  
  
#### <a name="to-clean-up"></a>清理  
  
1. 运行 Cleanup.bat。 这将删除安装过程中创建的虚拟目录，并移除安装过程中安装的证书。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\Federation`  
