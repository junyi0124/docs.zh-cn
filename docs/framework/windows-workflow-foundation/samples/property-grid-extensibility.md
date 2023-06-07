---
title: 属性网格扩展性-WF 示例
ms.date: 03/30/2017
ms.assetid: 3530c3a3-756d-4712-9f10-fb2897414d3a
ms.openlocfilehash: 130d8702795bccf0d5f28b5c0940bd7c25be3556
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715604"
---
# <a name="property-grid-extensibility"></a>属性网格扩展性

开发人员可以自定义属性网格，此网格将在设计器中选择给定活动时显示。 执行此操作可获得丰富的编辑体验。 此示例演示如何完成此操作。

## <a name="demonstrates"></a>演示文本

工作流设计器属性网格的扩展性。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Designer\PropertyGridExtensibility`

## <a name="discussion"></a>讨论

若要扩展属性网格，开发人员可以选择自定义属性网格编辑器的内联外观或提供一个为更高级的编辑图面显示的对话框。 此示例中演示了两类不同的编辑器；即内联编辑器和对话框编辑器。

## <a name="inline-editor"></a>内联编辑器

内联编辑器示例将演示如下内容：

- 创建一个从 <xref:System.Activities.Presentation.PropertyEditing.PropertyValueEditor> 派生的类型。

- 在构造函数中，<xref:System.Activities.Presentation.PropertyEditing.PropertyValueEditor.InlineEditorTemplate%2A> 值是使用 Windows Presentation Foundation （WPF）数据模板设置的。 虽然可以将其绑定到 XAML 模板，但在此示例中，代码用于初始化数据绑定。

- 数据模板具有在属性网格中呈现的项的 <xref:System.Activities.Presentation.PropertyEditing.PropertyValue> 的数据上下文。 请注意，在下面的代码（此代码来自 CustomInlineEditor.cs）中，稍后会将此上下文绑定到 `Value` 属性。

    ```csharp
    FrameworkElementFactory stack = new FrameworkElementFactory(typeof(StackPanel));
    FrameworkElementFactory slider = new FrameworkElementFactory(typeof(Slider));
    Binding sliderBinding = new Binding("Value");
    sliderBinding.Mode = BindingMode.TwoWay;
    slider.SetValue(Slider.MinimumProperty, 0.0);
    slider.SetValue(Slider.MaximumProperty, 100.0);
    slider.SetValue(Slider.ValueProperty, sliderBinding);
    stack.AppendChild(slider);
    ```

- 由于活动和设计器位于同一个程序集中，因此将在活动自身的静态构造函数中完成对活动设计器特性的注册，如 SimpleCodeActivity.cs 中的以下示例所示。

    ```csharp
    static SimpleCodeActivity()
    {
        AttributeTableBuilder builder = new AttributeTableBuilder();
        builder.AddCustomAttributes(typeof(SimpleCodeActivity), "RepeatCount", new EditorAttribute(typeof(CustomInlineEditor), typeof(PropertyValueEditor)));
        builder.AddCustomAttributes(typeof(SimpleCodeActivity), "FileName", new EditorAttribute(typeof(FilePickerEditor), typeof(DialogPropertyValueEditor)));
        MetadataStore.AddAttributeTable(builder.CreateTable());
    }
    ```

## <a name="dialog-editor"></a>对话框编辑器

对话框编辑器示例将演示如下内容：

1. 创建一个从 <xref:System.Activities.Presentation.PropertyEditing.DialogPropertyValueEditor> 派生的类型。

2. 使用 WPF 数据模板设置构造函数中的 <xref:System.Activities.Presentation.PropertyEditing.PropertyValueEditor.InlineEditorTemplate%2A> 值。 可以在 XAML 中创建该值，但在此示例中，将在代码中创建它。

3. 数据模板具有在属性网格中呈现的项的 <xref:System.Activities.Presentation.PropertyEditing.PropertyValue> 的数据上下文。 在下面的代码中，稍后会将其绑定到 `Value` 属性。 此外，包含 <xref:System.Activities.Presentation.PropertyEditing.EditModeSwitchButton> 以提供引发 FilePickerEditor.cs 中的对话框的按钮也很重要。

    ```csharp
    this.InlineEditorTemplate = new DataTemplate();

    FrameworkElementFactory stack = new FrameworkElementFactory(typeof(StackPanel));
    stack.SetValue(StackPanel.OrientationProperty, Orientation.Horizontal);
    FrameworkElementFactory label = new FrameworkElementFactory(typeof(Label));
    Binding labelBinding = new Binding("Value");
    label.SetValue(Label.ContentProperty, labelBinding);
    label.SetValue(Label.MaxWidthProperty, 90.0);

    stack.AppendChild(label);

    FrameworkElementFactory editModeSwitch = new FrameworkElementFactory(typeof(EditModeSwitchButton));

    editModeSwitch.SetValue(EditModeSwitchButton.TargetEditModeProperty, PropertyContainerEditMode.Dialog);

    stack.AppendChild(editModeSwitch);

    this.InlineEditorTemplate.VisualTree = stack;
    ```

4. 重写设计器类型中的 <xref:System.Activities.Presentation.PropertyEditing.DialogPropertyValueEditor.ShowDialog%2A> 方法以处理对话框的显示。 在此示例中，显示了基本 <xref:System.Windows.Forms.FileDialog>。

    ```csharp
    public override void ShowDialog(PropertyValue propertyValue, IInputElement commandSource)
    {
        Microsoft.Win32.OpenFileDialog ofd = new Microsoft.Win32.OpenFileDialog();
        if (ofd.ShowDialog() == true)
        {
            propertyValue.Value = ofd.FileName;
        }
    }
    ```

5. 由于活动和设计器位于同一个程序集中，因此将在活动自身的静态构造函数中完成对活动设计器特性的注册，如 SimpleCodeActivity.cs 中的以下示例所示。

    ```csharp
    static SimpleCodeActivity()
    {
        AttributeTableBuilder builder = new AttributeTableBuilder();
        builder.AddCustomAttributes(typeof(SimpleCodeActivity), "RepeatCount", new EditorAttribute(typeof(CustomInlineEditor), typeof(PropertyValueEditor)));
        builder.AddCustomAttributes(typeof(SimpleCodeActivity), "FileName", new EditorAttribute(typeof(FilePickerEditor), typeof(DialogPropertyValueEditor)));
        MetadataStore.AddAttributeTable(builder.CreateTable());
    }
    ```

## <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 生成解决方案，然后打开 Workflow1.xaml。

2. 将**SimpleCodeActivity**从工具箱拖到设计器画布上。

3. 单击 " **SimpleCodeActivity** "，然后打开属性网格，其中有滑块控件和文件选取控件。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Designer\PropertyGridExtensibility`
