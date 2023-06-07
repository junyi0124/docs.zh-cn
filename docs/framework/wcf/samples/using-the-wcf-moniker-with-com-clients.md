---
title: 对 COM 客户端使用 WCF 标记
ms.date: 03/30/2017
ms.assetid: e2799bfe-88bd-49d7-9d6d-ac16a9b16b04
ms.openlocfilehash: eb2f14db8b58fd182bbe711bf559055659a02652
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243685"
---
# <a name="using-the-wcf-moniker-with-com-clients"></a>对 COM 客户端使用 WCF 标记

此示例演示如何使用 Windows Communication Foundation (WCF) 服务名字对象将 Web 服务集成到基于 COM 的开发环境中，例如 Microsoft Office Visual Basic for Applications (Office VBA) 或 Visual Basic 6.0。 本示例由 Windows 脚本宿主客户端 (.vbs)、客户端支持库 (.dll) 和 Internet 信息服务 (IIS) 承载的服务库 (.dll) 组成。 该服务是一个计算器服务，COM 客户端将对服务调用数学运算（加、减、乘和除）。 客户端活动显示在消息框窗口中。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Interop\COM`  
  
 该服务实现一个 `ICalculator` 协定，下面的代码示例对该协定进行了如下定义。  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface ICalculator  
{  
    [OperationContract]  
    double Add(double n1, double n2);  
    [OperationContract]  
    double Subtract(double n1, double n2);  
    [OperationContract]  
    double Multiply(double n1, double n2);  
    [OperationContract]  
    double Divide(double n1, double n2);  
}  
```  
  
 示例演示三种使用标记的备选方法：  
  
- 类型化协定 - 该协定在客户端计算机上注册为 COM 可见类型。  
  
- WSDL 协定 - 该协定以 WSDL 文档的形式提供。  
  
- 元数据交换协定 - 该协定可在运行时从元数据交换 (MEX) 终结点进行检索。  
  
## <a name="typed-contract"></a>类型化协定  

 若要对类型化协定用法使用标记，必须向 COM 注册服务协定的经适当属性化的类型。 首先，必须使用 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md)来生成客户端。 在客户端目录中通过命令提示符运行以下命令可以生成该类型化代理。  
  
```console  
svcutil.exe /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples http://localhost/servicemodelsamples/service.svc /out:generatedClient.cs  
```  
  
 此类必须包括在项目中，并且应将项目配置为在编译时生成一个 COM 可见的已签名程序集。 AssemblyInfo.cs 文件中应该包括下面的属性。  
  
```csharp
[assembly: ComVisible(true)]  
```  
  
 生成项目以后，通过使用 `regasm` 注册 COM 可见类型，如下面的示例所示。  
  
```console  
regasm.exe /tlb:CalcProxy.tlb client.dll  
```  
  
 创建的程序集应该添加到全局程序集缓存中。 虽然并不严格要求，但这样做可简化运行时定位程序集的过程。 下面的命令将程序集添加到全局程序集缓存中。  
  
```console  
gacutil.exe /i client.dll  
```  
  
> [!NOTE]
> 服务标记只要求进行类型注册，并不使用代理与服务通信。  
  
 ComCalcClient.vbs 客户端应用程序使用 `GetObject` 函数来构造服务的代理，使用服务标记语法指定服务的地址、绑定和协定。  
  
```vbscript
Set typedServiceMoniker = GetObject(  
"service4:address=http://localhost/ServiceModelSamples/service.svc, binding=wsHttpBinding,
contractType={9213C6D2-5A6F-3D26-839B-3BA9B82228D3}")  
```  
  
 标记使用的参数将会指定：  
  
- 服务终结点的地址。  
  
- 客户端应该用来与该终结点连接的绑定。 虽然可以在客户端配置文件中定义自定义绑定，但本例中使用了系统定义的 wsHttpBinding。 自定义绑定在与 Cscript.exe 处于同一目录的 Cscript.exe.config 文件中进行定义，以便可以与 Windows 脚本主机一起使用。  
  
- 在终结点受支持的协定的类型。 此类型是在上面生成并注册的类型。 由于 Visual Basic 脚本不提供强类型 COM 环境，因此必须指定协定的标识符。 此 GUID 是 CalcProxy.tlb 中的 `interfaceID`，可以通过使用 COM 工具（如 OLE/COM 对象查看器 (OleView.exe)）来查看。 对于强类型化环境（如 Office VBA 或 Visual Basic 6.0），添加对类型库的显式引用，然后声明代理对象的类型可用于替代协定参数。 这样还可在客户端应用程序开发过程中提供 IntelliSense 支持。  
  
 在用服务标记构造代理实例之后，客户端应用程序可以对此代理调用方法，这将生成调用相应服务操作的服务标记基础结构。  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "Typed service moniker: 100 + 15.99 = " & typedServiceMoniker.Add(100, 15.99)  
