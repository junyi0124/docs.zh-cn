---
title: -moduleassemblyname
ms.date: 03/13/2018
helpviewer_keywords:
- moduleassemblyname compiler option [Visual Basic]
- /moduleassemblyname compiler option [Visual Basic]
- -moduleassemblyname compiler option [Visual Basic]
ms.assetid: 013a57b6-f425-4dd3-b333-512d72c42f55
ms.openlocfilehash: 9fb9287f9472d4b33eff4cb601aff5eed370b2c0
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91065562"
---
# <a name="-moduleassemblyname"></a>-moduleassemblyname

指定此模块所属程序集的名称。  
  
## <a name="syntax"></a>语法  
  
```console  
-moduleassemblyname:assembly_name  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`assembly_name`|此模块所属程序集的名称。|  
  
## <a name="remarks"></a>备注  

 仅当指定了 `-target:module` 选项时，编译器才处理 `-moduleassemblyname` 选项。 这将导致编译器创建模块。 编译器创建的模块仅对使用 `-moduleassemblyname` 选项指定的程序集有效。 如果将模块放在不同的程序集中，则会发生运行时错误。  
  
 仅当满足以下条件时，才需要 `-moduleassemblyname` 选项：  
  
- 模块中的数据类型需要访问所引用程序集中的 `Friend` 类型。  
  
- 引用的程序集已获得友元程序集访问权限，可访问将在其中生成模块的程序集。  
  
 有关创建模块的详细信息，请参阅 [-target (Visual Basic)](target.md)。 有关友元程序集的详细信息，请参阅[友元程序集](../../../standard/assembly/friend.md)。  
  
> [!NOTE]
> `-moduleassemblyname` 选项在 Visual Studio 开发环境内无法使用；仅当从命令提示符编译时才可用。  
  
## <a name="see-also"></a>请参阅

- [如何：生成单文件程序集](../../../framework/app-domains/build-multifile-assembly.md)
- [Visual Basic 命令行编译器](index.md)
- [-target (Visual Basic)](target.md)
- [-main](main.md)
- [-reference (Visual Basic)](reference.md)
- [-addmodule](addmodule.md)
- [.NET 中的程序集](../../../standard/assembly/index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [友元程序集](../../../standard/assembly/friend.md)
