---
ms.openlocfilehash: 0237c848d71150aaea1f9de4f4b16a0cbbc4ab3a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615606"
---
### <a name="new-64-bit-jit-compiler-in-the-net-framework-46"></a>.NET Framework 4.6 中新的 64 位 JIT 编译器

#### <a name="details"></a>详细信息

从 .NET Framework 4.6 开始，新的 64 位 JIT 编译器用于实时编译。 在某些情况下，比起使用 32 位编译器或较旧的 64 位 JIT 编译器运行应用，此举措会引发意外异常或发生其他行为。 此更改不影响 32 位 JIT 编译器。 已知差异如下：

- 在某些情况下，在启用优化的发布版本中，取消装箱操作可能会引发 <xref:System.NullReferenceException>。
- 在某些情况下，在大型方法主体中执行生产代码可能会引发 <xref:System.StackOverflowException>。
- 在某些情况下，传递给方法的结构将视为引用类型，而不是发布版本中的值类型。 此问题的表现形式之一是，集合中各个项的显示顺序出现异常。
- 在某些情况下，如果启用了优化，无法正确比较 <xref:System.UInt16> 值与其高位集。
- 在某些情况下，尤其是在初始化数组值时，通过 <xref:System.Reflection.Emit.OpCodes.Initblk?displayProperty=nameWithType> IL 指令执行的内存初始化可能会使用不正确的值初始化内存。 这可能会导致未经处理的异常抛出或输出不正确。
- 在某些极少数情况下，如果启用了编译器优化，条件位测试可能会返回错误的 <xref:System.Boolean> 值或引发异常。
- 在某些情况下，如果 `if` 语句用于在进入 `try` 块之前和从 `try` 块中退出之前测试条件，且在 `catch` 或 `finally` 块中计算的条件相同，那么新版 64 位 JIT 编译器会在优化代码时从 `catch` 或 `finally` 块中删除 `if` 条件。 因此，`catch` 或 `finally` 块中的 `if` 语句代码会无条件地执行。

#### <a name="suggestion"></a>建议

**已知问题的缓解措施** <br/> 如果遇到上面列出的问题，可以通过执行下列任一操作来解决：

- 升级到 .NET Framework 4.6.2。 .NET Framework 4.6.2 随附的新版 64 位编译器解决了上面列出的所有已知问题。
- 运行 Windows 更新，以确保 Windows 是最新版本。 .NET Framework 4.6 和 4.6.1 服务更新可解决以上问题，取消装箱操作中的 <xref:System.NullReferenceException> 除外。
- 使用旧版 64 位 JIT 编译器进行编译。 请参阅**其他问题的缓解措施**部分，详细了解如何执行此操作。
**其他问题的缓解措施** <br/> 如果遇到的是旧版和新版 64 位 JIT 编译器编译的代码的其他任何行为差异，或是使用新版 64 位 JIT 编译器编译的应用程序的调试和发布版本的其他任何行为差异，可以使用旧版 64 位 JIT 编译器编译应用程序，具体操作如下：

- 对于每个应用程序，可将 [<](~/docs/framework/configure-apps/file-schema/runtime/uselegacyjit-element.md) 元素添加到应用程序配置文件中。 下面的代码禁用新版 64 位 JIT 编译器，改用旧版 64 位 JIT 编译器进行编译。

    ```xml
    <?xml version ="1.0"?>
    <configuration>
      <runtime>
       <useLegacyJit enabled="1" />
      </runtime>
    </configuration>
    ```

- 对于每个用户，可以将名为 `useLegacyJit` 的 `REG_DWORD` 值添加到注册表的 `HKEY_CURRENT_USER\SOFTWARE\Microsoft\.NETFramework` 密钥中。 如果值为 1，可以启用旧版 64 位 JIT 编译器；如果值为 0，可以禁用旧版编译器，启用新版 64 位 JIT 编译器。
- 对于每台计算机，可以将名为 `useLegacyJit` 的 `REG_DWORD` 值添加到注册表的 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework` 密钥中。 如果值为 `1`，可以启用旧版 64 位 JIT 编译器；如果值为 `0`，可以禁用旧版编译器，启用新版 64 位 JIT 编译器。
还可以在 [Microsoft Connect](https://connect.microsoft.com/VisualStudio) 上报告 bug，告诉我们你遇到的问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6         |
| 类型    | 重定目标 |
