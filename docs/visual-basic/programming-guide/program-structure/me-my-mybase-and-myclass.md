---
title: Me、My、MyBase 和 MyClass
ms.date: 07/20/2015
f1_keywords:
- MyClass
- vb.Me
- MyBase
- vb.MyBase
- Me
- vb.MyClass
- vb.This
- vb.My
helpviewer_keywords:
- My object
- self-reference [Visual Basic], Me keyword
- MyClass keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], referring to the current instance of an object
- Me keyword [Visual Basic]
- self-reference
- current instance [Visual Basic], Me keyword
- MyBase keyword [Visual Basic], relationship to similar programming elements
ms.assetid: f8e241ae-b1ed-4886-9aa0-08c632154029
ms.openlocfilehash: cc96f39d9dc37b7f1a5d8205e145869fb1b5ecef
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072231"
---
# <a name="me-my-mybase-and-myclass-in-visual-basic"></a>Visual Basic 中的 Me、My、MyBase 和 MyClass

`Me`Visual Basic 中，、、 `My` `MyBase` 和的 `MyClass` 名称相似，但用途不同。 本主题将介绍其中的每个实体，以便将它们区分开来。  
  
## <a name="me"></a>我  

 `Me`关键字提供了一种方法，用于引用当前执行代码的类或结构的特定实例。 `Me` 的行为类似于引用当前实例的对象变量或结构变量。 使用 `Me` 对于将有关当前正在执行的类或结构实例的信息传递给另一个类、结构或模块中的过程特别有用。  
  
 例如，假设你在模块中具有以下过程。  
  
```vb  
Sub ChangeFormColor(FormName As Form)  
   Randomize()  
   FormName.BackColor = Color.FromArgb(Rnd() * 256, Rnd() * 256, Rnd() * 256)  
End Sub  
```  
  
 可以调用此过程，并 <xref:System.Windows.Forms.Form> 使用以下语句将类的当前实例作为参数传递。  
  
```vb  
ChangeFormColor(Me)  
```  
  
## <a name="my"></a>My  

 此 `My` 功能提供对多个 .NET Framework 类的简单直观的访问，使 Visual Basic 用户可以与计算机、应用程序、设置、资源等交互。  
  
## <a name="mybase"></a>MyBase  

 `MyBase`关键字的行为类似于引用类的当前实例的基类的对象变量。 `MyBase` 通常用于访问在派生类中被重写或隐藏的基类成员。 `MyBase.New` 用于从派生类构造函数中显式调用基类构造函数。  
  
## <a name="myclass"></a>MyClass  

 `MyClass`关键字的行为类似于引用最初实现的类的当前实例的对象变量。 `MyClass` 类似于 `Me` ，但其上的所有方法调用都视为方法 `NotOverridable` 。  
  
## <a name="see-also"></a>请参阅

- [继承基础知识](../language-features/objects-and-classes/inheritance-basics.md)
