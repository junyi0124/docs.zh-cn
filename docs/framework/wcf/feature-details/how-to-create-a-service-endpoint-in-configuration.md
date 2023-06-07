---
title: 如何：在配置中创建服务终结点
description: 了解如何使用同时包含相对和绝对地址的配置文件为 WCF 服务添加终结点。
ms.date: 06/16/2016
ms.assetid: f474e25d-2a27-4f31-84c5-395c442b8e70
ms.openlocfilehash: aa8dce18a39b44c2c56d072a81699d1bc1e7d7da
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286417"
---
# <a name="how-to-create-a-service-endpoint-in-configuration"></a>如何：在配置中创建服务终结点

终结点使客户端能够访问 Windows Communication Foundation (WCF) 服务所提供的功能。 您可以通过使用相对和绝对终结点地址的组合来定义一个或多个终结点，或者如果您未定义任何服务终结点，则默认情况下运行时为您提供一些终结点。 本主题演示如何使用同时包含相对和绝对地址的配置文件来添加终结点。  
  
## <a name="example"></a>示例  

 下列服务配置指定一个基址和五个终结点。  
  
```xml  
<configuration>  
  
  <appSettings>  
    <!-- use appSetting to configure base address provided by host -->  
    <add key="baseAddress" value="http://localhost:8000/servicemodelsamples/service" />  
  </appSettings>  
  
  <system.serviceModel>  
    <services>  
    <!-- This section is optional with the default configuration introduced in .NET Framework 4. -->  
      <service name="Microsoft.ServiceModel.Samples.CalculatorService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
          </baseAddresses>  
        </host>  
        <endpoint address=""  
                  binding="wsHttpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <endpoint address="/test"  
                  binding="wsHttpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <endpoint address="http://localhost:8001/hello/servicemodelsamples"  
                  binding="wsHttpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <endpoint address="net.tcp://localhost:9000/servicemodelsamples/service"  
                  binding="netTcpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <!-- the mex endpoint is another relative address exposed at   
             http://localhost:8000/ServiceModelSamples/service/mex -->  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
  </system.serviceModel>  
  
</configuration>  
```  
  
## <a name="example"></a>示例  

 基址是使用 `add` 元素在 service/host/baseAddresses 下指定的，如下面的示例所示。  
  
```xml  
<service
    name="Microsoft.ServiceModel.Samples.CalculatorService">  
  <host>  
    <baseAddresses>  
      <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
    </baseAddresses>  
  </host>  
```  
  
## <a name="example"></a>示例  

 在以下示例中显示的第一个终结点指定一个相对地址，这意味着该终结点地址是遵循统一资源标识符 (URI) 构成规则的基址和相对地址的结合。 相对地址为空 ("")，因此终结点地址与基址相同。 实际终结点地址为 `http://localhost:8000/servicemodelsamples/service` 。  
  
```xml  
<endpoint address=""
    binding="wsHttpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>示例  

 第二个终结点定义也指定一个相对地址，如下面的示例配置所示。 将相对地址“test”追加到基址。 实际终结点地址为 `http://localhost:8000/servicemodelsamples/service/test` 。  
  
```xml  
<endpoint address="/test"  
    binding="wsHttpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>示例  

 第三个终结点定义指定一个绝对地址，如下面的示例配置所示。 基址在地址中不起作用。 实际终结点地址为 `http://localhost:8001/hello/servicemodelsamples` 。  
  
```xml  
<endpoint address="http://localhost:8001/hello/servicemodelsamples"  
    binding="wsHttpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>示例  

 第四个终结点地址指定一个绝对地址和一个不同的传输协议 (TCP)。 基址在地址中不起作用。 实际终结点地址为 net.tcp://localhost:9000/servicemodelsamples/service。  
  
```xml  
<endpoint address="net.tcp://localhost:9000/servicemodelsamples/service"  
    binding="netTcpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>示例  

 若要使用运行时提供的默认终结点，请不要在代码或配置文件中指定任何服务终结点。 在此示例中，运行时在打开服务时创建默认终结点。 有关默认终结点、绑定和行为的详细信息，请参阅[简化配置](../simplified-configuration.md)和 [WCF 服务的简化配置](../samples/simplified-configuration-for-wcf-services.md)。  
  
```xml  
<configuration>  
  
  <appSettings>  
    <!-- use appSetting to configure base address provided by host -->  
    <add key="baseAddress" value="http://localhost:8000/servicemodelsamples/service" />  
  </appSettings>  
  
  <system.serviceModel>  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
  </system.serviceModel>  
  
</configuration>  
```
