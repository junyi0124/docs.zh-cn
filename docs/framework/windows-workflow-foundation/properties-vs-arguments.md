---
title: 属性与自变量
ms.date: 03/30/2017
ms.assetid: 14651389-4a49-4cbb-9ddf-c83fdc155df1
ms.openlocfilehash: 2ed443b962e6cccb8a0f670f1dbd744bfa8ed354
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245921"
---
# <a name="properties-vs-arguments"></a>属性与自变量

可使用多个选项将数据传入活动中。 除使用 <xref:System.Activities.InArgument> 之外，还可以使用标准 CLR 属性或公共 <xref:System.Activities.ActivityAction> 属性开发接收数据的活动。 本主题讨论如何选择适当的方法类型。  
  
## <a name="using-clr-properties"></a>使用 CLR 属性  

 在将数据传入活动中时，CLR 属性（即使用 Get 和 Set 例程来公开数据的公共方法）是受最多限制的选项。 在编译解决方案时，必须知道传入 CLR 属性中的参数的值；此值对于每个工作流实例都是相同的。 这样，传入 CLR 属性中的值会与代码中定义的常量类似；此值不能针对活动的生存期发生更改，也无法随活动实例的不同而不同。 <xref:System.Activities.Expressions.InvokeMethod%601.MethodName%2A> 是一个由活动公开的 CLR 属性的示例；此活动调用的方法名无法随运行时条件的不同而不同，并且该方法名对于该活动的每个实例都是相同的。  
  
## <a name="using-arguments"></a>使用参数  

 如果在活动的生存期内只计算一次数据，则应使用自变量；也就是说，它的值在活动的生存期内将不会发生更改，但该值会随活动实例的不同而不同 <xref:System.Activities.Statements.If.Condition%2A> 是一个只计算一次的值的示例；因此，它将定义为一个参数。 <xref:System.Activities.Statements.WriteLine.Text%2A> 是另一个应定义为参数的方法的示例，它在活动的执行期间只会计算一次，但它会随活动实例的不同而不同。  
  
## <a name="using-activityaction"></a>使用 ActivityAction  

 如果需要在活动的执行生存期内多次计算数据，则应使用 <xref:System.Activities.ActivityAction>。 例如，为 <xref:System.Activities.Statements.While.Condition%2A> 循环的每次迭代计算 <xref:System.Activities.Statements.While> 属性。 如果将 <xref:System.Activities.InArgument> 用于此目的，则循环将永不会退出，因为将不会为每个迭代重新计算该参数，并且将始终返回相同的结果。
