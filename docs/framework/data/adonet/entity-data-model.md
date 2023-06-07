---
title: 实体数据模型
description: 实体数据模型描述数据的结构，而不考虑其存储形式，它解决了在多种形式下存储数据导致的挑战。
ms.date: 03/30/2017
ms.assetid: 2dda3d5b-4582-4ba0-a91d-fcd7a1498137
ms.openlocfilehash: 9323f652d1cfa27d442d45bd01e546f3b81d7e80
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200766"
---
# <a name="entity-data-model"></a>实体数据模型

实体数据模型 (EDM) 是一组描述数据结构（而不管其存储形式如何）的概念。 EDM 借自于 Peter Chen 于 1976 年所描述的实体关系模型，但它是在实体关系模型基础上构建的并扩展了其传统用途。  
  
 EDM 解决了以多种形式存储的数据所带来的难题。 例如，假设某项业务将数据存储在关系数据库、文本文件、XML 文件、电子表格和报表中。 这在数据建模、应用程序设计和数据访问方面会带来很大的难题。 当设计面向对象的应用程序时，其困难在于如何编写高效的、可维护的代码而不必牺牲数据访问、存储和可扩展性方面的高效性。 当数据具有关系结构时，其在数据访问、存储和可扩展性方面会非常高效，但编写高效的、可维护的代码则变得非常困难。 当数据具有对象结构时，上述关系恰好相反：编写高效的、可维护的代码要以牺牲数据访问、存储和可扩展性方面的高效性为代价。 即使可以在这些优劣方面找到最佳平衡，但是当将数据从一种形式转变为另一种形式时又会引发新的难题。 实体数据模型通过以独立于任何存储架构的实体和关系的方式描述数据结构解决了这些难题。 这使得数据的存储形式与应用程序设计和开发变得不相关。 此外，由于实体和关系描述的是数据在应用程序中使用时的结构（而不是其存储形式），因此它们会随应用程序的演变而演变。  
  
 `conceptual model`是实体和关系的数据结构的特定表示形式，通常用实现 EDM 概念的域特定语言 (DSL) 定义。 [概念架构定义语言 (CSDL) ](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec) 是这样一种域特定语言的示例。 可以将概念模型中描述的实体和关系视为对应用程序中的对象和关联的一种抽象表述。 这使得开发人员可以专注于概念模型而不必考虑存储架构，从而在编写代码时更多关注高效性和可维护性。 与此同时，存储架构设计者可以专注于数据访问、存储和可扩展性方面的高效性。  
  
## <a name="in-this-section"></a>本节内容  

 本节中的主题描述实体数据模型的概念。 实现 EDM 的任何 DSL 都应包含这里描述的概念。 请注意， [ADO.NET 实体框架](./ef/index.md) 使用 CSDL 来定义概念模型。 有关详细信息，请参阅 [CSDL Specification](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)。  
  
 [实体数据模型关键概念](entity-data-model-key-concepts.md)  
  
 [实体数据模型：命名空间](entity-data-model-namespaces.md)  
  
 [实体数据模型：基元数据类型](entity-data-model-primitive-data-types.md)  
  
 [实体数据模型：继承](entity-data-model-inheritance.md)  
  
 [关联端](association-end.md)  
  
 [关联端重数](association-end-multiplicity.md)  
  
 [Association Set — 关联集](association-set.md)  
  
 [关联集端](association-set-end.md)  
  
 [关联类型](association-type.md)  
  
 [复杂类型](complex-type.md)  
  
 [Entity Container — 实体容器](entity-container.md)  
  
 [实体键](entity-key.md)  
  
 [实体集](entity-set.md)  
  
 [实体类型](entity-type.md)  
  
 [多](facet.md)  
  
 [外键属性](foreign-key-property.md)  
  
 [模型声明函数](model-declared-function.md)  
  
 [模型定义函数](model-defined-function.md)  
  
 [Navigation Property — 导航属性](navigation-property.md)  
  
 [property](property.md)  
  
 [引用完整性约束](referential-integrity-constraint.md)  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
- [.edmx 文件概述](/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))
- [CSDL 规范](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
