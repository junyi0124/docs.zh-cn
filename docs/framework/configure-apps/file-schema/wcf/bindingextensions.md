---
title: <bindingExtensions>
ms.date: 03/30/2017
ms.assetid: 8373f94d-d095-486f-8f1e-4ac2f72b58c7
ms.openlocfilehash: bd6aeb32e0994bceb9e56bcb1c6267f4cb64a5a4
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73039141"
---
# \<bindingExtensions>

本节为使用计算机或应用程序配置文件中的用户定义绑定提供支持。 通过使用 `add` 关键字，并将元素的 `type` 属性设置为用户定义的绑定，将 `name` 属性设置为用户定义的绑定的名称，可以将用户定义的绑定添加到此集合中。

用户可以使用绑定扩展来创建用户定义的绑定，并将其作为终结点配置的一部分来使用。 从编程角度来看，绑定扩展是一个实现抽象类 <xref:System.ServiceModel.Channels.Binding> 的类型。

下面的示例使用 `add` 元素以及 `name` 属性将绑定扩展添加到 `bindingExtensions` 配置文件的节中：

```xml
<system.serviceModel>
  <extensions>
    <bindingExtensions>
      <add name="MyBinding"
           type="Microsoft.ServiceModel.Samples.MyBinding, MyBinding,
                 Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    </bindingExtensions>
  </extensions>
</system.serviceModel>
```

若要向元素添加配置功能，用户需要编写和注册 `bindingSection` 元素。 有关这方面的更多信息，请参见 <xref:System.Configuration> 文档。

定义元素及其配置类型之后，可以将扩展用作终结点的一部分，如以下示例中所示：

```xml
<services>
  <service name="MyService">
    <endpoint address="myAddress"
              binding="MyBinding" />
  </service>
</services>
```

## <a name="see-also"></a>另请参阅

- [扩展绑定](../../../wcf/extending/extending-bindings.md)
