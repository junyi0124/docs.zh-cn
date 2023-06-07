---
ms.openlocfilehash: 2d0798917b639bc90bf3f78f6fb9f19d0eaf2c71
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614382"
---
### <a name="incorrect-implementation-of-memberdescriptorequals"></a>MemberDescriptor.Equals 实现不正确

#### <a name="details"></a>详细信息

<xref:System.ComponentModel.MemberDescriptor.Equals%2A?displayProperty=nameWithType> 方法的原始实现从比较类别名称和描述字符串这两个对象来比较两个不同字符串属性。 解决方法是比较第一个对象的 <xref:System.ComponentModel.MemberDescriptor.Category> 和第二个对象的 <xref:System.ComponentModel.MemberDescriptor.Category>，并比较第一个对象的 <xref:System.ComponentModel.MemberDescriptor.Description> 和第二个对象的 <xref:System.ComponentModel.MemberDescriptor.Description>。

#### <a name="suggestion"></a>建议

描述符相等时如果应用程序依赖于有时返回 `false` 的 <xref:System.ComponentModel.MemberDescriptor.Equals%2A?displayProperty=nameWithType> 并且你面向 .NET Framework 4.6.2 和更高版本，那么可以采用以下几个选项：

- 除了调用 <xref:System.ComponentModel.MemberDescriptor.Equals%2A?displayProperty=nameWithType> 方法，还需更改代码来手动比较 <xref:System.ComponentModel.MemberDescriptor.Category> 和 <xref:System.ComponentModel.MemberDescriptor.Description> 字段。
- 将以下值添加到 app.config 文件从而选择弃用此更改：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.MemberDescriptorEqualsReturnsFalseIfEquivalent=true" />
</runtime>
```

如果应用程序面向 .NET Framework 4.6.1 或更低版本，并且在 .NET Framework 4.6.2 及更高版本上运行且你想要启用此更改，可以通过将以下值添加到 app.config 文件以将兼容性开关设为 `false`：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.MemberDescriptorEqualsReturnsFalseIfEquivalent=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.ComponentModel.MemberDescriptor.Equals(System.Object)?displayProperty=nameWithType>
