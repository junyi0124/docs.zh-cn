---
title: 示例 COM 类 - C# 编程指南
description: 了解如何在 C# 中将类公开为 COM 对象。 此示例将 .cs 文件中的代码添加到项目中，并设置“注册 COM 互操作”属性。
ms.date: 07/20/2015
helpviewer_keywords:
- examples [C#], COM classes
- COM, exposing Visual C# objects to
ms.assetid: 6504dea9-ad1c-4993-a794-830fec5270af
ms.openlocfilehash: 9274fef15e4fcfd4a268e4f245581966ad6ab750
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170358"
---
# <a name="example-com-class-c-programming-guide"></a>COM 类示例（C# 编程指南）

下面是将公开为 COM 对象的类的示例。 在将此代码放置在 .cs 文件中并添加到项目后，将“注册 COM 互操作”属性设置为“True”。 有关详细信息，请参阅[如何：注册 COM 互操作组件](/previous-versions/visualstudio/visual-studio-2010/w29wacsy(v=vs.100))。
  
 对 COM 公开 Visual C# 对象需要声明类接口、事件接口（如有必要）和类本身。 类成员必须遵循这些规则才能显示在 COM 中：  
  
- 类必须是公开的。  
  
- 属性、方法和事件必须是公开的。  
  
- 必须在类接口上声明属性和方法。  
  
- 必须在事件接口中声明事件。  
  
 该类中未在这些接口中声明的其他公共成员将对 COM 不可见，但它们对其他 .NET 对象可见。  
  
 若要对 COM 公开属性和方法，则必须在类接口上声明这些属性和方法，将它们标记为 `DispId` 属性，并在类中实现它们。 在接口中声明成员的顺序是用于 COM vtable 的顺序。  
  
 若要从类中公开事件，则必须在事件接口上声明这些事件并将其标记为 `DispId` 属性。 此类不应实现此接口。  
  
 此类实现此类接口；它可以实现多个接口，但第一个实现将为默认类接口。 在此处实现向 COM 公开的方法和属性。 它们必须标记为公共，并且必须匹配类接口中的声明。 此外，在此处声明此类引发的事件。 它们必须标记为公共，并且必须匹配事件接口中的声明。  
  
## <a name="example"></a>示例  

 [!code-csharp[csProgGuideInterop#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInterop/CS/ExampleCOM.cs#8)]  
  
## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [互操作性](./index.md)
- [“项目设计器”->“生成”页 (C#)](/visualstudio/ide/reference/build-page-project-designer-csharp)
