---
title: 声明性约束
ms.date: 03/30/2017
ms.assetid: 67001ed1-7f4d-4ada-ae57-a31176901a53
ms.openlocfilehash: 9098a3d79337689fef6d37e4cccf3633d8128a10
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236450"
---
# <a name="declarative-constraints"></a>声明性约束

声明性约束为活动及其与其他活动的关系提供了一个功能强大的验证方法。 约束是在创作过程中为活动配置的，但工作流宿主也可以指定其他约束。 本主题概述了如何使用声明性约束提供活动验证。  
  
## <a name="using-declarative-constraints"></a>使用声明性约束  

 约束是包含验证逻辑的活动。 可以用代码或 XAML 创作此约束活动。 创建约束活动之后，活动作者可将此约束添加到该活动的 <xref:System.Activities.Activity.Constraints%2A> 属性中以进行验证，或者他们也可以使用该约束通过使用 <xref:System.Activities.Validation.ValidationSettings.AdditionalConstraints%2A> 实例的 <xref:System.Activities.Validation.ValidationSettings> 属性提供其他验证。 验证逻辑可以包含简单的验证（如验证活动的元数据），也可以执行考虑当前活动与其父级、子级和同级活动的关系的验证。 <xref:System.Activities.Validation.Constraint%601> 活动可用于创作约束，并且提供了多个其他验证活动以协助创建验证错误和警告并提供有关工作流中相关活动的信息。  
  
### <a name="assertvalidation-and-addvalidationerror"></a>AssertValidation 和 AddValidationError  

 <xref:System.Activities.Validation.AssertValidation> 活动计算其 <xref:System.Activities.Validation.AssertValidation.Assertion%2A> 属性引用的表达式，如果表达式的计算结果为 `false`，则会向 <xref:System.Activities.Validation.ValidationResults> 添加验证错误或警告。 <xref:System.Activities.Validation.AssertValidation.Message%2A> 属性描述验证错误，<xref:System.Activities.Validation.AssertValidation.IsWarning%2A> 属性指示验证失败为错误还是警告。 <xref:System.Activities.Validation.AssertValidation.IsWarning%2A> 的默认值为 `false`。  
  
 在下面的示例中，声明一个约束，如果验证的活动的 <xref:System.Activities.Activity.DisplayName%2A> 是两个字符或更少字符，则该约束返回一个验证警告。 用于 <xref:System.Activities.Validation.Constraint%601> 的泛型类型参数指定该约束验证的活动类型。 此约束使用 <xref:System.Activities.Activity> 作为泛型类型并且可用于验证所有类型的活动。  
  
```csharp  
public static Constraint ActivityDisplayNameIsNotSetWarning()  
{  
    DelegateInArgument<Activity> element = new DelegateInArgument<Activity>();  
  
    return new Constraint<Activity>  
    {  
        Body = new ActivityAction<Activity, ValidationContext>  
        {  
            Argument1 = element,  
            Handler = new AssertValidation  
            {  
                IsWarning = true,  
                Assertion = new InArgument<bool>(env => (element.Get(env).DisplayName.Length > 2)),  
                Message = new InArgument<string>("It is a best practice to have a DisplayName of more than 2 characters."),  
            }  
        }  
    };  
}  
```  
  
 若要为某个活动指定此约束，请将其添加到该活动的 <xref:System.Activities.Activity.Constraints%2A> 中，如以下代码示例所示。  
  
```csharp  
public sealed class SampleActivity : CodeActivity  
{  
    public SampleActivity()  
    {  
        base.Constraints.Add(ActivityDisplayNameIsNotSetWarning());  
    }  
  
    // Activity implementation omitted.  
}  
```  
  
 宿主还可使用 <xref:System.Activities.Validation.ValidationSettings.AdditionalConstraints%2A> 为工作流中的活动指定此约束，将在下一节中对此内容进行介绍。  
  
 <xref:System.Activities.Validation.AddValidationError> 活动用于生成验证错误或警告，而无需对表达式进行计算。 其属性类似于 <xref:System.Activities.Validation.AssertValidation> 并且可以将其与约束的流控制活动（如 <xref:System.Activities.Statements.If> 活动）结合使用。
  
### <a name="workflow-relationship-activities"></a>工作流关系活动

可以使用多个验证活动，这些活动提供工作流中与验证的活动有关的其他活动的相关信息。 <xref:System.Activities.Validation.GetParentChain> 返回一个活动集合，该集合包含当前活动和根活动之间的所有活动。 <xref:System.Activities.Validation.GetChildSubtree> 提供一个活动集合，该集合包含采用递归模式的子活动，并且 <xref:System.Activities.Validation.GetWorkflowTree> 获取工作流中的所有活动。  
  
