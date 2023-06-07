---
title: 动态更新
ms.date: 03/30/2017
ms.assetid: 8b6ef19b-9691-4b4b-824c-3c651a9db96e
ms.openlocfilehash: 2553e6a29f498a9b53752d6c191df21a391dee34
ms.sourcegitcommit: 4735bb7741555bcb870d7b42964d3774f4897a6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2019
ms.locfileid: "66378281"
---
# <a name="dynamic-update"></a>动态更新

动态更新为工作流应用程序开发人员提供了一种机制，可用于更新持久化工作流实例的工作流定义。 操作可以是实施 bug 修复、实施新的要求或适应意外的变化。 本主题概述了.NET Framework 4.5 中引入的动态更新功能。

## <a name="dynamic-update"></a>动态更新

为了将动态更新应用于持久化的工作流实例，会创建一个 <xref:System.Activities.DynamicUpdate.DynamicUpdateMap>，其中包含描述如何修改持久化工作流实例以反映所需更改的运行时说明。 更新映射一旦创建，即应用于所需的持久化工作流实例。 应用动态更新后，可能会使用新的更新工作流定义来恢复工作流实例。 创建和应用更新映射须执行四个步骤的操作。

1. [准备动态更新工作流定义](dynamic-update.md#Prepare)

2. [更新工作流定义以反映所需的更改](dynamic-update.md#Update)

3. [创建更新映射](dynamic-update.md#Create)

4. [将更新映射应用于所需的持久化工作流实例](dynamic-update.md#Apply)

> [!NOTE]
> 请注意，步骤 1 到 3 涵盖更新映射的创建，可不通过应用更新来执行。 一种常见方案，工作流开发人员将创建更新映射脱机，然后管理员将在以后的更新。

本主题将一个新活动添加至已编译 Xaml 工作流的持久性实例中，借此提供对动态更新过程的概述。

### <a name="Prepare"></a> 准备动态更新工作流定义

动态更新过程的第一步是准备要更新的工作流定义。 这是通过调用 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType> 方法并传入要修改的工作流定义来实现的。 方法将验证然后遍历工作流树，以标识所有对象（如需要标记的公共活动和变量），以便稍后将其与修改的工作流定义进行比较。 完成此操作后，将对工作流树进行克隆并将其附加到原始工作流定义。 创建更新映射时，将比较工作流定义的更新版本和原始工作流定义，并根据比较差异生成更新映射。

若要为动态更新准备 Xaml 工作流，可将其加载到 <xref:System.Activities.ActivityBuilder>，然后 <xref:System.Activities.ActivityBuilder> 将传入 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType>。

> [!NOTE]
> 详细了解如何使用序列化工作流和<xref:System.Activities.ActivityBuilder>，请参阅[序列化工作流和活动与 XAML](serializing-workflows-and-activities-to-and-from-xaml.md)。

在以下示例中，`MortgageWorkflow` 定义（由带有若干子活动的 <xref:System.Activities.Statements.Sequence> 组成）将加载到 <xref:System.Activities.ActivityBuilder> 中，然后准备进行动态更新。 该方法返回后，<xref:System.Activities.ActivityBuilder> 包含原始工作流定义以及副本。

```csharp
// Load the MortgageWorkflow definition from Xaml into
// an ActivityBuilder.
XamlXmlReaderSettings readerSettings = new XamlXmlReaderSettings()
{
    LocalAssembly = Assembly.GetExecutingAssembly()
};

XamlXmlReader xamlReader = new XamlXmlReader(@"C:\WorkflowDefinitions\MortgageWorkflow.xaml",
    readerSettings);

ActivityBuilder ab = XamlServices.Load(
    ActivityXamlServices.CreateBuilderReader(xamlReader)) as ActivityBuilder;

// Prepare the workflow definition for dynamic update.
DynamicUpdateServices.PrepareForUpdate(ab);
```

> [!NOTE]
> 若要下载本主题附带的示例代码，请参阅[动态更新示例代码](https://go.microsoft.com/fwlink/?LinkId=227905)。

### <a name="Update"></a> 更新工作流定义以反映所需的更改

一旦工作流定义准备好进行更新，就可以作出所需的更改。 你可以添加或删除活动，添加、移动或删除公共变量，添加或删除自变量，并对活动委托的签名进行更改。 您不能删除正在运行的活动或更改正在运行的委托的签名。 这些更改可利用代码，或在预先承载的工作流设计器中进行。 在以下示例中，自定义 `VerifyAppraisal` 活动添加到 Sequence，后者构成前例中 `MortgageWorkflow` 的主体。

```csharp
// Make desired changes to the definition. In this example, we are
// inserting a new VerifyAppraisal activity as the 3rd child of the root Sequence.
VerifyAppraisal va = new VerifyAppraisal
{
    Result = new VisualBasicReference<bool>("LoanCriteria")
};

// Get the Sequence that makes up the body of the workflow.
Sequence s = ab.Implementation as Sequence;

// Insert the new activity into the Sequence.
s.Activities.Insert(2, va);
```

### <a name="Create"></a> 创建更新映射

修改完准备好进行更新的工作流定义之后，就可以创建更新映射。 为创建动态更新映射，需调用 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.CreateUpdateMap%2A?displayProperty=nameWithType> 方法。 这将返回一个 <xref:System.Activities.DynamicUpdate.DynamicUpdateMap>，它包含运行时修改持久化工作流实例所需的信息，以便可以使用新的工作流定义加载和恢复该实例。 在以下示例中，为上例中修改的 `MortgageWorkflow` 定义创建了一个动态映射。

```csharp
// Create the update map.
DynamicUpdateMap map = DynamicUpdateServices.CreateUpdateMap(ab);
```

此更新映射可以立即用于修改持久化工作流实例，或者采用更常见的做法，即进行保存并在以后应用更新。 保存更新映射的一种方法是将它序列化到文件中，如下面的示例中所示。

```csharp
// Serialize the update map to a file.
DataContractSerializer serializer = new DataContractSerializer(typeof(DynamicUpdateMap));
using (FileStream fs = System.IO.File.Open(@"C:\WorkflowDefinitions\MortgageWorkflow.map", FileMode.Create))
{
    serializer.WriteObject(fs, map);
}
```

当 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.CreateUpdateMap%2A?displayProperty=nameWithType> 返回时，克隆的工作流定义和其他动态更新信息（在调用 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.PrepareForUpdate%2A?displayProperty=nameWithType> 时添加）被删除，并且可以保存修改的工作流定义，以便在以后恢复更新工作流实例时使用。 在以下示例中，修改的工作流定义保存到 `MortgageWorkflow_v1.1.xaml` 中。

```csharp
// Save the modified workflow definition.
StreamWriter sw = File.CreateText(@"C:\WorkflowDefinitions\MortgageWorkflow_v1.1.xaml");
XamlWriter xw = ActivityXamlServices.CreateBuilderWriter(new XamlXmlWriter(sw, new XamlSchemaContext()));
XamlServices.Save(xw, ab);
sw.Close();
```

### <a name="Apply"></a> 将更新映射应用于所需的持久化工作流实例

更新映射创建后在任意时间都可应用。 可以使用 <xref:System.Activities.DynamicUpdate.DynamicUpdateMap> 实例（由 <xref:System.Activities.DynamicUpdate.DynamicUpdateServices.CreateUpdateMap%2A?displayProperty=nameWithType> 返回）立即应用，也可以使用保存的更新映射副本在以后应用。 若要更新工作流实例，可使用 <xref:System.Activities.WorkflowApplicationInstance> 将该实例加载到 <xref:System.Activities.WorkflowApplication.GetInstance%2A?displayProperty=nameWithType> 中。 接下来，使用更新的工作流定义和所需 <xref:System.Activities.WorkflowApplication> 创建一个 <xref:System.Activities.WorkflowIdentity>。 此 <xref:System.Activities.WorkflowIdentity> 可能不同于用来持久化保存原始工作流的标识，通常是为了反映持久化保存的实例已修改。 一旦创建了 <xref:System.Activities.WorkflowApplication>，即使用 <xref:System.Activities.WorkflowApplication.Load%2A?displayProperty=nameWithType>（采用 <xref:System.Activities.DynamicUpdate.DynamicUpdateMap>）的重载进行加载，然后通过调用 <xref:System.Activities.WorkflowApplication.Unload%2A?displayProperty=nameWithType> 将其卸载。 这适用于动态更新，并持久化保存更新的工作流实例。

```csharp
// Load the serialized update map.
DynamicUpdateMap map;
using (FileStream fs = File.Open(@"C:\WorkflowDefinitions\MortgageWorkflow.map", FileMode.Open))
{
    DataContractSerializer serializer = new DataContractSerializer(typeof(DynamicUpdateMap));
    object updateMap = serializer.ReadObject(fs);
    if (updateMap == null)
    {
        throw new ApplicationException("DynamicUpdateMap is null.");
    }

    map = (DynamicUpdateMap)updateMap;
}

// Retrieve a list of workflow instance ids that corresponds to the
// workflow instances to update. This step is the responsibility of
// the application developer.
List<Guid> ids = GetPersistedWorkflowIds();
foreach (Guid id in ids)
{
    // Get a proxy to the persisted workflow instance.
    SqlWorkflowInstanceStore store = new SqlWorkflowInstanceStore(connectionString);
    WorkflowApplicationInstance instance = WorkflowApplication.GetInstance(id, store);

    // If desired, you can inspect the WorkflowIdentity of the instance
    // using the DefinitionIdentity property to determine whether to apply
    // the update.
    Console.WriteLine(instance.DefinitionIdentity);

    // Create a workflow application. You must specify the updated workflow definition, and
    // you may provide an updated WorkflowIdentity if desired to reflect the update.
    WorkflowIdentity identity = new WorkflowIdentity
    {
        Name = "MortgageWorkflow v1.1",
        Version = new Version(1, 1, 0, 0)
    };

    // Load the persisted workflow instance using the updated workflow definition
    // and with an updated WorkflowIdentity. In this example the MortgageWorkflow class
    // contains the updated definition.
    WorkflowApplication wfApp = new WorkflowApplication(new MortgageWorkflow(), identity);

    // Apply the dynamic update on the loaded instance.
    wfApp.Load(instance, map);

    // Unload the updated instance.
    wfApp.Unload();
}
```

### <a name="resuming-an-updated-workflow-instance"></a>恢复更新的工作流实例

一旦应用了动态更新，即可恢复工作流实例。 注意，必须使用新更新的定义和 <xref:System.Activities.WorkflowIdentity>。

> [!NOTE]
> 有关使用详细信息<xref:System.Activities.WorkflowApplication>并<xref:System.Activities.WorkflowIdentity>，请参阅[使用 WorkflowIdentity 和版本控制](using-workflowidentity-and-versioning.md)。

在以下示例中，前面示例中的 `MortgageWorkflow_v1.1.xaml` 工作流已经过编译，并使用更新的工作流定义进行加载和恢复。

```csharp
// Load the persisted workflow instance using the updated workflow definition
// and updated WorkflowIdentity.
WorkflowIdentity identity = new WorkflowIdentity
{
    Name = "MortgageWorkflow v1.1",
    Version = new Version(1, 1, 0, 0)
};

WorkflowApplication wfApp = new WorkflowApplication(new MortgageWorkflow(), identity);

// Configure persistence and desired workflow event handlers.
// (Omitted for brevity.)
ConfigureWorkflowApplication(wfApp);

// Load the persisted workflow instance.
wfApp.Load(InstanceId);

// Resume the workflow.
// wfApp.ResumeBookmark(...);
```

> [!NOTE]
> 若要下载本主题附带的示例代码，请参阅[动态更新示例代码](https://go.microsoft.com/fwlink/?LinkId=227905)。
