---
title: -warnaserror
ms.date: 03/13/2018
helpviewer_keywords:
- warnaserror compiler option [Visual Basic]
- /warnaserror compiler option [Visual Basic]
- -warnaserror compiler option [Visual Basic]
ms.assetid: 49819f1d-a1bd-4201-affe-5afe6d9712e1
ms.openlocfilehash: 2c243b05b7e819691165ef20996691c0bd38ae4a
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91098860"
---
# <a name="-warnaserror-visual-basic"></a>-warnaserror (Visual Basic)

使编译器将第一次出现的警告视为错误。  
  
## <a name="syntax"></a>语法  
  
```console  
-warnaserror[+ | -][:numberList]  
```  
  
## <a name="arguments"></a>自变量  
  
|术语|定义|  
|---|---|  
|+ &#124; -|可选。 默认情况下，`-warnaserror-` 生效；警告不会阻止编译器生成输出文件。 `-warnaserror` 选项与 `-warnaserror+` 相同，会导致将警告视为错误。|  
|`numberList`|可选。 `-warnaserror` 选项适用的警告 ID 编号列表，以逗号分隔。 如果未指定警告 ID，则 `-warnaserror` 选项适用于所有警告。|  
  
## <a name="remarks"></a>备注  

 `-warnaserror` 选项将所有警告视为错误。 通常将报告为警告的任何消息都报告为错误。 编译器将随后出现的相同警告报告为警告。  
  
 默认情况下，`-warnaserror-` 生效，导致警告仅提供信息。 `-warnaserror` 选项与 `-warnaserror+` 相同，会导致将警告视为错误。  
  
 如果希望仅将一些特定警告视为错误，则可以指定视为错误的警告编号的逗号分隔列表。  
  
> [!NOTE]
> `-warnaserror` 选项不控制警告的显示方式。 使用 [-nowarn](nowarn.md) 选项来禁用警告。  
  
|设置 -warnaserror 以将所有警告视为 Visual Studio IDE 中的错误|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。 <br />2.单击“编译”  选项卡。<br />3.确保“禁用所有警告”  复选框处于未选中状态。<br />4.选中“将所有警告视为错误”  复选框。|  
  
|设置 -warnaserror 以将特定警告视为 Visual Studio IDE 中的错误|  
|---|  
|1.在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。<br />2.单击“编译”  选项卡。<br />3.确保“禁用所有警告”  复选框处于未选中状态。<br />4.确保“将所有警告视为错误”  复选框处于未选中状态。<br />5.从应将其视为错误的警告旁的“通知”  列中选择“错误”  。|  
  
## <a name="example"></a>示例  

 以下代码编译 `In.vb` 并指示编译器在第一次发现每个警告时显示错误。  
  
```console
vbc -warnaserror in.vb  
```  
  
## <a name="example"></a>示例  

 以下代码编译 `T2.vb` 并仅将未使用的本地变量 (42024) 的警告视为错误。  
  
```console
vbc -warnaserror:42024 t2.vb  
```  
  
## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)
