---
ms.openlocfilehash: 3506653bfc749ae3d8002715ca72ca89de7a681b
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "91025210"
---
### <a name="wcf-addressheadercollection-now-throws-an-argumentexception-if-an-addressheader-element-is-null"></a>如果 addressHeader 元素为 null，则 WCF AddressHeaderCollection 现在引发 ArgumentException

#### <a name="details"></a>详细信息

自 .NET Framework 4.7.1 起，如果有一个元素为 `null`，<xref:System.ServiceModel.Channels.AddressHeaderCollection.%23ctor(System.Collections.Generic.IEnumerable{System.ServiceModel.Channels.AddressHeader})> 构造函数就会引发 <xref:System.ArgumentException>。 在 .NET Framework 4.7 和更低版本中，不引发异常。

#### <a name="suggestion"></a>建议

如果在包含此更改的 .NET Framework 4.7.1 或更高版本中遇到兼容性问题，可以通过在 app.config 文件的 `<runtime>` 部分添加下面的代码行选择退出它：

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.ServiceModel.DisableAddressHeaderCollectionValidation=true" />
  </runtime>
</configuration>
```

| 名称    | 值   |
|:--------|:--------|
| 范围   | 次要   |
| Version | 4.7.1   |
| 类型    | 运行时 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.ServiceModel.Channels.AddressHeaderCollection.%23ctor(System.Collections.Generic.IEnumerable{System.ServiceModel.Channels.AddressHeader})>

<!--

#### Affected APIs

- `M:System.ServiceModel.Channels.AddressHeaderCollection.#ctor(System.Collections.Generic.IEnumerable{System.ServiceModel.Channels.AddressHeader})`

-->
