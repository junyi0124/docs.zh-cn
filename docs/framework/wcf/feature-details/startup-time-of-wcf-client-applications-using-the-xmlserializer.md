---
title: 如何：使用 XmlSerializer 改善 WCF 客户端应用程序的启动时间
ms.date: 03/30/2017
ms.assetid: 21093451-0bc3-4b1a-9a9d-05f7f71fa7d0
ms.openlocfilehash: ac54a766161db146331a3e072b97822b609344c0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96246376"
---
# <a name="how-to-improve-the-startup-time-of-wcf-client-applications-using-the-xmlserializer"></a>如何：使用 XmlSerializer 改善 WCF 客户端应用程序的启动时间

如果服务和客户端应用程序使用可用 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化的数据类型，则会在运行时生成并编译这些数据类型的序列化代码，从而导致启动性能降低。  
  
> [!NOTE]
> 预生成的序列化代码只能在客户端应用程序中使用，不能在服务中使用。  
  
 通过从应用程序的编译的程序集生成必要的序列化代码， [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 可提高这些应用程序的启动性能。 Svcutil.exe 会为已编译的应用程序集的服务协定中使用的所有数据生成序列化代码，该代码可使用 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化。 使用 <xref:System.Xml.Serialization.XmlSerializer> 的服务和操作协定用 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 进行标记。  
  
### <a name="to-generate-xmlserializer-serialization-code"></a>生成 XmlSerializer 序列化代码  
  
1. 将服务或客户端代码编译为一个或多个程序集。  
  
2. 打开一个 SDK 命令提示符窗口。  
  
3. 在命令提示符处，使用下面的格式启动 Svcutil.exe 工具。  
  
    ```console  
    svcutil.exe /t:xmlSerializer  <assemblyPath>*  
    ```  
  
     `assemblyPath` 参数指定包含服务协定类型的程序集的路径。 Svcutil.exe 会为已编译的应用程序集的服务协定中使用的所有数据生成序列化代码，该代码可使用 <xref:System.Xml.Serialization.XmlSerializer> 进行序列化。  
  
     Svcutil.exe 只能生成 C# 序列化代码。 为每个输入程序集生成一个源代码文件。 不能使用 **/language** 开关更改所生成代码的语言。  
  
     若要指定依赖程序集的路径，请使用 **/reference** 选项。  
  
4. 通过使用下列选项之一使生成的序列化代码可供你的应用程序使用：  
  
    1. 将生成的序列化代码编译为名称为 [*原始程序集*] .XmlSerializers.dll 的单独程序集 (例如，MyApp.XmlSerializers.dll) 。 您的应用程序必须能够加载该程序集，而该程序集必须使用与原始程序集相同的密钥进行签名。 如果您要重新编译原始程序集，则必须重新生成序列化程序集。  
  
    2. 将生成的序列化代码编译成单独的程序集，并在使用 <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute> 的服务协定上使用 <xref:System.ServiceModel.XmlSerializerFormatAttribute>。 将 <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.AssemblyName%2A> 或 <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.CodeBase%2A> 属性设置为指向已编译的序列化程序集。  
  
    3. 将生成的序列化代码编译到您的应用程序集内并将 添加到使用  的服务协定中。 不要设置 <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.AssemblyName%2A> 或 <xref:System.Xml.Serialization.XmlSerializerAssemblyAttribute.CodeBase%2A> 属性。 默认序列化程序集假定为当前程序集。  
  
### <a name="to-generate-xmlserializer-serialization-code-in-visual-studio"></a>在 Visual Studio 中生成 XmlSerializer 序列化代码  
  
1. 在 Visual Studio 中创建 WCF 服务和客户端项目。 然后，将服务引用添加到客户端项目。  
  
2. 将添加 <xref:System.ServiceModel.XmlSerializerFormatAttribute> 到 " *reference.cs* " 文件中的 " **serviceReference**" 下的客户端应用项目中的服务协定  ->  **reference.svcmap**。 请注意，需要在 **解决方案资源管理器** 中显示所有文件以查看这些文件。  
  
3. 生成客户端应用。  
  
4. 使用 "工作项 [元数据实用工具" 工具 ( # A0)](../servicemodel-metadata-utility-tool-svcutil-exe.md) 通过使用命令创建预先生成的序列化程序 *.cs* 文件：  
  
    ```console  
    svcutil.exe /t:xmlSerializer  <assemblyPath>*  
    ```  
  
     AssemblyPath 参数指定 WCF 客户端程序集的路径。  
  
     如：  
  
    ```console  
    svcutil.exe /t:xmlSerializer wcfclient.exe  
    ```  
  
     将生成 *WCFClient.XmlSerializers.dll .cs* 文件。  
  
5. 编译预生成的序列化程序集。  
  
     根据上一步骤中的示例，编译命令如下所示：  
  
    ```console  
    csc /r:wcfclient.exe /out:WCFClient.XmlSerializers.dll /t:library WCFClient.XmlSerializers.dll.cs  
    ```  
  
     请确保生成的 *WCFClient.XmlSerializers.dll* 位于与客户端应用相同的目录中，这在此情况下 *WCFClient.exe* 。  
  
6. 照常运行客户端应用。 将使用预先生成的序列化程序集。  
  
## <a name="example"></a>示例  

 下面的命令为 `XmlSerializer` 类型生成程序集中所有服务协定使用的序列化类型。  
  
```console  
svcutil /t:xmlserializer myContractLibrary.exe  
```  
  
## <a name="see-also"></a>另请参阅

- [ServiceModel 元数据实用工具 (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
