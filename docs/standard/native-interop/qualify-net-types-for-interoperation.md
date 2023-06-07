---
title: 为 COM 互操作限定 .NET 类型
description: 本文提供了一些指导原则，可帮助你为 COM 互操作向 COM 应用程序公开 .NET 程序集中的类型。
ms.date: 03/30/2017
helpviewer_keywords:
- exposing .NET components to COM
- COM interop, qualifying .NET types
- qualifying .NET types for interoperation
- interoperation with unmanaged code, qualifying .NET types
- interoperation with unmanaged code, exposing .NET components
- COM interop, exposing COM components
ms.assetid: 4b8afb52-fb8d-4e65-b47c-fd82956a3cdd
ms.openlocfilehash: 3f5fe0f168e0e520ce1985faf5a8228c1bfbdf20
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734304"
---
# <a name="qualifying-net-types-for-com-interoperation"></a>为 COM 互操作限定 .NET 类型

若要向 COM 应用程序公开程序集中的类型，请考虑 COM 互操作在设计时的需求。 如果符合以下准则，托管类型（类、接口、结构和枚举）将与 COM 类型无缝集成：  
  
- 类应显式实现接口。  
  
     尽管 COM 互操作提供了一种机制，用于自动生成包含类的所有成员及其基类成员的接口，但最好还是提供显式接口。 自动生成的接口称为类接口。 有关指南，请参阅[类接口简介](com-callable-wrapper.md#introducing-the-class-interface)。  
  
     可以使用 Visual Basic、C# 和 C++ 将接口定义合并在代码中，而无需使用接口定义语言 (IDL) 或其等效语言。 有关语法的详细信息，请参见语言文档。  
  
- 托管的类型必须是公共的。  
  
     只有程序集中的公共类型才会注册并导出到类型库中。 因此只有公共类型才对 COM 可见。  
  
     托管类型会向其他未向 COM 公开的托管代码公开功能。 例如，参数化的构造函数、静态方法和常数字段不会向 COM 客户端公开。 此外，运行时在类型中和类型外封送数据时，数据可能会被复制或转换。  
  
- 方法、属性、字段和事件必须是公共的。  
  
     如果要对 COM 可见，公共类型的成员也必须是公共的。 通过应用 <xref:System.Runtime.InteropServices.ComVisibleAttribute>，可以限制程序集、公共类型或公共类型的公共成员的可见性。 默认情况下，所有公共类型和成员都是可见的。  
  
- 具备公共无参数构造函数的类型才能从 COM 中激活。  
  
     托管的公共类型对于 COM 是可见的。 但是如果没有公共无参数构造函数（不带任何参数的构造函数），COM 客户端无法创建该类型。 如果该类型由其他方法激活，COM 客户端仍可使用该类型。  
  
- 类型不能是抽象的。  
  
     COM 客户端和 .NET 客户端都不能创建抽象的类型。  
  
 导出到 COM 后，托管类型的继承层次结构将被展平。 托管和非托管环境之间的版本控制也会有所不同。 向 COM 公开的类型不具有其他托管类型相同的版本控制特性。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.ComVisibleAttribute>
- [向 COM 公开 .NET Framework 组件](../../framework/interop/exposing-dotnet-components-to-com.md)
- [类接口简介](com-callable-wrapper.md#introducing-the-class-interface)
- [应用互操作属性](apply-interop-attributes.md)
- [打包用于 COM 的 .NET Framework 程序集](../../framework/interop/packaging-an-assembly-for-com.md)
