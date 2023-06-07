---
ms.openlocfilehash: f011f6ebe0fa85a59f3e69c93bba9eddc4c1689b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497807"
---
### <a name="reflection-objects-can-no-longer-be-passed-from-managed-code-to-out-of-process-dcom-clients"></a>反射对象可能无法再从托管代码传递至进程外 DCOM 客户端

#### <a name="details"></a>详细信息

反射对象可能无法再从托管代码传道至进程外客户端 以下类型将受影响：

- <xref:System.Reflection.Assembly?displayProperty=fullName>
- <xref:System.Reflection.MemberInfo?displayProperty=fullName>（及其派生类型，包括 <xref:System.Reflection.FieldInfo?displayProperty=fullName>、<xref:System.Reflection.MethodInfo?displayProperty=fullName>、<xref:System.Type?displayProperty=fullName> 和 <xref:System.Reflection.TypeInfo?displayProperty=fullName>）
- <xref:System.Reflection.MethodBody?displayProperty=fullName>
- <xref:System.Reflection.Module?displayProperty=fullName>
- <xref:System.Reflection.ParameterInfo?displayProperty=fullName>

调用对象的 <code>IMarshal</code> 会返回 <code>E_NOINTERFACE</code>。

#### <a name="suggestion"></a>建议

更新封送代码，以与非反射对象一起使用。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.6|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Reflection.Assembly?displayProperty=fullName>
- <xref:System.Reflection.FieldInfo?displayProperty=fullName>
- <xref:System.Reflection.MemberInfo?displayProperty=fullName>
- <xref:System.Reflection.MethodBody?displayProperty=fullName>
- <xref:System.Reflection.MethodInfo?displayProperty=fullName>
- <xref:System.Reflection.Module?displayProperty=fullName>
- <xref:System.Reflection.ParameterInfo?displayProperty=fullName>
- <xref:System.Reflection.TypeInfo?displayProperty=fullName>
- <xref:System.Type?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Reflection.Assembly`
- `T:System.Reflection.FieldInfo`
- `T:System.Reflection.MemberInfo`
- `T:System.Reflection.MethodBody`
- `T:System.Reflection.MethodInfo`
- `T:System.Reflection.Module`
- `T:System.Reflection.ParameterInfo`
- `T:System.Reflection.TypeInfo`
- `T:System.Type`

-->
