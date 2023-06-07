---
ms.openlocfilehash: d25c14f93da5fe8acf06269554fed30ddc6bc95d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614362"
---
### <a name="operationcontextcurrent-may-return-null-when-called-in-a-using-clause"></a>OperationContext.Current 在 using 子句中被调用时可能返回 null

#### <a name="details"></a>详细信息

如果满足以下所有条件，那么 <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> 可能返回 `null` 且导致 <xref:System.NullReferenceException>：

- 在返回 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 的方法中检索 <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> 属性的值。
- 在 `using` 子句中实例化 <xref:System.ServiceModel.OperationContextScope> 对象。
- 在 `using statement` 中检索 <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> 属性的值。 例如：

```csharp
using (new OperationContextScope(OperationContext.Current))
{
    // OperationContext.Current is null.
    OperationContext context = OperationContext.Current;

    // ...
}
```

#### <a name="suggestion"></a>建议

要解决此问题，可执行以下操作：

- 按如下所示修改代码，实例化一个新的非 `null` <xref:System.ServiceModel.OperationContext.Current%2A> 对象：

    ```csharp
    OperationContext ocx = OperationContext.Current;
    using (new OperationContextScope(OperationContext.Current))
    {
        OperationContext.Current = new OperationContext(ocx.Channel);

        // ...
    }
    ```

- 安装 .NET Framework 4.6.2 的最新更新，或升级到更高版本的 .NET Framework。 这将禁用 <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> 中的 <xref:System.Threading.ExecutionContext> 流并还原 .NET Framework 4.6.1 以及更低版本中的 WCF 应用程序行为。 此行为是可配置的；它相当于将以下应用设置添加到配置文件中：

    ```xml
    <appSettings>
      <add key="Switch.System.ServiceModel.DisableOperationContextAsyncFlow" value="true" />
    </appSettings>
    ```

    如果不需要此更改并且应用程序依赖操作上下文间的执行上下文流，那么可以启用它的流，如下所示：

    ```xml
    <appSettings>
      <add key="Switch.System.ServiceModel.DisableOperationContextAsyncFlow" value="false" />
    </appSettings>
    ```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType>
