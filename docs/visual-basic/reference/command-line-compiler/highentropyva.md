---
title: -highentropyva
ms.date: 03/10/2018
helpviewer_keywords:
- highentropyva compiler option (Visual Basic)
- /highentropyva compiler option (Visual Basic)
ms.assetid: ff25f20a-6ca2-467b-9e52-5cf439f5028e
ms.openlocfilehash: 4d88c302281097fabd0eb361d60319bc8a0daf8d
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058061"
---
# <a name="-highentropyva-visual-basic"></a>-highentropyva (Visual Basic)

指示 64 位可执行文件或由 [-platform:anycpu](platform.md) 编译器选项标记的可执行文件是否支持高熵地址空间布局随机化 (ASLR)。  
  
## <a name="syntax"></a>语法  
  
```console  
-highentropyva[+ | -]  
```  
  
## <a name="arguments"></a>自变量  

 `+` &#124; `-`  
 可选。 默认情况下，或指定 `-highentropyva-` 时，此选项处于关闭状态。 如果指定 `-highentropyva` 或 `-highentropyva+`，则选项处于打开状态。  
  
## <a name="remarks"></a>备注  

 如果指定此选项，那么当内核将进程的地址空间布局作为 ASLR 的一部分随机化时，Windows 内核的兼容版本可以使用更高程度的熵。 如果内核使用更高程度的熵，则可向内存区域（例如堆栈或堆）分配更多的地址。 因此，猜测特定内存区域的位置会更加困难。  
  
 如果此选项处于打开状态，则目标可执行文件及其依赖的任何模块在作为 64 位进程运行时，必须能够处理大于 4 GB 的指针值。  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
