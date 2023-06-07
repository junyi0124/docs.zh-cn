---
title: 将自定义活动属性绑定到设计器控件
ms.date: 03/30/2017
ms.assetid: 2e8061ea-10f5-407c-a31f-d0d74ce12f27
ms.openlocfilehash: 142a9eb273a98d3a2d83a1239d6d7c891d5cc305
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61945894"
---
# <a name="binding-a-custom-activity-property-to-a-designer-control"></a>将自定义活动属性绑定到设计器控件

将文本框设计器控件与活动参数绑定非常简单；但将复杂设计器控件（如组合框）与活动参数绑定则可能非常困难。 本主题讨论如何将活动自变量与自定义活动设计器上的组合框控件绑定。

## <a name="creating-the-combo-box-item-converter"></a>创建组合框项转换器

1. 在 Visual Studio 中新建一个名为 CustomProperty 的空解决方案。

2. 新建一个名为 ComboBoxItemConverter 的类。 添加对 System.Windows.Data 的引用，并让该类从 <xref:System.Windows.Data.IValueConverter> 派生。 让 Visual Studio 实现该接口以生成 `Convert` 和 `ConvertBack` 的存根。

3. 将以下代码添加到 `Convert` 方法中。 此代码会将 <xref:System.Activities.InArgument%601> 类型的活动 <xref:System.String> 转换为要放入设计器中的值。

    ```csharp
    ModelItem modelItem = value as ModelItem;
    if (value != null)
    {
        InArgument<string> inArgument = modelItem.GetCurrentValue() as InArgument<string>;

        if (inArgument != null)
        {
            Activity<string> expression = inArgument.Expression;
            VisualBasicValue<string> vbexpression = expression as VisualBasicValue<string>;
            Literal<string> literal = expression as Literal<string>;

            if (literal != null)
            {
                return "\"" + literal.Value + "\"";
            }
            else if (vbexpression != null)
            {
                return vbexpression.ExpressionText;
            }
        }
    }
    return null;
    ```

     还可以使用 <xref:Microsoft.CSharp.Activities.CSharpValue%601>（而不是 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601>）创建上面代码段中的表达式。

    ```csharp
    ModelItem modelItem = value as ModelItem;
    if (value != null)
    {
        InArgument<string> inArgument = modelItem.GetCurrentValue() as InArgument<string>;

        if (inArgument != null)
        {
            Activity<string> expression = inArgument.Expression;
            CSharpValue<string> csexpression = expression as CSharpValue<string>;
            Literal<string> literal = expression as Literal<string>;

            if (literal != null)
            {
                return "\"" + literal.Value + "\"";
            }
            else if (csexpression != null)
            {
                return csexpression.ExpressionText;
            }
        }
    }
    return null;
    ```

4. 将以下代码添加到 `ConvertBack` 方法中。 此代码会将传入的组合框项再转回为 <xref:System.Activities.InArgument%601>。

    ```csharp
    // Convert combo box value to InArgument<string>
                string itemContent = (string)((ComboBoxItem)value).Content;
                VisualBasicValue<string> vbArgument = new VisualBasicValue<string>(itemContent);
                InArgument<string> inArgument = new InArgument<string>(vbArgument);
                return inArgument;
    ```

     还可以使用 <xref:Microsoft.CSharp.Activities.CSharpValue%601>（而不是 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601>）创建上面代码段中的表达式。

    ```csharp
    // Convert combo box value to InArgument<string>
                string itemContent = (string)((ComboBoxItem)value).Content;
                CSharpValue<string> csArgument = new CSharpValue<string>(itemContent);
                InArgument<string> inArgument = new InArgument<string>(csArgument);
                return inArgument;
    ```

## <a name="adding-the-comboboxitemconverter-to-the-custom-designer-of-an-activity"></a>向活动的自定义设计器添加 ComboBoxItemConverter

1. 向项目添加一个新项。 在“新建项”对话框中，选择“工作流”节点并选择“活动设计器”作为新项的类型。 将该项命名为 CustomPropertyDesigner。

2. 向新设计器添加一个组合框。 在项属性中，向组合框添加两项，其“内容”值分别为“Item1”和“Item2”。

3. 修改组合框的 XAML，以将新的项转换器添加为要用于组合框的项转换器。 转换器作为资源添加到 ActivityDesigner.Resources 段中，然后在 <xref:System.Windows.Controls.ComboBox> 的 Converter 属性中指定该转换器。 请注意，项目的命名空间是在活动设计器的命名空间属性中指定的；如果要将该设计器用在另一个项目中，也需要更改此命名空间。

    ```xaml
    <sap:ActivityDesigner x:Class="CustomProperty.CustomPropertyDesigner"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:c="clr-namespace:CustomProperty"
        xmlns:sap="clr-namespace:System.Activities.Presentation;assembly=System.Activities.Presentation"
        xmlns:sapv="clr-namespace:System.Activities.Presentation.View;assembly=System.Activities.Presentation">

        <sap:ActivityDesigner.Resources>
            <ResourceDictionary>
                <c:ComboBoxItemConverter x:Key="comboBoxItemConverter"/>
            </ResourceDictionary>
        </sap:ActivityDesigner.Resources>
        <Grid>
            <ComboBox SelectedValue="{Binding Path=ModelItem.Text, Mode=TwoWay, Converter={StaticResource comboBoxItemConverter}}"  Height="23" HorizontalAlignment="Left" Margin="132,5,0,0" Name="comboBox1" VerticalAlignment="Top" Width="120" ItemsSource="{Binding}">
                <ComboBoxItem>item1</ComboBoxItem>
                <ComboBoxItem>item2</ComboBoxItem>
            </ComboBox>
        </Grid>
    </sap:ActivityDesigner>
    ```

4. 新建一个 <xref:System.Activities.CodeActivity> 类型的项。 对本示例而言，IDE 为活动创建的默认代码就足够了。

5. 向类定义添加下列属性：

    ```csharp
    [Designer(typeof(CustomPropertyDesigner))]
    ```

     此行代码将新设计器与新类相关联。

 新活动现在应当已与设计器关联起来。 若要测试新活动，将其添加到一个工作流中，然后将组合框设置为它的两个值。 属性窗口将会更新以反映组合框的值。
