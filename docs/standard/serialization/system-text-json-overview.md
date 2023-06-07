---
title: 使用 C# 对 JSON 进行序列化和反序列化 - .NET
description: 本概述介绍了在 .NET 中对 JSON 进行序列化和反序列化的 System.Text.Json 命名空间功能。
ms.date: 01/10/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: cb5c15c2a5c336e2d5b4a3754fa7a02a370602f3
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009880"
---
# <a name="json-serialization-and-deserialization-marshalling-and-unmarshalling-in-net---overview"></a>.NET 中的 JSON 序列化和反序列化（封送和拆收）- 概述

`System.Text.Json` 命名空间提供用于序列化和反序列化 JavaScript 对象表示法 (JSON) 的功能。

库的设计强调对广泛的功能集实现高性能和低内存分配。 内置的 UTF-8 支持可优化读写以 UTF-8 编码的 JSON 文本的过程，UTF-8 编码是针对 Web 上的数据和磁盘上的文件的最普遍的编码方式。

库还提供了用于处理内存中文档对象模型 (DOM) 的类。 此功能允许对 JSON 文件或字符串中的元素进行随机只读访问。

## <a name="how-to-get-the-library"></a>如何获取库

* 该库是作为 .NET Core 3.0 及更高版本共享框架的一部分内置的。
* 对于早期版本的框架，请安装 [System.Text.Json](https://www.nuget.org/packages/System.Text.Json) NuGet 包。 包支持以下框架：

  * .NET Standard 2.0 及更高版本
  * .NET Framework 4.7.2 及更高版本
  * .NET Core 2.0、2.1 和 2.2

## <a name="additional-resources"></a>其他资源

* [如何使用库](system-text-json-how-to.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [自定义属性名称和值](system-text-json-customize-properties.md)
* [忽略属性](system-text-json-ignore-properties.md)
* [允许无效的 JSON](system-text-json-invalid-json.md)
* [处理溢出 JSON](system-text-json-handle-overflow.md)
* [保留引用](system-text-json-preserve-references.md)
* [不可变类型和非公共访问器](system-text-json-immutability.md)
* [多态序列化](system-text-json-polymorphism.md)
* [从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [自定义字符编码](system-text-json-character-encoding.md)
* [编写自定义序列化程序和反序列化程序](write-custom-serializer-deserializer.md)
* [编写用于 JSON 序列化的自定义转换器](system-text-json-converters-how-to.md)
* [DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
* [System.Text.Json.Serialization API 参考](xref:System.Text.Json.Serialization)
