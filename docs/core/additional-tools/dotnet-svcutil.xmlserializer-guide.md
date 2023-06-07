---
title: 使用 dotnet-svcutil.xmlserializer
description: 了解如何使用 `dotnet-svcutil.xmlserializer` NuGet 包为 .NET Core 项目预生成序列化程序集。
author: huanwu
ms.date: 11/27/2018
ms.openlocfilehash: 14bb2e8a85ec35f08b0a83b9734a64d751074f1b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84284320"
---
# <a name="using-dotnet-svcutilxmlserializer-on-net-core"></a>在 .NET Core 上使用 dotnet-svcutil.xmlserializer

`dotnet-svcutil.xmlserializer` NuGet 包可以为 .NET Core 项目预生成序列化程序集。 它为客户端应用程序中由 WCF 服务协定使用的且可由 XmlSerializer 序列化的类型预生成 C# 序列化代码。 当序列化或反序列化这些类型的对象时，这会提高 XML 序列化的启动性能。

## <a name="prerequisites"></a>先决条件

* [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本
* 你最喜欢的代码编辑器

可以使用命令 `dotnet --info` 检查已安装哪些版本的 .NET Core SDK 和运行时。

## <a name="getting-started"></a>入门

在 .NET Core 控制台应用程序中使用 `dotnet-svcutil.xmlserializer`：

1. 在 .NET Framework 中使用默认模板“WCF 服务应用程序”创建一个名为“MyWCFService”的 WCF 服务。 在服务方法上添加 `[XmlSerializerFormat]` 属性，如下所示：

   ```csharp
    [ServiceContract]
    public interface IService1
    {
        [XmlSerializerFormat]
        [OperationContract(Action = "http://tempuri.org/IService1/GetData", ReplyAction = "http://tempuri.org/IService1/GetDataResponse")]
        string GetData(int value);
    }
    ```

2. 创建 .NET Core 控制台应用程序作为面向 .NET Core 2.1 或更高版本的 WCF 客户端应用程序。 例如，使用以下命令创建名为“MyWCFClient”的应用：

    ```dotnetcli
    dotnet new console --name MyWCFClient
    ```

    要确保项目面向 .NET Core 2.1 或更高版本，请检查项目文件中的 `TargetFramework` XML 元素：

    ```xml
    <TargetFramework>netcoreapp2.1</TargetFramework>
    ```

3. 通过运行以下命令将包引用添加到 `System.ServiceModel.Http`：

    ```dotnetcli
    dotnet add package System.ServiceModel.Http
    ```

4. 添加 WCF 客户端代码：

    ```csharp
    using System.ServiceModel;

        class Program
        {
            static void Main(string[] args)
            {
                var myBinding = new BasicHttpBinding();
                var myEndpoint = new EndpointAddress("http://localhost:2561/Service1.svc"); //Fill your service url here
                var myChannelFactory = new ChannelFactory<IService1>(myBinding, myEndpoint);
                IService1 client = myChannelFactory.CreateChannel();
                string s = client.GetData(1);
                ((ICommunicationObject)client).Close();
            }
        }

    [ServiceContract]
    public interface IService1
    {
        [XmlSerializerFormat]
        [OperationContract(Action = "http://tempuri.org/IService1/GetData", ReplyAction = "http://tempuri.org/IService1/GetDataResponse")]
        string GetData(int value);
    }
    ```

5. 通过运行以下命令将引用添加到 `dotnet-svcutil.xmlserializer` 包：
  
    ```dotnetcli
    dotnet add package dotnet-svcutil.xmlserializer
    ```

    运行该命令应向项目文件中添加一个类似于以下内容的条目：
  
    ```xml
    <ItemGroup>
      <DotNetCliToolReference Include="dotnet-svcutil.xmlserializer" Version="1.0.0" />
    </ItemGroup>
    ```

6. 通过运行 `dotnet build` 生成应用程序。 如果一切顺利，则会在输出文件夹中生成名为“MyWCFClient.XmlSerializers.dll”的程序集。 如果该工具无法生成程序集，将在生成输出中看到警告。

7. 例如，通过在浏览器中运行 `http://localhost:2561/Service1.svc` 来启动 WCF 服务。 然后启动客户端应用程序，它将在运行时自动加载和使用预生成的序列化程序。
