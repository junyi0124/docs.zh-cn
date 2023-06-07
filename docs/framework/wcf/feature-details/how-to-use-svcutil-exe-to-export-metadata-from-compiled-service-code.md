---
title: 如何：使用 Svcutil.exe 将元数据从已编译的服务代码中导出
ms.date: 03/30/2017
ms.assetid: 95d0aed3-16a2-4398-89bb-39418eeb7355
ms.openlocfilehash: f60d0c9ad3f6fc4e9596d466b5eabdaab0f4822f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280606"
---
# <a name="how-to-use-svcutilexe-to-export-metadata-from-compiled-service-code"></a>如何：使用 Svcutil.exe 将元数据从已编译的服务代码中导出

Svcutil.exe 可以导出已编译程序集中的服务、协定和数据类型的元数据，如下所示：  
  
- 若要使用 Svcutil.exe 为一组程序集导出所有已编译服务协定的元数据，请将这些程序集指定为输入参数。 这是默认行为。  
  
- 若要使用 Svcutil.exe 导出已编译服务的元数据，请将该（这些）服务程序集指定为输入参数。 必须使用 `/serviceName` 选项来指示要导出的服务的配置名称。 Svcutil.exe 自动为指定的可执行程序集加载配置文件。  
  
- 若要导出一组程序集内的所有数据协定类型，请使用 `/dataContractOnly` 选项。  
  
> [!NOTE]
> 请使用 `/reference` 选项来指定所有相关程序集的文件路径。  
  
### <a name="to-export-metadata-for-compiled-service-contracts"></a>导出已编译服务协定的元数据  
  
1. 将服务协定实现编译为一个或多个类库。  
  
2. 在已编译程序集上运行 Svcutil.exe。  
  
    > [!NOTE]
    > 您可能需要使用 `/reference` 开关指定任意相关程序集的文件路径。  
  
    ```console
    svcutil.exe Contracts.dll  
    ```  
  
### <a name="to-export-metadata-for-a-compiled-service"></a>导出已编译服务的元数据  
  
1. 将服务实现编译为可执行程序集。  
  
2. 为服务可执行程序集创建一个配置文件，并添加服务配置。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name="MyService" >  
            <endpoint address="finder" contract="IPeopleFinder" binding="wsHttpBinding" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
3. 在使用 `/serviceName` 开关指定服务的配置名称的已编译服务可执行程序集上运行 Svcutil.exe。  
  
    > [!NOTE]
    > 您可能需要使用 `/reference` 开关指定任意相关程序集的文件路径。  
  
    ```console  
    svcutil.exe /serviceName:MyService Service.exe /reference:path/Contracts.dll  
    ```  
  
### <a name="to-export-metadata-for-compiled-data-contracts"></a>导出已编译数据协定的元数据  
  
1. 将数据协定实现编译为一个或多个类库。  
  
2. 在使用 `/dataContract` 开关指定应仅生成数据协定元数据的已编译程序集上运行 Svcutil.exe。  
  
    > [!NOTE]
    > 您可能需要使用 `/reference` 开关指定任意相关程序集的文件路径。  
  
    ```console  
    svcutil.exe /dataContractOnly Contracts.dll  
    ```  
  
## <a name="example"></a>示例  

 下面的示例演示如何为单个服务实现和配置生成元数据。  
  
 导出服务协定的元数据。  
  
```console  
svcutil.exe Contracts.dll  
```  
  
 导出数据协定的元数据。  
  
```console  
svcutil.exe /dataContractOnly Contracts.dll  
```  
  
 导出服务实现的元数据。  
  
```console  
svcutil.exe /serviceName:MyService Service.exe /reference:<path>/Contracts.dll  
```  
  
 `<path>` 是 Contracts.dll 的路径。  
  
```csharp
// The following service contract and data contracts are compiled into
// Contracts.dll.  
[ServiceContract(ConfigurationName="IPeopleFinder")]  
public interface IPersonFinder  
{  
    [OperationContract]  
    Address GetAddress(Person s);  
}  
  
[DataContract]  
public class Person  
{  
    [DataMember]  
    public string firstName;  
    [DataMember]  
    public string lastName;  
    [DataMember]  
    public int age;  
}  
  
[DataContract]  
public class Address  
{  
    [DataMember]  
    public string city;  
    [DataMember]  
    public string state;  
    [DataMember]  
    public string street;  
    [DataMember]  
    public int zipCode;  
    [DataMember]  
    public Person person;  
}  
```

```csharp
// The following service implementation is compiled into Service.exe.
// This service uses the contracts specified in Contracts.dll.  
[ServiceBehavior(ConfigurationName = "MyService")]  
public class MyService : IPersonFinder  
{  
    public Address GetAddress(Person person)  
    {  
        Address address = new Address();  
        address.person = person;  
        return address;  
    }  
}  
```

```xml  
<!-- The following is the configuration file for Service.exe. -->  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="MyService">  
         <endpoint  address="finder"  
                    binding="basicHttpBinding"  
                    contract="IPeopleFinder"/>  
      </service>  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- [ServiceModel 元数据实用工具 (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
- [导出和导入元数据](exporting-and-importing-metadata.md)
