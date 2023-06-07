---
title: 大小写约定
description: 为标识符、组合词和常见字词应用大写约定。 了解如何在 .NET 中使用区分大小写。
ms.date: 10/22/2008
helpviewer_keywords:
- camel-case names [.NET Framework]
- class library design guidelines [.NET Framework], capitalization
- Pascal-case names [.NET Framework]
- case sensitivity, capitalization conventions
- names [.NET Framework], capitalization
ms.assetid: 4c4ea526-9203-486f-b72d-29d61c5b3c6d
ms.openlocfilehash: e416a8346952a41d9c89f526bfce990dfc277fc1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701258"
---
# <a name="capitalization-conventions"></a>大小写约定

本章中的指导原则介绍了使用大小写的简单方法，即在应用一致的情况下，使类型、成员和参数的标识符易于阅读。

## <a name="capitalization-rules-for-identifiers"></a>标识符的大小写规则

 若要区分标识符中的单词，请将标识符中每个单词的首字母大写。 不要使用下划线来区分词，或在标识符中的任何位置使用下划线。 有两种方法可以根据标识符的使用来大写标识符：

- PascalCasing

- camelCasing

 PascalCasing 约定用于除了参数名称外的所有标识符，使每个单词的第一个字符大写， (包括长度超过两个字母的首字母缩写) ，如以下示例中所示：

 `PropertyDescriptor`
 `HtmlTag`

 这两个字母的首字母缩写词是一种特殊情况，其中两个字母都大写，如以下标识符所示：

 `IOStream`

 CamelCasing 约定仅用于参数名称，将每个单词的第一个字符（除第一个单词之外）设为大写，如以下示例中所示。 如示例中所示，以字母混合形式表示的两字母首字母缩写形式均为小写。

 `propertyDescriptor`
 `ioStream`
 `htmlTag`

 ✔️对于包含多个单词的所有公共成员、类型和命名空间名称，请使用 PascalCasing。

 ✔️使用 camelCasing 作为参数名称。

 下表描述了不同类型的标识符的大小写规则。

|标识符|大小写|示例|
|----------------|------------|-------------|
|命名空间|形式|`namespace System.Security { ... }`|
|类型|形式|`public class StreamReader { ... }`|
|接口|形式|`public interface IEnumerable { ... }`|
|方法|形式|`public class Object {` <br />  `public virtual string ToString();` <br /> `}`|
|properties|形式|`public class String {` <br />  `public int Length { get; }` <br /> `}`|
|事件|形式|`public class Process {` <br />  `public event EventHandler Exited;` <br /> `}`|
|字段|形式|`public class MessageQueue {` <br />  `public static readonly TimeSpan` <br /> `InfiniteTimeout;` <br /> `}` <br /> `public struct UInt32 {` <br />  `public const Min = 0;` <br /> `}`|
|枚举值|形式|`public enum FileMode {` <br />  `Append,` <br />  `...` <br /> `}`|
|参数|混合|`public class Convert {` <br />  `public static int ToInt32(string value);` <br /> `}`|

## <a name="capitalizing-compound-words-and-common-terms"></a>大写复合词和常见字词

 出于大写字母的目的，大多数复合词都被视为单个字词。

 ❌ 不要将所谓的 "封闭格式" 组合词中的每个词大写。

 它们是以单个词（如终结点）形式编写的组合词。 为了实现大小写准则，请将关闭窗体的组合词视为单个单词。 使用当前字典来确定是否以闭合形式写入复合单词。

|形式|混合|非|
|------------|-----------|---------|
|`BitFlag`|`bitFlag`|`Bitflag`|
|`Callback`|`callback`|`CallBack`|
|`Canceled`|`canceled`|`Cancelled`|
|`DoNot`|`doNot`|`Don't`|
|`Email`|`email`|`EMail`|
|`Endpoint`|`endpoint`|`EndPoint`|
|`FileName`|`fileName`|`Filename`|
|`Gridline`|`gridline`|`GridLine`|
|`Hashtable`|`hashtable`|`HashTable`|
|`Id`|`id`|`ID`|
|`Indexes`|`indexes`|`Indices`|
|`LogOff`|`logOff`|`LogOut`|
|`LogOn`|`logOn`|`LogIn`|
|`Metadata`|`metadata`|`MetaData, metaData`|
|`Multipanel`|`multipanel`|`MultiPanel`|
|`Multiview`|`multiview`|`MultiView`|
|`Namespace`|`namespace`|`NameSpace`|
|`Ok`|`ok`|`OK`|
|`Pi`|`pi`|`PI`|
|`Placeholder`|`placeholder`|`PlaceHolder`|
|`SignIn`|`signIn`|`SignOn`|
|`SignOut`|`signOut`|`SignOff`|
|`UserName`|`userName`|`Username`|
|`WhiteSpace`|`whiteSpace`|`Whitespace`|
|`Writable`|`writable`|`Writeable`|

## <a name="case-sensitivity"></a>区分大小写

 可在 CLR 上运行的语言不需要支持区分大小写，但有些则不需要。 即使您的语言支持，其他可能访问您的框架的语言也不是这样。 因此，外部可访问的任何 Api 都不能单独依赖于 case 来区分同一上下文中的两个名称。

 ❌ 不要假设所有编程语言都区分大小写。 它们不是。 名称不能单独区分。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [命名准则](naming-guidelines.md)
