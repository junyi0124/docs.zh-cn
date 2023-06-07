---
title: End <keyword> 语句
ms.date: 07/20/2015
f1_keywords:
- vb.EndDefinition
helpviewer_keywords:
- End keyword [Visual Basic]
ms.assetid: 42d6e088-ab0f-4cda-88e8-fdce3e5fcf4f
ms.openlocfilehash: 87f4724cc036e6e0bdf0b558854a4034f45b9ab5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343739"
---
# <a name="end-keyword-statement-visual-basic"></a>End \<关键字 > 语句（Visual Basic）

后跟附加的关键字时，将终止该关键字引入的语句块的定义。

## <a name="syntax"></a>语法

```vb
End AddHandler
End Class
End Enum
End Event
End Function
End Get
End If
End Interface
End Module
End Namespace
End Operator
End Property
End RaiseEvent  
End RemoveHandler  
End Select
End Set
End Structure
End Sub
End SyncLock
End Try
End While
End With  
```  
  
## <a name="parts"></a>部件

|部件|说明|
|---|---|
|`End`|必需。 终止编程元素的定义。|
|`AddHandler`|需要终止自定义[事件语句](event-statement.md)中的匹配 `AddHandler` 语句开始的 `AddHandler` 访问器。|
|`Class`|需要终止由匹配的[类语句](class-statement.md)开始的类定义。|
|`Enum`|需要终止由匹配[枚举语句](enum-statement.md)开始的枚举定义。|
|`Event`|需要终止匹配[事件语句](event-statement.md)开始的 `Custom` 事件定义。|  
|`Function`|需要终止由匹配[函数语句](function-statement.md)开始的 `Function` 过程定义。 如果执行过程中遇到 `End Function` 语句，则控件会返回到调用代码。|
|`Get`|需要终止由匹配的[Get 语句](get-statement.md)开始的 `Property` 过程定义。 如果执行遇到 `End Get` 语句，控制将返回到请求属性值的语句。|
|`If`|需要终止 `If`...`Then`...`Else` 按匹配的 `If` 语句开始的块定义。 请参阅[If .。。Then .。。Else 语句](if-then-else-statement.md)。|
|`Interface`|需要终止由匹配的[Interface 语句](interface-statement.md)开始的接口定义。|
|`Module`|需要终止匹配[Module 语句](module-statement.md)开始的模块定义。|
|`Namespace`|需要终止由匹配的[命名空间语句](namespace-statement.md)开始的命名空间定义。|
|`Operator`|需要终止由匹配的[Operator 语句](operator-statement.md)开始的运算符定义。|
|`Property`|需要终止由匹配的[属性语句](property-statement.md)开始的属性定义。|
|`RaiseEvent`|需要终止自定义[事件语句](event-statement.md)中的匹配 `RaiseEvent` 语句开始的 `RaiseEvent` 访问器。|
|`RemoveHandler`|需要终止自定义[事件语句](event-statement.md)中的匹配 `RemoveHandler` 语句开始的 `RemoveHandler` 访问器。|
|`Select`|需要终止 `Select`...`Case` 块定义由匹配的 `Select` 语句开始。 请参阅[Select .。。Case 语句](select-case-statement.md)。  
|`Set`|需要终止由匹配[Set 语句](set-statement.md)开始的 `Property` 过程定义。 如果执行遇到 `End Set` 语句，控制将返回到设置属性值的语句。  
|`Structure`|需要终止由匹配的[结构语句](structure-statement.md)开始的结构定义。  
|`Sub`|需要终止由匹配的[Sub 语句](sub-statement.md)开始的 `Sub` 过程定义。 如果执行过程中遇到 `End Sub` 语句，则控件会返回到调用代码。  
|`SyncLock`|需要终止由匹配的 `SyncLock` 语句开始的 `SyncLock` 块定义。 请参阅[SyncLock 语句](synclock-statement.md)。  
|`Try`|需要终止 `Try`...`Catch`...`Finally` 按匹配的 `Try` 语句开始的块定义。 请参阅[Try .。。Catch .。。Finally 语句](try-catch-finally-statement.md)。  
|`While`|需要终止由匹配的 `While` 语句开始的 `While` 循环定义。 查看[.。。End While 语句](while-end-while-statement.md)。  
|`With`| 需要终止由匹配的 `With` 语句开始的 `With` 块定义。 查看[方式 .。。End With 语句](with-end-with-statement.md)。  
|||
  
## <a name="directives"></a>指令

如果前面有一个数字符号（`#`），则 `End` 关键字将终止相应指令引入的预处理块。  

```vb
#End ExternalSource
#End If
#End Region
```

|部件|说明|
|---|---|
|`#End`|必需。 终止预处理块的定义。|
|`ExternalSource`|需要终止由匹配的[#ExternalSource 指令](../directives/externalsource-directive.md)开始的外部源块。|
|`If`|需要终止由匹配的 `#If` 指令开始的条件编译块。 请参阅[#If .。。Then ... #Else 指令](../directives/if-then-else-directives.md)。|
|`Region`|需要终止由匹配的[#Region 指令](../directives/region-directive.md)开始的源区域块。|
|||

## <a name="remarks"></a>备注

[End 语句](end-statement.md)没有其他关键字，则立即终止执行。

## <a name="smart-device-developer-notes"></a>智能设备开发人员说明  

不支持不带额外关键字的 `End` 语句。  
  
## <a name="see-also"></a>另请参阅

- [End 语句](end-statement.md)
