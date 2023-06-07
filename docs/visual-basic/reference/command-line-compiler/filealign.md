---
title: -filealign
ms.date: 03/10/2018
helpviewer_keywords:
- sections compiler option [Visual Basic]
- alignment compiler option [Visual Basic]
- -filealign compiler option [Visual Basic]
- section alignment
- /filealign compiler option [Visual Basic]
- filealign compiler option [Visual Basic]
ms.assetid: cc61ec3d-ad38-4b28-9659-099d73cad099
ms.openlocfilehash: 809b7ad005b6bb5f127f84425b5d2beb980df471
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058126"
---
# <a name="-filealign"></a>-filealign

指定输出文件各节的对齐位置。  
  
## <a name="syntax"></a>语法  
  
```console  
-filealign:number  
```  
  
## <a name="arguments"></a>自变量  

 `number`  
 必需。 一个值，用于指定输出文件中各节的对齐方式。 有效值为 512、1024、2048、4096 和 8192。 这些值以字节为单位。  
  
## <a name="remarks"></a>备注  

 可以使用 `-filealign` 选项来指定输出文件中各节的对齐方式。 节是可移植可执行 (PE) 文件中包含代码或数据的连续内存块。 `-filealign` 选项允许你使用非标准对齐方式编译应用程序；大多数开发人员不需要使用此选项。  
  
 每一节都与边界一致，该边界是 `-filealign` 值的倍数。 没有固定的默认值。 如果未指定 `-filealign`，编译器将在编译时选取默认值。  
  
 通过指定节的大小，可以更改输出文件的大小。 修改节的大小可能对将在较小设备上运行的程序有用。  
  
> [!NOTE]
> `-filealign` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
