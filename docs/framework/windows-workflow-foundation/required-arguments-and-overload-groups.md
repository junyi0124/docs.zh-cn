---
title: 必需自变量和重载组
ms.date: 03/30/2017
ms.assetid: 4ca3ed06-b9af-4b85-8b70-88c2186aefa3
ms.openlocfilehash: 452285b1f5b73ecf75fc50f59365aa2633f26e42
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245869"
---
# <a name="required-arguments-and-overload-groups"></a>必需自变量和重载组

可以对活动进行配置，以便必须绑定某些参数才能有效执行该活动。 `RequiredArgument` 特性用于指示活动中的某些自变量是必需自变量，`OverloadGroup` 特性用于将必需自变量的多个类别组合在一起。 使用这些特性，活动作者可以提供简单或复杂的活动验证配置。  
  
## <a name="using-required-arguments"></a>使用必需参数  

 若要在活动中使用 `RequiredArgument` 特性，请使用 <xref:System.Activities.RequiredArgumentAttribute> 指示所需参数。 下面的示例定义了一个含有两个必需自变量的 `Add` 活动。  
  
```csharp  
public sealed class Add : CodeActivity<int>  
{  
    [RequiredArgument]  
    public InArgument<int> Operand1 { get; set; }  
  
    [RequiredArgument]  
    public InArgument<int> Operand2 { get; set; }  
  
    protected override int Execute(CodeActivityContext context)  
    {  
        return Operand1.Get(context) + Operand2.Get(context);  
    }  
}  
```  
  
 在 XAML 中，也请使用 <xref:System.Activities.RequiredArgumentAttribute> 来指示所需参数。 下面的示例通过使用三个参数定义了一个 `Add` 活动，并使用 <xref:System.Activities.Statements.Assign%601> 活动执行添加操作。  
  
```xaml  
<Activity x:Class="ValidationDemo.Add" ...>  
  <x:Members>  
    <x:Property Name="Operand1" Type="InArgument(x:Int32)">  
      <x:Property.Attributes>  
        <RequiredArgumentAttribute />  
      </x:Property.Attributes>  
    </x:Property>  
    <x:Property Name="Operand2" Type="InArgument(x:Int32)">  
      <x:Property.Attributes>  
        <RequiredArgumentAttribute />  
      </x:Property.Attributes>  
    </x:Property>  
    <x:Property Name="Result" Type="OutArgument(x:Int32)" />  
  </x:Members>  
  <Assign>  
    <Assign.To>  
      <OutArgument x:TypeArguments="x:Int32">[Result]</OutArgument>  
    </Assign.To>  
    <Assign.Value>  
      <InArgument x:TypeArguments="x:Int32">[Operand1 + Operand2]</InArgument>  
    </Assign.Value>  
  </Assign>  
</Activity>  
```  
  
 如果使用了该活动，但未绑定任何一个必需参数，则会返回以下验证错误。  
  
 **未提供必需活动自变量“Operand1”的值。**  
> [!NOTE]
> 有关检查和处理验证错误和警告的详细信息，请参阅 [调用活动验证](invoking-activity-validation.md)。  
  
## <a name="using-overload-groups"></a>使用重载组

重载组提供了一种方法，指示哪些自变量组合在活动中有效。 使用 <xref:System.Activities.OverloadGroupAttribute> 将参数组合在一起。 为每个组指定一个名称，该名称由指定 <xref:System.Activities.OverloadGroupAttribute> 。 仅当绑定重载组中的一组参数时，该活动才有效。 下面的示例定义了一个 `CreateLocation` 类。  
  
```csharp  
class CreateLocation: Activity  
{  
    [RequiredArgument]  
    public InArgument<string> Name { get; set; }  
  
    public InArgument<string> Description { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("G1")]  
    public InArgument<int> Latitude { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("G1")]  
    public InArgument<int> Longitude { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("G2")]  
    [OverloadGroup("G3")]  
    public InArgument<string> Street { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("G2")]  
    public InArgument<string> City { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("G2")]  
    public InArgument<string> State { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("G3")]  
    public InArgument<int> Zip { get; set; }
}  
```  
  
 该活动的目的是指定美国的某个位置。 为此，活动用户可以使用三组参数中的一组来指定该位置。 若要指定自变量的有效组合，请定义三个重载组。 `G1` 包含 `Latitude` 和 `Longitude` 自变量。 `G2` 包含 `Street`、`City` 和 `State`。 `G3` 包含 `Street` 和 `Zip`。 `Name` 也是一个必需的自变量，但它不是重载组的一部分。 为使该活动有效，必须一起绑定 `Name` 和一个（且只有一个）重载组中的所有参数。  
  
 在下面的示例中，从 " [数据库访问" 活动](./samples/database-access-activities.md) 示例中，有两个重载组： `ConnectionString` 和 `ConfigFileSectionName` 。 为使此活动有效，必须绑定 `ProviderName` 和 `ConnectionString` 自变量，或者绑定 `ConfigName` 自变量，但不能这些自变量一起绑定。  
  
```csharp  
public class DbUpdate: AsyncCodeActivity  
{  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DefaultValue(null)]  
    public InArgument<string> ProviderName { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConnectionString")]  
    [DependsOn("ProviderName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConnectionString { get; set; }  
  
    [RequiredArgument]  
    [OverloadGroup("ConfigFileSectionName")]  
    [DefaultValue(null)]  
    public InArgument<string> ConfigName { get; set; }  
  
    [DefaultValue(null)]  
    public CommandType CommandType { get; set; }  
  
    [RequiredArgument]  
    public InArgument<string> Sql { get; set; }  
  
    [DependsOn("Sql")]  
    [DefaultValue(null)]  
    public IDictionary<string, Argument> Parameters { get; }  
  
    [DependsOn("Parameters")]  
    public OutArgument<int> AffectedRecords { get; set; }
}  
```  
  
 定义重载组时：  
  
- 重载组不能为另一个重载组的子集或等价集。  
  
    > [!NOTE]
    > 此规则有一个例外。 如果某个重载组是另一重载组的子集，并且该子集仅包含 `RequiredArgument` 为 `false` 的参数，则该重载集有效。  
  
- 重载组可以重叠，但如果组的交集包含一个或两个重载组的所有必需自变量，则会出错。 在前面的示例中，`G2` 和 `G3` 重载组重叠，但因为交集不包含一个或两个组的所有自变量，所以这是有效的。  
  
 在重载组中绑定自变量时：  
  
- 如果重载组中的所有 `RequiredArgument` 参数均被绑定，则该组将被视为绑定的重载组。  
  
- 如果一个组具有零个 `RequiredArgument` 自变量且至少有一个自变量被绑定，则该组将被视为绑定的组。  
  
- 如果没有绑定任何重载组，除非其中的一个重载组中未包含任何 `RequiredArgument` 参数，否则它是一个验证错误。  
  
- 绑定多个重载组（即，绑定一个重载组中的所有必需参数，并同时绑定另一个重载组中的所有参数）的做法是错误的。
