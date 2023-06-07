---
description: -filealign（C# 编译器选项）
title: -filealign（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /filealign
helpviewer_keywords:
- /alignment compiler option [C#]
- filealign compiler option [C#]
- -filealign compiler option [C#]
- /sections compiler option [C#]
- alignment compiler option [C#]
- sections compiler option [C#]
- -sections compiler option [C#]
- /filealign compiler option [C#]
- file sharing [C#]
- -alignment compiler option [C#]
- section alignment [C#]
ms.assetid: 15cf1c98-3798-4ced-9f08-60619308a073
ms.openlocfilehash: 4b61217a3d6812ea3ab036f82d49bba05c20629e
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91173238"
---
# <a name="-filealign-c-compiler-options"></a>-filealign（C# 编译器选项）

-filealign 选项用于指定输出文件中各节的大小。  
  
## <a name="syntax"></a>语法  
  
```console  
-filealign:number  
```  
  
## <a name="arguments"></a>自变量  

 `number`  
 一个值，用于指定输出文件中各节的大小。 有效值为 512、1024、2048、4096 和 8192。 这些值以字节为单位。  
  
## <a name="remarks"></a>备注  

 每一节都在是 -filealign 值的倍数的边界上对齐。 没有固定的默认值。 如果未指定 -filealign，则公共语言运行时在编译时会选取一个默认值。  
  
 通过指定节的大小，可以影响输出文件的大小。 修改节的大小可能对将在较小设备上运行的程序有用。  
  
 使用 [DUMPBIN](/cpp/build/reference/dumpbin-options) 可查看有关输出文件中各节的信息。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项  
  
1. 打开项目的“属性”页。  
  
2. 单击“生成”属性页。  
  
3. 单击“高级”按钮。  
  
4. 修改“文件对齐”属性。  
  
 有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.FileAlignment%2A>。  
  
## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
