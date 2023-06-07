---
title: <comContracts>
ms.date: 03/30/2017
ms.assetid: 42e74148-223d-4888-a8ed-1d928527eb09
ms.openlocfilehash: 404cc66ce423ba947c2817b56bebb4daf341ef0b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176040"
---
# \<comContracts>

`comContracts` 配置节所包含的元素允许指定 COM+ 集成服务协定的各个属性。  
  
## <a name="specifying-namespace-and-contract"></a>指定命名空间和协定  

 COM + 集成服务协定当前仅限于 `http://tempuri.org` 命名空间，协定名称派生自支持的 COM 接口。 但是，可以使用配置文件中的 `comContracts` 节来指定替代服务协定。  
  
 例如，可以使用以下配置来指定服务协定的命名空间和协定名称，也可以指定某个选项以在会话绑定上强制使用。  
  
```xml  
<comContracts>
  <comContract contract="{5163B1E7-F0CF-4B6A-9A02-4AB654F34284}"
               namespace="http://tempuri.org/5163B1E7-F0CF-4B6A-9A02-4AB654F34284"
               name="_Broker"
               requireSession="true">
  </comContract>
</comContracts>
```  
  
 在初始化服务时，指定的命名空间和协定名称将应用到生成的服务说明。  
  
 当此节为空时，服务初始化将应用取自提供支持的 COM 接口 ID 的默认命名空间和协定名称。  
  
 此外，还可以使用 [\<exposedMethod>](exposedmethod.md) 元素指定在 COM + 组件上的接口作为 Web 服务公开时公开的 COM + 方法。 你还可以使用 [\<persistableTypes>](persistabletypes.md) 来指定集成中使用的持久类型。 最后，你可以使用 [\<userDefinedType>](userdefinedtype.md) 元素来包含要包含在服务协定中 (UDT) 的用户定义类型。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ComContractElementCollection>
- <xref:System.ServiceModel.Configuration.ComContractElement>
- [\<exposedMethod>](exposedmethod.md)
- [\<persistableTypes>](persistabletypes.md)
- [\<userDefinedType>](userdefinedtype.md)
- [\<comContract>](comcontract.md)
- [与 COM + 应用程序集成](../../../wcf/feature-details/integrating-with-com-plus-applications.md)
- [如何：配置 COM+ 服务设置](../../../wcf/feature-details/how-to-configure-com-service-settings.md)