下面的示例定义了一个 `CreateState` 活动。 `CreateState` 活动必须包含在 `CreateCountry` 活动中，并且 `GetParent` 方法返回一个强制此要求的约束。  `GetParent` 将 <xref:System.Activities.Validation.GetParentChain> 活动与 <xref:System.Activities.Statements.ForEach%601> 结合使用，以检查 `CreateState` 活动的父活动，从而确定是否满足要求。  
  
```csharp  
public sealed class CreateState : CodeActivity  
{  
    public CreateState()  
    {  
        base.Constraints.Add(CheckParent());  
        this.Cities = new List<Activity>();
    }  
  
    public List<Activity> Cities { get; set; }  
  
    public string Name { get; set; }
  
    static Constraint CheckParent()  
    {  
        DelegateInArgument<CreateState> element = new DelegateInArgument<CreateState>();  
        DelegateInArgument<ValidationContext> context = new DelegateInArgument<ValidationContext>();
        Variable<bool> result = new Variable<bool>();  
        DelegateInArgument<Activity> parent = new DelegateInArgument<Activity>();  
  
        return new Constraint<CreateState>  
        {
            Body = new ActivityAction<CreateState,ValidationContext>  
            {
                Argument1 = element,  
                Argument2 = context,  
                Handler = new Sequence  
                {  
                    Variables =  
                    {  
                        result
                    },  
                    Activities =  
                    {  
                        new ForEach<Activity>  
                        {
                            Values = new GetParentChain  
                            {  
                                ValidationContext = context
                            },  
                            Body = new ActivityAction<Activity>  
                            {
                                Argument = parent,
                                Handler = new If()  
                                {
                                    Condition = new InArgument<bool>((env) => object.Equals(parent.Get(env).GetType(),typeof(CreateCountry))),
                                    Then = new Assign<bool>  
                                    {  
                                        Value = true,  
                                        To = result  
                                    }  
                                }  
                            }
                        },  
                        new AssertValidation  
                        {  
                            Assertion = new InArgument<bool>(result),  
                            Message = new InArgument<string> ("CreateState has to be inside a CreateCountry activity"),
                        }  
                    }  
                }  
            }  
        };  
    }  
  
    protected override void Execute(CodeActivityContext context)  
    {  
        // not needed for the sample  
    }  
}  
```
  
## <a name="additional-constraints"></a>其他约束  

 工作流主机作者可以通过创建约束并将这些约束添加到 <xref:System.Activities.Validation.ValidationSettings.AdditionalConstraints%2A> 实例的 <xref:System.Activities.Validation.ValidationSettings> 字典中为工作流中的活动指定其他验证约束。 <xref:System.Activities.Validation.ValidationSettings.AdditionalConstraints%2A> 中的每个项目都包含应用约束的活动类型以及用于该活动类型的其他约束列表。 为工作流调用验证时，指定类型的每个活动（包括派生的类）都计算约束。 在本示例中，将上一节中的 `ActivityDisplayNameIsNotSetWarning` 约束应用于工作流中的所有活动。  
  
```csharp  
Activity wf = new Sequence  
{  
    // Workflow Details Omitted.  
};  
  
ValidationSettings settings = new ValidationSettings()  
{  
  
    AdditionalConstraints =  
    {  
        {typeof(Activity), new List<Constraint> {ActivityDisplayNameIsNotSetWarning()}},
    }  
};  
  
// Validate the workflow.  
ValidationResults results = ActivityValidationServices.Validate(wf, settings);  
  
// Evaluate the results.  
if (results.Errors.Count == 0 && results.Warnings.Count == 0)  
{  
    Console.WriteLine("No warnings or errors");  
}  
else  
{  
    foreach (ValidationError error in results.Errors)  
    {  
        Console.WriteLine("Error in " + error.Source.DisplayName + ": " + error.Message);  
    }  
    foreach (ValidationError warning in results.Warnings)  
    {  
        Console.WriteLine("Warning in " + warning.Source.DisplayName + ": " + warning.Message);  
    }  
}  
```  
  
 如果 <xref:System.Activities.Validation.ValidationSettings.OnlyUseAdditionalConstraints%2A> 的 <xref:System.Activities.Validation.ValidationSettings> 属性为 `true`，则在通过调用 <xref:System.Activities.Validation.ActivityValidationServices.Validate%2A> 调用验证时只计算指定的其他约束。 对于检查工作流的特定验证配置，这非常有用。 但是，请注意调用工作流时，对在工作流中配置的验证逻辑进行计算并且必须传递给该工作流才能成功开始。 有关调用验证的详细信息，请参阅 [调用活动验证](invoking-activity-validation.md)。
