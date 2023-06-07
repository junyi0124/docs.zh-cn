---
title: -optionstrict
ms.date: 07/20/2015
f1_keywords:
- -optionstrict
helpviewer_keywords:
- -optionstrict compiler option [Visual Basic]
- optionstrict compiler option [Visual Basic]
- /optionstrict compiler option [Visual Basic]
ms.assetid: c7b10086-0fa4-49db-b3c8-4ae0db5957da
ms.openlocfilehash: 3dd12971a082869c32b6292ed45e2014b8b0e2c0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400534"
---
# <a name="-optionstrict"></a>-optionstrict

强制执行严格类型语义，限制隐式类型转换。

## <a name="syntax"></a>语法

```console
-optionstrict[+ | -]
-optionstrict[:custom]
```

## <a name="arguments"></a>自变量

`+` &#124; `-`  
可选。 `-optionstrict+` 选项限制隐式类型转换。 此选项的默认值为 `-optionstrict-`。 `-optionstrict+` 选项与 `-optionstrict` 相同。 两者均可用于宽松类型语义。

`custom`  
必需。 不遵从严格语言语义时发出警告。

## <a name="remarks"></a>备注

`-optionstrict+` 生效时，仅可隐式生成扩大类型转换。 隐式收缩类型转换将被报告为错误，例如将 `Decimal` 类型对象分配给整数类型对象。

若要针对隐式收缩类型转换生成警告，请使用 `-optionstrict:custom`。 使用 `-nowarn:numberlist` 可忽略特定警告，使用 `-warnaserror:numberlist` 可将特定警告视为错误。

### <a name="to-set--optionstrict-in-the-visual-studio-ide"></a>在 Visual Studio IDE 中设置 -optionstrict

1. 在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。

2. 单击“编译”  选项卡。

3. 修改“Option Strict”框中的值。 

### <a name="to-set--optionstrict-programmatically"></a>以编程方式设置 -optionstrict

请参阅 [Option Strict 语句](../../language-reference/statements/option-strict-statement.md)。

## <a name="example"></a>示例

下面的代码使用严格类型语义编译 `Test.vb`。

```console
vbc -optionstrict+ test.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-optioncompare](optioncompare.md)
- [-optionexplicit](optionexplicit.md)
- [-optioninfer](optioninfer.md)
- [-nowarn](nowarn.md)
- [-warnaserror (Visual Basic)](warnaserror.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [Option Strict 语句](../../language-reference/statements/option-strict-statement.md)
- [“选项”对话框 ->“项目”->“Visual Basic 默认值”](/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)
