---
title: 在类和结构之间选择
description: 了解如何确定是将类型设计为类，还是将类型设计为结构。 了解引用类型和值类型在 .NET 中的不同之处。
ms.date: 10/22/2008
helpviewer_keywords:
- class library design guidelines [.NET Framework], structures
- class library design guidelines [.NET Framework], classes
- structures [.NET Framework], vs. classes
- classes [.NET Framework], design guidelines
- type design guidelines, structures
- structures [.NET Framework], design guidelines
- classes [.NET Framework], vs. structures
- type design guidelines, classes
ms.assetid: f8b8ec9b-0ba7-4dea-aadf-a93395cd804f
ms.openlocfilehash: 05ba9abbc9495d927b7f58ebb06f152c0c15772f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95701245"
---
# <a name="choosing-between-class-and-struct"></a>在类和结构之间选择

每个框架设计器所面临的一项基本设计决策是，是将类型设计为作为类 (引用类型) 还是作为结构 () 值类型。 充分了解引用类型和值类型的行为差异对于做出此选择至关重要。

 引用类型和值类型的第一个区别在于，引用类型在堆上分配并进行垃圾回收，而值类型则在堆栈上分配，或在包含类型中以内联方式分配，在堆栈展开或其包含类型释放时释放。 因此，值类型的分配和释放通常比引用类型的分配和释放更便宜。

 接下来，将对引用类型的数组进行内嵌分配，也就是说，数组元素只是引用驻留在堆上的引用类型的实例。 值类型数组是以内联方式分配的，这意味着数组元素是值类型的实际实例。 因此，值类型阵列的分配和释放比引用类型阵列的分配和释放更便宜。 此外，在大多数情况下，值类型数组表现出更好的引用位置。

 下一个差异与内存使用情况相关。 值类型在转换为引用类型或其实现的接口之一时获得装箱。 它们会在转换回值类型时进行取消装箱。 因为框是在堆上分配并进行垃圾回收的对象，所以太多装箱和取消装箱会对堆、垃圾回收器和最终性能产生负面影响。  与此相反，引用类型不会被强制转换。  (有关详细信息，请参阅 [装箱和取消装箱](../../csharp/programming-guide/types/boxing-and-unboxing.md)) 。

 接下来，引用类型分配复制引用，而值类型分配则复制整个值。 因此，大型引用类型的分配比大值类型的分配更便宜。

 最后，引用类型通过引用传递，而值类型通过值传递。 对引用类型的实例所做的更改将影响指向该实例的所有引用。 值类型实例在按值传递时复制。 当值类型的实例发生更改时，它不会影响它的任何副本。 由于这些副本不是由用户显式创建的，而是在传递参数或返回值时隐式创建的，因此，可以更改的值类型可能会给许多用户造成混淆。 因此，值类型应是不可变的。

 作为经验法则，框架中的大部分类型都应为类。 但在某些情况下，值类型的特征使其更适合使用结构。

 ✔️考虑定义结构而不是类的实例，如果该类型的实例较小且通常为短生存期，或者通常嵌入到其他对象中。

 ❌ 避免定义结构，除非该类型具有以下所有特性：

- 它以逻辑方式表示单个值，与 )  (、等的基元类型类似。 `int` `double`

- 它的实例大小为16字节。

- 它是不可变的。

- 它不需要频繁装箱。

 在所有其他情况下，应将类型定义为类。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [类型设计准则](type.md)
- [框架设计准则](index.md)
