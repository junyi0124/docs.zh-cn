---
description: -nowarn（C# 编译器选项）
title: -nowarn（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /nowarn
helpviewer_keywords:
- nowarn compiler option [C#]
- /nowarn compiler option [C#]
- -nowarn compiler option [C#]
ms.assetid: 6dcbc5e8-ae67-4566-9df3-f63cfdd9c4e4
ms.openlocfilehash: 31a7ee5eacb2e7cd6b24c4a2276ce6e07fcc67e1
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91194019"
---
# <a name="-nowarn-c-compiler-options"></a>-nowarn（C# 编译器选项）

使用 -nowarn 选项可以禁止编译器显示一个或多个警告。 使用逗号分隔多个警告编号。  
  
## <a name="syntax"></a>语法  
  
```console  
-nowarn:number1[,number2,...]  
```  
  
## <a name="arguments"></a>自变量  

 `number1`, `number2`  
 希望编译器禁止显示的警告编号。  
  
## <a name="remarks"></a>备注  

 只应指定警告标识符的数值部分。 例如，如果要禁止显示 CS0028，则可以指定 `-nowarn:28`。  
  
 编译器会以无提示方式忽略传递给 `-nowarn` 的警告编号，这些编号在早期版本中有效，但已从编译器中移除。 例如，CS0679 在 Visual Studio .NET 2002 的编译器中有效，但是在后来已移除。  
  
 无法通过 `-nowarn` 选项禁止显示以下警告：  
  
- 编译器警告（等级 1）CS2002  
  
- 编译器警告（等级 1）CS2023  
  
- 编译器警告（等级 1）CS2029  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”  页。 有关详细信息，请参阅[“项目设计器”->“生成”页 (C#)](/visualstudio/ide/reference/build-page-project-designer-csharp)。  
  
2. 单击“生成”属性页。  
  
3. 修改“禁止显示警告”属性。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.ProjectProperties3.DelaySign%2A>。  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
- [C# 编译器错误](../compiler-messages/index.md)
