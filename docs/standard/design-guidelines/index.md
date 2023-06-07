---
title: 框架设计准则
description: 请参阅框架设计准则，用于设计扩展和与 .NET 交互的库，以确保 API 一致性和易用性。
titleSuffix: ''
ms.date: 10/22/2008
helpviewer_keywords:
- libraries, .NET Framework class library
- class library design guidelines [.NET Framework], about
- class library design guidelines [.NET Framework]
ms.assetid: 5fbcaf4f-ea2a-4d20-b0d6-e61dee202b4b
ms.openlocfilehash: 03fa44c1fed219b50cf1a8d22b2c9f79947f4976
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706653"
---
# <a name="framework-design-guidelines"></a>框架设计准则

本部分提供了有关设计扩展和与 .NET Framework 进行交互的库的指南。 其目标是通过提供与用于开发的编程语言无关的统一编程模型，来帮助库设计人员确保 API 一致性和易用性。 建议在开发扩展 .NET Framework 的类和组件时遵循这些设计指导原则。 不一致的库设计会对开发人员工作效率产生负面影响，并防止采用。  
  
 本指南被组织为简单的建议，其前缀为 `Do` 、、 `Consider` `Avoid` 和 `Do not` 。 这些准则旨在帮助类库设计人员了解不同解决方案之间的权衡。 在某些情况下，良好的库设计需要你违反这些设计准则。 这种情况应该很少见，因此您有一个清晰且引人注目的决策理由。  
  
 这些指导原则摘录内容于本书 *框架设计指南：约定、惯例和模式，适用于可重复使用的 .Net 库第2版*，通过 Krzysztof Cwalina 和 Brad Abrams。  
  
## <a name="in-this-section"></a>本节内容  

 [命名准则](naming-guidelines.md)  
 提供用于命名类库中的程序集、命名空间、类型和成员的准则。  
  
 [类型设计准则](type.md)  
 提供使用静态和抽象类、接口、枚举、结构和其他类型的指导原则。  
  
 [成员设计准则](member.md)  
 提供设计和使用属性、方法、构造函数、字段、事件、运算符和参数的准则。  
  
 [扩展性设计](designing-for-extensibility.md)  
 讨论扩展性机制（如子类化、使用事件、虚拟成员和回调），并说明如何选择最能满足框架要求的机制。  
  
 [异常设计准则](exceptions.md)  
 介绍设计、引发和捕获异常的设计准则。  
  
 [使用准则](usage-guidelines.md)  
 介绍使用常见类型（如数组、特性和集合、支持序列化和重载相等运算符）的准则。  
  
 [常见设计模式](common-design-patterns.md)  
 提供有关选择和实现依赖项属性的准则。  
  
 *部分©2005，2009 Microsoft Corporation。保留所有权利。*  
  
 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*  
  
## <a name="see-also"></a>另请参阅

- [概述](../../framework/get-started/overview.md)
- [开发指南](../../framework/development-guide.md)