```  
  
 运行示例时，操作响应将显示在 Windows 脚本宿主消息框窗口中。 这说明了使用类型化名字对象发出 COM 调用以便与 WCF 服务进行通信的 COM 客户端。 虽然在客户端应用程序中使用了 COM，但与服务的通信仅由 Web 服务调用组成。  
  
## <a name="wsdl-contract"></a>WSDL 协定  

 使用具有 WSDL 协定的标记不需要注册任何客户端库，但必须通过带外机制（如使用浏览器访问服务的 WSDL 终结点）来检索服务的 WSDL 协定。 标记之后可以在执行时访问该协定。  
  
 ComCalcClient.vbs 客户端应用程序使用 `FileSystemObject` 访问保存在本地的 WSDL 文件，然后再次使用 `GetObject` 函数来构造服务的代理。  
  
```vbscript  
' Open the WSDL contract file and read it all into the wsdlContract string  
Const ForReading = 1  
Set objFSO = CreateObject("Scripting.FileSystemObject")  
Set objFile = objFSO.OpenTextFile("serviceWsdl.xml", ForReading)  
wsdlContract = objFile.ReadAll  
objFile.Close  
  
' Create a string for the service moniker including the content of the WSDL contract file  
wsdlMonikerString = "service4:address='http://localhost/ServiceModelSamples/service.svc'"  
wsdlMonikerString = wsdlMonikerString + ", binding=WSHttpBinding_ICalculator, bindingNamespace='http://Microsoft.ServiceModel.Samples'"  
wsdlMonikerString = wsdlMonikerString + ", wsdl='" & wsdlContract & "'"  
wsdlMonikerString = wsdlMonikerString + ", contract=ICalculator, contractNamespace='http://Microsoft.ServiceModel.Samples'"  
  
' Create the service moniker object  
Set wsdlServiceMoniker = GetObject(wsdlMonikerString)  
```  
  
 标记使用的参数将会指定：  
  
- 服务终结点的地址。  
  
- 客户端用于与该终结点连接的绑定和在其中定义该绑定的命名空间。 在本例中，使用 `wsHttpBinding_ICalculator`。  
  
- 定义协定的 WSDL。 在本例中是已从 serviceWsdl.xml 文件中读取的字符串。  
  
- 协定的名称和命名空间。 由于 WSDL 可能包含多个协定，因此需要此标识。  
  
    > [!NOTE]
    > 默认情况下，WCF 服务为使用的每个命名空间生成单独的 WSDL 文件。 这些文件通过使用 WSDL 导入构造连接。 由于标记需要单个 WSDL 定义，因此服务必须使用单个命名空间（如本示例中的演示），或者必须手动合并单独的文件。  
  
 在用服务标记构造代理实例之后，客户端应用程序可以对此代理调用方法，这将生成调用相应服务操作的服务标记基础结构。  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "WSDL service moniker: 145 - 76.54 = " & wsdlServiceMoniker.Subtract(145, 76.54)  
```  
  
 运行示例时，操作响应将显示在 Windows 脚本宿主消息框窗口中。 这说明了使用带有 WSDL 协定的名字对象进行 COM 调用以便与 WCF 服务进行通信的 COM 客户端。  
  
## <a name="metadata-exchange-contract"></a>元数据交换协定  

 和 WSDL 协定一样，使用具有 MEX 协定的标记不需要注册任何客户端。 服务的协定通过内部使用元数据交换在执行时进行检索。  
  
 ComCalcClient.vbs 客户端应用程序也使用 `GetObject` 函数来构造服务的代理。  
  
```vbscript  
' Create a string for the service moniker specifying the address to retrieve the service metadata from  
mexMonikerString = "service4:mexAddress='http://localhost/servicemodelsamples/service.svc/mex'"  
mexMonikerString = mexMonikerString + ", address='http://localhost/ServiceModelSamples/service.svc'"  
mexMonikerString = mexMonikerString + ", binding=WSHttpBinding_ICalculator, bindingNamespace='http://Microsoft.ServiceModel.Samples'"  
mexMonikerString = mexMonikerString + ", contract=ICalculator, contractNamespace='http://Microsoft.ServiceModel.Samples'"  
  
' Create the service moniker object  
Set mexServiceMoniker = GetObject(mexMonikerString)  
```  
  
 标记使用的参数将会指定：  
  
- 服务元数据交换终结点的地址。  
  
- 服务终结点的地址。  
  
- 客户端用于与该终结点连接的绑定和在其中定义该绑定的命名空间。 在本例中，使用 `wsHttpBinding_ICalculator`。  
  
