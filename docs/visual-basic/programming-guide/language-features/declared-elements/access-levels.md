---
title: 访问级别
ms.date: 05/10/2018
helpviewer_keywords:
- members [Visual Basic], accessing in Visual Basic
- Friend access modifier
- access levels, declared elements
- access levels
- access modifiers
- Public access modifier
- Protected access modifier
- Protected Friend access modifier
- Private Protected access modifier
- Private access modifier
- declared elements [Visual Basic], access level
ms.assetid: 6e06c1ab-fd78-47f0-83a8-1152780b5e1a
ms.openlocfilehash: 33a218a2acc3c876428d6c9a887280a559f84323
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348685"
---
# <a name="access-levels-in-visual-basic"></a>Visual Basic 中的访问级别

已声明元素的*访问级别*是访问它的能力范围，即，哪些代码有权读取或写入该元素。 这不仅取决于你如何声明元素本身，还取决于元素的容器的访问级别。 不能访问包含元素的代码不能访问其包含的任何元素，甚至是声明为 `Public`的元素。 例如，可以从包含结构的类中访问 `Private` 结构中的 `Public` 变量，而不是从该类的外部访问。

## <a name="public"></a>公共

声明语句中的[Public](../../../language-reference/modifiers/public.md)关键字指定可以从同一项目中的任何位置、引用项目的其他项目以及从项目生成的任何程序集访问元素。 下面的代码演示 `Public` 声明的示例：

```vb
Public Class ClassForEverybody
```

只能在模块、接口或命名空间级别使用 `Public`。 这意味着，可以在源文件或命名空间级别或在接口、模块、类或结构中声明公共元素，而不是在过程中声明。
  
## <a name="protected"></a>受保护

声明语句中的[Protected](../../../language-reference/modifiers/protected.md)关键字指定只能从同一个类或从该类派生的类中访问元素。 下面的代码演示 `Protected` 声明的示例：

```vb
Protected Class ClassForMyHeirs
```

仅当声明类的成员时，才可以在类级别使用 `Protected`。 这意味着可以在类中声明受保护的元素，但不能在源文件或命名空间的级别，也可以在接口、模块、结构或过程中声明。

## <a name="friend"></a>Friend

声明语句中的[Friend](../../../language-reference/modifiers/friend.md)关键字指定可以从同一程序集内访问元素，但不能从程序集外部访问元素。 下面的代码演示 `Friend` 声明的示例：

```vb
Friend stringForThisProject As String
```

只能在模块、接口或命名空间级别使用 `Friend`。 这意味着，可以在源文件或命名空间级别或在接口、模块、类或结构中声明友元元素，而不是在过程中声明。

## <a name="protected-friend"></a>Protected Friend

声明语句中[受保护的 Friend](../../../language-reference/modifiers/protected-friend.md)关键字组合指定可以从派生的类或在同一程序集中访问元素。 下面的代码演示 `Protected Friend` 声明的示例：

```vb
Protected Friend stringForProjectAndHeirs As String
```

仅当声明类的成员时，才可以在类级别使用 `Protected Friend`。 这意味着，可以在类中声明受保护的 friend 元素，但不能在源文件或命名空间的级别，也可以在接口、模块、结构或过程中声明。

## <a name="private"></a>专用

声明语句中的[Private](../../../language-reference/modifiers/private.md)关键字指定只能从同一个模块、类或结构内部访问元素。 下面的代码演示 `Private` 声明的示例：

```vb
Private _numberForMeOnly As Integer
```

只能在模块级别使用 `Private`。 这意味着，可以在模块、类或结构中声明私有元素，但不能在源文件或命名空间的级别、接口内或过程中声明私有元素。

在模块级别，没有任何访问级别关键字的 `Dim` 语句等效于 `Private` 声明。 不过，您可能希望使用 `Private` 关键字来使代码更易于阅读和解释。

## <a name="private-protected"></a>Private Protected

声明语句中的[私有受保护](../../../language-reference/modifiers/private-protected.md)关键字组合指定只能从同一个类中访问元素，也可以从与包含类相同的程序集中找到的派生类中访问。 从 Visual Basic 15.5 开始支持 `Private Protected` 访问修饰符。

下面的示例演示了一个 `Private Protected` 声明：

```vb
Private Protected internalValue As Integer
```

只能在类内部声明 `Private Protected` 元素。 不能在接口或结构中声明它，也不能在源文件或命名空间的级别、接口或结构中或在过程中声明它。

