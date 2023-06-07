---
title: -nowarn
ms.date: 07/20/2015
helpviewer_keywords:
- nowarn compiler option [Visual Basic]
- /nowarn compiler option [Visual Basic]
- -nowarn compiler option [Visual Basic]
ms.assetid: 7ebf2106-0652-4fdc-bf60-70fc86465d83
ms.openlocfilehash: cde96fff975a65d6303ee62e6a811bfd83d5ff97
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91097678"
---
# <a name="-nowarn"></a>-nowarn

禁止编译器生成警告的能力。  
  
## <a name="syntax"></a>语法  
  
```console  
-nowarn[:numberList]  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|`numberList`|可选。 编译器应禁止的以逗号分隔的警告 ID 编号列表。 如果未指定警告 ID，将禁止所有警告。|  
  
## <a name="remarks"></a>备注  

 `-nowarn` 选项会导致编译器不生成警告。 若要禁止单个警告，请向后跟冒号的 `-nowarn` 选项提供警告 ID。 使用逗号分隔多个警告编号。  
  
 仅需指定警告标识符的数值部分。 例如，如果要禁止 BC42024（针对未使用本地变量的警告），请指定 `-nowarn:42024`。  
  
 有关警告 ID 编号的详细信息，请参阅[在 Visual Basic 中配置警告](/visualstudio/ide/configuring-warnings-in-visual-basic)。  
  
|在 Visual Studio 集成开发环境中设置 -nowarn|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“编译”  选项卡。<br />3.选择“禁用所有警告”  复选框以禁用所有警告。<br />     - 或 -<br />     若要禁用特定警告，请单击警告旁边的下拉列表中的“无”  。|  
  
## <a name="example"></a>示例  

 以下代码编译 `T2.vb` 并且不显示任何警告。  
  
```console
vbc -nowarn t2.vb  
```  
  
## <a name="example"></a>示例  

 以下代码编译 `T2.vb` 并且不显示未使用的本地变量 (42024) 的警告。  
  
```console
vbc -nowarn:42024 t2.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)