- 协定的名称和命名空间。 由于 WSDL 可能包含多个协定，因此需要此标识。  
  
 在用服务标记构造代理实例之后，客户端应用程序可以对此代理调用方法，这将生成调用相应服务操作的服务标记基础结构。  
  
```vbscript  
' Call the service operations using the moniker object  
WScript.Echo "MEX service moniker: 9 * 81.25 = " & mexServiceMoniker.Multiply(9, 81.25)  
```  
  
 运行示例时，操作响应将显示在 Windows 脚本宿主消息框窗口中。 这表明，COM 客户端使用名字对象和 MEX 协定进行 COM 调用，以便与 WCF 服务进行通信。  
  
#### <a name="to-set-up-and-build-the-sample"></a>设置和生成示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 在 Visual Studio 开发人员命令提示中，打开语言特定文件夹下的 \client\bin 文件夹。  
  
    > [!NOTE]
    > 如果使用的是 Windows Vista、Windows Server 2008、Windows 7 或 Windows Server 2008 R2，请确保使用管理员权限运行命令提示符。  
  
4. 键入以 `tlbexp.exe client.dll /out:CalcProxy.tlb` 将 dll 导出到 tlb 文件中。 预期会出现“Type library exporter warning”（类型库导出程序警告），但由于不需要泛型类型，因此这不是问题。  
  
5. 键入 `regasm.exe /tlb:CalcProxy.tlb client.dll` 以向 COM 注册类型。 预期会出现“Type library exporter warning”（类型库导出程序警告），但由于不需要泛型类型，因此这不是问题。  
  
6. 键入以 `gacutil.exe /i client.dll` 将程序集添加到全局程序集缓存中。  
  
#### <a name="to-run-the-sample-on-the-same-computer"></a>在同一计算机上运行示例  
  
1. 通过键入以下地址来测试是否可以使用浏览器访问服务： `http://localhost/servicemodelsamples/service.svc` 。 在响应中应显示确认页。  
  
2. 运行 \client（在语言特定文件夹内）中的 ComCalcClient.vbs。 客户端活动将显示在消息框窗口中。  
  
3. 如果客户端和服务无法进行通信，请参阅 [WCF 示例的故障排除提示](/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))。  
  
#### <a name="to-run-the-sample-across-computers"></a>跨计算机运行示例  
  
1. 在服务计算机上创建一个名为 ServiceModelSamples 的虚拟目录。 可以使用示例中附带的 Setupvroot.bat 脚本来创建磁盘目录和虚拟目录。  
  
2. 从 %SystemDrive%\Inetpub\wwwroot\servicemodelsamples 中将服务程序文件复制到服务计算机上的 ServiceModelSamples 虚拟目录中。 确保在 \bin 目录中包括这些文件。  
  
3. 将 \client 文件夹（在语言特定文件夹内）中的客户端脚本文件复制到客户端计算机上。  
  
4. 在该脚本文件中，更改终结点定义的地址值以与服务的新地址相匹配。 在地址中将任何对“localhost”的引用替换为一个完全限定的域名。  
  
5. 将 WSDL 文件复制到客户端计算机。 在 WSDL 文件 serviceWsdl.xml 中，将地址中任何对“localhost”的引用替换为一个完全限定的域名。  
  
6. 将 \client\bin\ 文件夹（在语言特定文件夹内）中的 Client.dll 库复制到客户端计算机上的一个目录中。  
  
7. 在命令提示符下，定位到客户端计算机上的目标目录。 如果使用的是 Windows Vista 或 Windows Server 2008，请确保以管理员身份运行命令提示符。  
  
8. 键入以 `tlbexp.exe client.dll /out:CalcProxy.tlb` 将 dll 导出到 tlb 文件中。 预期会出现“Type library exporter warning”（类型库导出程序警告），但由于不需要泛型类型，因此这不是问题。  
  
9. 键入 `regasm.exe /tlb:CalcProxy.tlb client.dll` 以向 COM 注册类型。 在运行命令之前，请确保已将路径设置为包含的文件夹 `regasm.exe` 。  
  
10. 键入以 `gacutil.exe /i client.dll` 将程序集添加到全局程序集缓存中。 在运行命令之前，请确保已将路径设置为包含的文件夹 `gacutil.exe` 。  
  
11. 测试是否可使用浏览器从客户端计算机访问服务。  
  
12. 在客户端计算机上，启动 ComCalcClient.vbs。  
  
#### <a name="to-clean-up-after-the-sample"></a>运行示例后进行清理  
  
- 出于安全目的，请在示例结束后删除虚拟目录定义和在安装步骤中授予的权限。
