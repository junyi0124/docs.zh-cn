---
title: .NET 库中断性变更
description: 列出 .NET Core 1.0-3.0 版核心 .NET 库中的中断性变更。
ms.date: 07/27/2020
ms.openlocfilehash: 092ff36a5e07c9e226fe2a67d5e7cfd391e9d16b
ms.sourcegitcommit: fcbe432482464b1639decad78cc4dc8387c6269e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2020
ms.locfileid: "97366883"
---
# <a name="core-net-libraries-breaking-changes-in-net-core-10-30"></a>.NET Core 1.0-3.0 中的核心 .NET 库中断性变更

核心 .NET 库提供了 .NET Core 使用的基元和其他常规类型。

本页记录了以下中断性变更：

| 重大更改 | 引入的版本 |
| - | :-: |
| [将 GroupCollection 传递到采用 IEnumerable\<T> 的扩展方法需要消除歧义](#passing-groupcollection-to-extension-methods-taking-ienumerablet-requires-disambiguation) | 3.0 |
| [报告版本的 API 现在报告产品版本而不是文件版本](#apis-that-report-version-now-report-product-and-not-file-version) | 3.0 |
| [自定义 EncoderFallbackBuffer 实例无法递归回退](#custom-encoderfallbackbuffer-instances-cannot-fall-back-recursively) | 3.0 |
| [浮点格式设置和分析行为变更](#floating-point-formatting-and-parsing-behavior-changed) | 3.0 |
| [浮点分析操作不再失败或引发 OverflowException](#floating-point-parsing-operations-no-longer-fail-or-throw-an-overflowexception) | 3.0 |
| [InvalidAsynchronousStateException 已移到另一个程序集](#invalidasynchronousstateexception-moved-to-another-assembly) | 3.0 |
| [替换格式错误的 UTF-8 字节序列将遵循 Unicode 准则](#replacing-ill-formed-utf-8-byte-sequences-follows-unicode-guidelines) | 3.0 |
| [TypeDescriptionProviderAttribute 已移到另一个程序集](#typedescriptionproviderattribute-moved-to-another-assembly) | 3.0 |
| [ZipArchiveEntry 不再处理条目大小不一致的存档](#ziparchiveentry-no-longer-handles-archives-with-inconsistent-entry-sizes) | 3.0 |
| [FieldInfo.SetValue 将对静态、仅初始化字段引发异常](#fieldinfosetvalue-throws-exception-for-static-init-only-fields) | 3.0 |
| [添加到内置结构类型的私有字段](#private-fields-added-to-built-in-struct-types) | 2.1 |
| [UseShellExecute 默认值更改](#change-in-default-value-of-useshellexecute) | 2.1 |
| [macOS 上的 OpenSSL 版本](#openssl-versions-on-macos) | 2.1 |
| [FileSystemInfo.Attributes 引发的 UnauthorizedAccessException](#unauthorizedaccessexception-thrown-by-filesysteminfoattributes) | 1.0 |
| [不支持处理损坏的进程状态异常](#handling-corrupted-state-exceptions-is-not-supported) | 1.0 |
| [UriBuilder 属性不再预置前导字符](#uribuilder-properties-no-longer-prepend-leading-characters) | 1.0 |
| [Process.StartInfo 对未启动的进程引发 InvalidOperationException](#processstartinfo-throws-invalidoperationexception-for-processes-you-didnt-start) | 1.0 |

## <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE [disambiguate-generic-type-for-groupcollection](../../../includes/core-changes/corefx/3.0/disambiguate-generic-type-for-groupcollection.md)]

***

[!INCLUDE[APIs that report version now report product and not file version](~/includes/core-changes/corefx/3.0/version-information-changes.md)]

**_

[!INCLUDE[Custom EncoderFallbackBuffer instances cannot fall back recursively](~/includes/core-changes/corefx/3.0/custom-encoderfallbackbuffer-cannot-be-recursive.md)]

_*_

[!INCLUDE[Floating point formatting and parsing behavior changes](~/includes/core-changes/corefx/3.0/floating-point-changes.md)]

_*_

[!INCLUDE[Floating-point parsing operations no longer fail or throw an OverflowException](~/includes/core-changes/corefx/3.0/floating-point-parsing-does-not-overflow.md)]

_*_

[!INCLUDE[InvalidAsynchronousStateException moved to another assembly](~/includes/core-changes/corefx/3.0/move-invalidasynchronousstateexception.md)]

_*_

[!INCLUDE[NET Core 3.0 follows Unicode best practices when replacing ill-formed UTF-8 byte sequences](~/includes/core-changes/corefx/3.0/net-core-3-0-follows-unicode-utf8-best-practices.md)]

_*_

[!INCLUDE[TypeDescriptionProviderAttribute moved to another assembly](~/includes/core-changes/corefx/3.0/move-typedescriptionproviderattribute.md)]

_*_

[!INCLUDE[ZipArchiveEntry no longer handles archives with inconsistent entry sizes](~/includes/core-changes/corefx/3.0/ziparchiveentry-and-inconsistent-entry-sizes.md)]

_*_

[!INCLUDE [FieldInfo.SetValue throws exception for static, init-only fields](~/includes/core-changes/corefx/3.0/fieldinfo-setvalue-exception.md)]

_*_

## <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE[Private fields added to built-in struct types](~/includes/core-changes/corefx/2.1/instantiate-struct.md)]

_*_

[!INCLUDE[Change in default value of UseShellExecute](~/includes/core-changes/corefx/2.1/process-start-changes.md)]

_*_

[!INCLUDE [OpenSSL versions on macOS](../../../includes/core-changes/corefx/openssl-dependencies-macos.md)]

_*_

## <a name="net-core-10"></a>.NET Core 1.0

[!INCLUDE [UnauthorizedAccessException thrown by FileSystemInfo.Attributes](~/includes/core-changes/corefx/1.0/filesysteminfo-attributes-exceptions.md)]

_*_

[!INCLUDE [corrupted-state-exceptions](~/includes/core-changes/corefx/1.0/corrupted-state-exceptions.md)]

_*_

[!INCLUDE [uribuilder-behavior-changes](../../../includes/core-changes/corefx/1.0/uribuilder-behavior-changes.md)]

_*_

[!INCLUDE [startinfo-throws-exception](../../../includes/core-changes/corefx/1.0/startinfo-throws-exception.md)]

_**
