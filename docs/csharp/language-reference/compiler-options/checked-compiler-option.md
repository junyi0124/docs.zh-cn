---
description: -checked（C# 编译器选项）
title: -checked（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /checked
helpviewer_keywords:
- checked compiler option [C#]
- -checked compiler option [C#]
- /checked compiler option [C#]
ms.assetid: fb7475d3-e6a6-4e6d-b86c-69e7a74c854b
ms.openlocfilehash: c92ad61b2f482631230e0e6aeb0af5716a4fcb61
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91196827"
---
# <a name="-checked-c-compiler-options"></a>-checked（C# 编译器选项）

-checked 选项指定，不在 [checked](../keywords/checked.md) 或 [unchecked](../keywords/unchecked.md) 关键字的范围内、并且产生的值超出数据类型范围的整数算法语句是否将导致运行时异常。  
  
## <a name="syntax"></a>语法  
  
```console  
-checked[+ | -]  
```  
  
## <a name="remarks"></a>备注  

 `checked` 或 `unchecked` 关键字范围内的整数算法语句不受 -checked 选项的影响。  
  
 如果不在 `checked` 或 `unchecked` 关键字范围内的整数算法语句产生的值超出数据类型范围，并且编译中使用了 -checked+（或 -checked），则该语句将在运行时导致异常。 如果编译中使用的是 -checked-，则该语句在运行时不会导致异常。  
  
 此选项的默认值为“-checked-”；溢出检查已禁用。

 有时，用于生成大型应用程序的自动化工具将 -checked 设置为 +。 使用 -checked- 的一种方案：通过指定 -checked- 来替代工具的全局默认值。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。 有关详细信息，请参阅 [“项目设计器”->“生成”页 (C#)](/visualstudio/ide/reference/build-page-project-designer-csharp)。  
  
2. 单击“生成”属性页。  
  
3. 单击“高级”按钮。  
  
4. 修改“检查算术溢出”属性。  
  
 要以编程方式访问此编译器选项，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.CheckForOverflowUnderflow%2A>。  
  
## <a name="example"></a>示例  

 以下命令编译 `t2.cs`。 命令中 `-checked` 的使用指定，任何不在 `checked` 或 `unchecked` 关键字范围内以及导致数据类型以外值的结果的文件中的整数算术语句，会在运行时引发异常。  
  
```console  
csc t2.cs -checked  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
