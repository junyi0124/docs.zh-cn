---
title: 对 2.0 版中的 System.Uri 命名空间的更改
ms.date: 03/30/2017
ms.assetid: 35883fe9-2d09-4d8b-80ca-cf23a941e459
ms.openlocfilehash: 987010b8367069e8089df3f809d23f258bb68f2b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "61642758"
---
# <a name="changes-to-the-systemuri-namespace-in-version-20"></a>对 2.0 版中的 System.Uri 命名空间的更改

对 <xref:System.Uri?displayProperty=nameWithType> 类进行了少量更改。 这些更改修复了不正确的行为、提高了可用性、增强了安全性。

## <a name="obsolete-and-deprecated-members"></a>已过时和弃用的成员

 构造函数：

- 所有具有 `dontEscape` 参数的构造函数。

 方法：

- <xref:System.Uri.CheckSecurity%2A>

- <xref:System.Uri.Escape%2A>

- <xref:System.Uri.Canonicalize%2A>

- <xref:System.Uri.Parse%2A>

- <xref:System.Uri.IsReservedCharacter%2A>

- <xref:System.Uri.IsBadFileSystemCharacter%2A>

- <xref:System.Uri.IsExcludedCharacter%2A>

- <xref:System.Uri.EscapeString%2A>

## <a name="changes"></a>Changes

- 对于已知不含有查询部件（文件、ftp 等）的 URI 方案，“?”字符始终转义，不将它视为 <xref:System.Uri.Query%2A> 部件的开头。

- 对于隐式文件 URI（采用 `c:\directory\file@name.txt` 形式），片段字符（“#”）始终转义，除非要求完全反转义或 <xref:System.Uri.LocalPath%2A> 为 `true`。

- 删除了 UNC 主机名支持；采用了 IDN 规范来表示国际主机名。

- <xref:System.Uri.LocalPath%2A> 始终返回完全反转义的字符串。

- <xref:System.Uri.ToString%2A> 不反转义已转义的 %、? 或 # 字符。

- <xref:System.Uri.Equals%2A> 在相等性检查现在包含 <xref:System.Uri.Query%2A> 部件。

- 运算符 == 和 != 已重写并链接到 <xref:System.Uri.Equals%2A> 方法。

- <xref:System.Uri.IsLoopback%2A> 现在产生一致的结果。

- URI“`file:///path`”不再转换为 `file://path`。

- 现在，“#”被识别为主机名终止符。 即，`http://contoso.com#fragment` 现在转换为 `http://contoso.com/#fragment`。

- 结合基 URI 与片段时的 bug 已修复。

- <xref:System.Uri.HostNameType%2A> 中的 bug 已修复。

- NNTP 分析中的 bug 已修复。

- HTTP:contoso.com 形式的 URI 现引发分析异常。

- Framework 正确处理 URI 中的用户信息。

- URI 路径压缩已修复，所以断开的 URI 不能遍历根以上的文件系统。

## <a name="see-also"></a>另请参阅

- <xref:System.Uri?displayProperty=nameWithType>
