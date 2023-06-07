---
description: -optimize（C# 编译器选项）
title: -optimize（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /optimize
helpviewer_keywords:
- /optimize compiler option [C#]
- -o compiler option [C#]
- optimize compiler option [C#]
- /o compiler option [C#]
- -optimize compiler option [C#]
- compiler optimization [C#]
- o compiler option [C#]
ms.assetid: 6dd5b6f2-cd1d-4593-a9f4-1c2ed9404ca0
ms.openlocfilehash: 1862794e4d823e38ce19780300a0b04f4e57dc44
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91193980"
---
# <a name="-optimize-c-compiler-options"></a>-optimize（C# 编译器选项）

-optimize 选项启用或禁用编译器执行的优化，使输出文件更小、更快、更有效。  
  
## <a name="syntax"></a>语法  
  
```console  
-optimize[+ | -]  
```  
  
## <a name="remarks"></a>备注  

 -optimize 还指示公共语言运行时在运行时优化代码。  
  
 默认情况下，禁用优化。 指定 -optimize+ 可启用优化。  
  
 生成程序集使用的模块时，请使用与程序集相同的 -optimize 设置。  
  
 -o 是 -optimize 的缩写形式。  
  
 可以将 -optimize 和 [-debug](./debug-compiler-option.md) 选项组合使用。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。  
  
2. 单击“生成”属性页。  
  
3. 修改“优化代码”属性。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.Optimize%2A>。  
  
## <a name="example"></a>示例  

 编译 `t2.cs` 并启用编译器优化：  
  
```console  
csc t2.cs -optimize  
```  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
