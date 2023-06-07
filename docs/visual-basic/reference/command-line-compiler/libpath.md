---
title: -libpath
ms.date: 03/10/2018
helpviewer_keywords:
- libpath compiler option [Visual Basic]
- /libpath compiler option [Visual Basic]
- -libpath compiler option [Visual Basic]
ms.assetid: 5f1c26c9-3455-4e89-bdf3-b12d6c2e655b
ms.openlocfilehash: a91bd74d0be4f1cb223091ee2527f9567b4ca5db
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058464"
---
# <a name="-libpath"></a>-libpath

指定引用的程序集的位置。  
  
## <a name="syntax"></a>语法  
  
```console  
-libpath:dirList  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`dirList`|必需。 在当前工作目录（调用编译器的目录）或公共语言运行时的系统目录中未找到引用的程序集时，编译器将在其中进行查找的由分号分隔的目录列表。 如果目录名包含空格，则将名称括在引号 (" ") 内。|  
  
## <a name="remarks"></a>备注  

 `-libpath` 选项指定通过 [-reference](reference.md) 选项引用的程序集的位置。  
  
 编译器按以下顺序搜索未完全限定的程序集引用：  
  
1. 当前工作目录。 该目录为从其调用编译器的目录。  
  
2. 公共语言运行时系统目录。  
  
3. 由 `-libpath` 指定的目录。  
  
4. 由 LIB 环境变量指定的目录。  
  
 `-libpath` 选项是累加的；每一次指定的值都追加到以前的值中。  
  
 使用 `-reference` 指定程序集引用。  
  
|在 Visual Studio 集成开发环境中设置 -libpath|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“引用”  选项卡。<br />3.单击“引用路径...”按钮。 <br />4.在“引用路径”对话框的“文件夹:”框中，输入目录名称。  <br />5.单击“添加文件夹”。 |  
  
## <a name="example"></a>示例  

 下面的代码编译 `T2.vb`，用于生成 .exe 文件。 编译器将在工作目录、C: 驱动器的根目录和 C: 驱动器的“新程序集”目录中查找程序集引用。  
  
```console  
vbc -libpath:c:\;"c:\New Assemblies" -reference:t2.dll t2.vb  
```  
  
## <a name="see-also"></a>请参阅

- [.NET 中的程序集](../../../standard/assembly/index.md)
- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
