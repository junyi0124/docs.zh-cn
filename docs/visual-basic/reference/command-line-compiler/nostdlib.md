---
title: -nostdlib
ms.date: 03/13/2018
helpviewer_keywords:
- nostdlib compiler option [Visual Basic]
- -nostdlib compiler option [Visual Basic]
- /nostdlib compiler option [Visual Basic]
ms.assetid: 140381b8-dc96-4ad5-ae11-792c9ed0be4d
ms.openlocfilehash: 4fcc5305985f5ba32b3e6ffb740c0611821215d3
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097652"
---
# <a name="-nostdlib-visual-basic"></a>-nostdlib (Visual Basic)

导致编译器不自动引用标准库。  
  
## <a name="syntax"></a>语法  
  
```console  
-nostdlib  
```  
  
## <a name="remarks"></a>备注  

 `-nostdlib` 选项会删除对 System.dll 程序集的自动引用，并防止编译器读取 Vbc.rsp 文件。 Vbc.rsp 文件（与 Vbc.exe 文件位于同一个目录）引用常用的 .NET Framework 程序集，并导入 `System` 和 `Microsoft.VisualBasic` 命名空间。  
  
> [!NOTE]
> 始终引用 Mscorlib.dll 和 Microsoft.VisualBasic.dll 程序集。  
  
> [!NOTE]
> `-nostdlib` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。  
  
## <a name="example"></a>示例  

 下面的代码在不引用标准库的情况下编译 `T2.vb`。 必须将 `_MYTYPE` 条件编译常量设置为字符串“Empty”，才能删除 `My` 对象。  
  
```console
vbc -nostdlib -define:_MYTYPE=\"Empty\" T2.vb  
```  
  
## <a name="see-also"></a>请参阅

- [-noconfig](noconfig.md)
- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [自定义 My 中可用的对象](../../developing-apps/customizing-extending-my/customizing-which-objects-are-available-in-my.md)