Visual Basic 15.5 及更高版本支持 `Private Protected` 访问修饰符。 若要使用它，请将以下元素添加到 Visual Basic 项目（ *\*.vbproj*）文件中。 只要您的系统上安装了 Visual Basic 15.5 或更高版本，就可以利用 Visual Basic 编译器最新版本支持的所有语言功能：

```xml
<PropertyGroup>
   <LangVersion>latest</LangVersion>
</PropertyGroup>
```

若要使用 `Private Protected` 访问修饰符，必须将以下元素添加到 Visual Basic 项目（ *\*.vbproj*）文件中：

```xml
<PropertyGroup>
   <LangVersion>15.5</LangVersion>
</PropertyGroup>
```

有关详细信息，请参阅[设置 Visual Basic 语言版本](../../../language-reference/configure-language-version.md)。

## <a name="access-modifiers"></a>访问修饰符

指定访问级别的关键字称为*访问修饰符*。 下表比较了访问修饰符：

|访问修饰符|授予的访问级别|可使用此访问级别声明的元素|可在其中使用此修饰符的声明上下文|
|---------------------|--------------------------|-----------------------------------------------------|----------------------------------------------------------------|
|`Public`|L<br /><br /> 可以查看公共元素的任何代码都可以访问它|界面<br /><br /> 模块<br /><br /> 类<br /><br /> 结构<br /><br /> 结构成员<br /><br /> 过程<br /><br /> 属性<br /><br /> 成员变量<br /><br /> 常量<br /><br /> 枚举<br /><br /> Events<br /><br /> 外部声明<br /><br /> 委派|源文件<br /><br /> 命名空间<br /><br /> 接口<br /><br /> 模块<br /><br /> 实例<br /><br /> 结构|
|`Protected`|Derivational:<br /><br /> 类中声明受保护元素的代码或从其派生的类可访问元素|界面<br /><br /> 类<br /><br /> 结构<br /><br /> 过程<br /><br /> 属性<br /><br /> 成员变量<br /><br /> 常量<br /><br /> 枚举<br /><br /> Events<br /><br /> 外部声明<br /><br /> 委派|实例|
|`Friend`|程序集：<br /><br /> 声明 friend 元素的程序集中的代码可以访问它|界面<br /><br /> 模块<br /><br /> 类<br /><br /> 结构<br /><br /> 结构成员<br /><br /> 过程<br /><br /> 属性<br /><br /> 成员变量<br /><br /> 常量<br /><br /> 枚举<br /><br /> Events<br /><br /> 外部声明<br /><br /> 委派|源文件<br /><br /> 命名空间<br /><br /> 接口<br /><br /> 模块<br /><br /> 实例<br /><br /> 结构|
|`Protected` `Friend`|`Protected` 和 `Friend`的联合：<br /><br /> 与受保护的 friend 元素相同的类或同一程序集中的代码，或从该元素的类派生的任何类中的代码，都可以访问它|界面<br /><br /> 类<br /><br /> 结构<br /><br /> 过程<br /><br /> 属性<br /><br /> 成员变量<br /><br /> 常量<br /><br /> 枚举<br /><br /> Events<br /><br /> 外部声明<br /><br /> 委派|实例|
|`Private`|声明上下文：<br /><br /> 声明私有元素的类型中的代码（包括包含类型内的代码）可以访问元素|界面<br /><br /> 类<br /><br /> 结构<br /><br /> 结构成员<br /><br /> 过程<br /><br /> 属性<br /><br /> 成员变量<br /><br /> 常量<br /><br /> 枚举<br /><br /> Events<br /><br /> 外部声明<br /><br /> 委派|模块<br /><br /> 实例<br /><br /> 结构|
|`Private Protected`|类中的代码，该代码声明一个私有受保护的元素，或在与 bas 类相同的程序集中找到的派生类中的代码。|界面<br /><br /> 类<br /><br /> 结构<br /><br /> 过程<br /><br /> 属性<br /><br /> 成员变量<br /><br /> 常量<br /><br /> 枚举<br /><br /> Events<br /><br /> 外部声明<br /><br /> 委派|实例|

## <a name="see-also"></a>另请参阅

- [Dim 语句](../../../language-reference/statements/dim-statement.md)
- [Static](../../../language-reference/modifiers/static.md)
- [已声明的元素名称](declared-element-names.md)
- [对已声明元素的引用](references-to-declared-elements.md)
- [已声明元素的特性](declared-element-characteristics.md)
- [生存期（Visual Basic](lifetime.md)
- [范围 Visual Basic](scope.md)
- [如何：控制变量的可用性](how-to-control-the-availability-of-a-variable.md)
- [变量](../variables/index.md)
- [变量声明](../variables/variable-declaration.md)
