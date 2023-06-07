---
title: 封送类、结构和联合
description: 查看如何封送类、结构和联合。 查看封送类的示例、具有嵌套结构的结构、结构的数组和联合。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data marshaling, classes
- marshaling, unions
- marshaling, structures
- marshaling, samples
- data marshaling, structures
- platform invoke, marshaling data
- marshaling, classes
- data marshaling, unions
- data marshaling, samples
- data marshaling, platform invoke
- marshaling, platform invoke
ms.assetid: 027832a2-9b43-4fd9-9b45-7f4196261a4e
ms.openlocfilehash: 25de633faabb1424bcf5e618cc5ca129e61c5fca
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90547865"
---
# <a name="marshaling-classes-structures-and-unions"></a>封送类、结构和联合

.NET Framework 中类和结构非常相似。 它们都可以具有字段、属性和事件。 并且都可以具有静态和非静态方法。 一个显著区别是结构是值类型，而类是引用类型。

下表列出了类、结构和联合的封送处理选项；描述了它们的用法；并提供了到相应平台调用示例的链接。

|类型|描述|示例|
|----------|-----------------|------------|
|按值传递类。|将具有整数成员的类传递为 In/Out 参数，与托管的情形相似。|[SysTime 示例](#systime-sample)|
|按值传递结构。|将结构作为 In 参数传递。|[结构示例](#structures-sample)|
|按引用传递结构。|将结构作为 In/Out 参数传递。|[OSInfo 示例](/previous-versions/dotnet/netframework-4.0/795sy883(v=vs.100))|
|具有内嵌结构（平展）的结构。|传递非托管函数中表示内嵌结构的结构的类。 此结构在托管的原型中将平展为一个大的结构。|[FindFile 示例](#findfile-sample)|
|具有指向另一结构的指针的结构。|将包含指向第二结构的指针的结构作为成员传递。|[结构示例](#structures-sample)|
|按值传递具有整数的结构数组。|将仅包含整数的结构数组作为 In/Out 参数进行传递。 可以更改数组的成员。|[数组示例](marshaling-different-types-of-arrays.md)|
|按引用传递具有整数和字符串的结构数组。|将包含整数和字符串的结构数组作为 Out 参数传递。 被调用的函数为数组分配内存。|[OutArrayOfStructs 示例](#outarrayofstructs-sample)|
|具有值类型的联合。|传递具有值类型（整数和双精度）的联合。|[联合示例](#unions-sample)|
|具有混合类型的联合。|传递具有混合类型（整数和字符串）的联合。|[联合示例](#unions-sample)|
|具有特定于平台的布局的结构。|使用本机打包定义传递类型。|[平台示例](#platform-sample)|
|结构中的 null 值。|传递空引用（Visual Basic 中为 Nothing），而不传递对值类型的引用。|[HandleRef 示例](/previous-versions/dotnet/netframework-3.0/hc662t8k(v=vs.85))|

## <a name="structures-sample"></a>结构示例

此示例演示了如何传递指向第二结构的结构、具有嵌入结构的结构和具有嵌入数组的结构。  
  
“结构”示例使用以下非托管函数（与原始函数声明一同显示）：

- 从 PinvokeLib.dll 导出的 TestStructInStruct。

    ```cpp
    int TestStructInStruct(MYPERSON2* pPerson2);
    ```

- 从 PinvokeLib.dll 导出的 TestStructInStruct3。

    ```cpp
    void TestStructInStruct3(MYPERSON3 person3);
    ```

- 从 PinvokeLib.dll 导出的 TestArrayInStruct。

    ```cpp
    void TestArrayInStruct(MYARRAYSTRUCT* pStruct);
    ```

[PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) 是一种自定义的非托管库，包含上述函数和 4 种结构（MYPERSON、MYPERSON2、MYPERSON3 和 MYARRAYSTRUCT   ）的实现。 这些结构包含以下元素：

```cpp
typedef struct _MYPERSON
{
   char* first;
   char* last;
} MYPERSON, *LP_MYPERSON;

typedef struct _MYPERSON2
{
   MYPERSON* person;
   int age;
} MYPERSON2, *LP_MYPERSON2;

typedef struct _MYPERSON3
{
   MYPERSON person;
   int age;
} MYPERSON3;

typedef struct _MYARRAYSTRUCT
{
   bool flag;
   int vals[ 3 ];
} MYARRAYSTRUCT;
```

托管的 `MyPerson`、`MyPerson2`、`MyPerson3` 和 `MyArrayStruct` 结构具有以下特征：

- `MyPerson` 仅包含字符串成员。 [CharSet](specifying-a-character-set.md) 字段在传递到非托管函数时将字符串设置为 ANSI 格式。

- `MyPerson2` 将 IntPtr 包含到 `MyPerson` 结构中。 IntPtr 类型替换指向非托管结构的原始指针，因为 .NET Framework 应用程序不使用指针，除非代码被标记为“不安全” 。

- `MyPerson3` 将 `MyPerson` 作为嵌入结构包含在内。 嵌入其他结构的结构可通过将嵌入结构的元素直接放入主结构中来进行平展，还可以保留为嵌入结构，如本示例中操作所示。

- `MyArrayStruct` 包含整数数组。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性将 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举值设置为 ByValArray，此值用于指示数组中的元素数。

对于此示例中的所有结构，应用 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性以确保成员在内存中按出现的顺序进行排列。

`NativeMethods` 类包含 `App` 类所调用的 `TestStructInStruct`、`TestStructInStruct3` 和 `TestArrayInStruct` 方法的托管原型。 每个原型均声明一个参数，如下所示：

- `TestStructInStruct` 将对 `MyPerson2` 类型的引用声明为其参数。

- `TestStructInStruct3` 将 `MyPerson3` 类型声明为其参数并按值传递此参数。

- `TestArrayInStruct` 将对 `MyArrayStruct` 类型的引用声明为其参数。

作为方法自变量的结构按值传递，除非此参数包含 ref（Visual Basic 中为 ByRef）关键字 。 例如，`TestStructInStruct` 方法将对 `MyPerson2` 类型对象的引用（地址的值）传递到非托管代码。 为了处理 `MyPerson2` 指向的结构，此示例创建了具有指定大小的缓冲区，并同时使用 <xref:System.Runtime.InteropServices.Marshal.AllocCoTaskMem%2A?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.Marshal.SizeOf%2A?displayProperty=nameWithType> 方法返回其地址。 然后，此示例将该托管结构的内容复制到非托管的缓冲区。 最后，此示例使用 <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%2A?displayProperty=nameWithType> 方法将非托管缓冲区的数据封送到托管对象，并使用 <xref:System.Runtime.InteropServices.Marshal.FreeCoTaskMem%2A?displayProperty=nameWithType> 方法释放非托管的内存块。

### <a name="declaring-prototypes"></a>声明原型

[!code-cpp[Conceptual.Interop.Marshaling#23](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/structures.cpp#23)]
[!code-csharp[Conceptual.Interop.Marshaling#23](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/structures.cs#23)]
[!code-vb[Conceptual.Interop.Marshaling#23](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/structures.vb#23)]

### <a name="calling-functions"></a>调用函数

[!code-cpp[Conceptual.Interop.Marshaling#24](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/structures.cpp#24)]
[!code-csharp[Conceptual.Interop.Marshaling#24](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/structures.cs#24)]
[!code-vb[Conceptual.Interop.Marshaling#24](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/structures.vb#24)]

## <a name="findfile-sample"></a>FindFile 示例

此示例演示了如何将包含第二、嵌入结构的结构传递到非托管函数。 它还演示了如何使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性在结构中声明固定长度的数组。 在此示例中，嵌入的结构元素将添加到父结构。 有关未平展的嵌入结构的示例，请参阅[结构示例](/previous-versions/dotnet/netframework-4.0/eadtsekz(v=vs.100))。

FindFile 示例使用以下的非托管函数（与其原始函数声明一同显示）：

- 从 Kernel32.dll 导出的 FindFirstFile。

    ```cpp
    HANDLE FindFirstFile(LPCTSTR lpFileName, LPWIN32_FIND_DATA lpFindFileData);
    ```

传递至函数的原始结构包含以下元素：

```cpp
typedef struct _WIN32_FIND_DATA
{
  DWORD    dwFileAttributes;
  FILETIME ftCreationTime;
  FILETIME ftLastAccessTime;
  FILETIME ftLastWriteTime;
  DWORD    nFileSizeHigh;
  DWORD    nFileSizeLow;
  DWORD    dwReserved0;
  DWORD    dwReserved1;
  TCHAR    cFileName[ MAX_PATH ];
  TCHAR    cAlternateFileName[ 14 ];
} WIN32_FIND_DATA, *PWIN32_FIND_DATA;
```

在此示例中，`FindData` 类包含原始结构和嵌入结构中每个元素的对应数据成员。 如果存在 2 个原始字符缓冲区，类将替换字符串。 MarshalAsAttribute 将 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举设置为 ByValTStr，它用于标识非托管结构中出现的定长内联字符数组 。

`NativeMethods` 类包含 `FindFirstFile` 方法的托管原型，此方法将 `FindData` 类作为参数传递。 此参数必须使用 <xref:System.Runtime.InteropServices.InAttribute> 和 <xref:System.Runtime.InteropServices.OutAttribute> 属性进行声明，因为作为引用类型的类默认传递为 In 参数。

### <a name="declaring-prototypes"></a>声明原型

[!code-cpp[Conceptual.Interop.Marshaling#17](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/findfile.cpp#17)]
[!code-csharp[Conceptual.Interop.Marshaling#17](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/findfile.cs#17)]
[!code-vb[Conceptual.Interop.Marshaling#17](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/findfile.vb#17)]

### <a name="calling-functions"></a>调用函数

[!code-cpp[Conceptual.Interop.Marshaling#18](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/findfile.cpp#18)]
[!code-csharp[Conceptual.Interop.Marshaling#18](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/findfile.cs#18)]
 [!code-vb[Conceptual.Interop.Marshaling#18](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/findfile.vb#18)]

## <a name="unions-sample"></a>“联合”示例

此示例演示了如何将仅包含值类型的结构以及包含值类型和字符串的结构作为参数传递至需要联合的非托管函数。 联合是指可由两个或多个变量共享的内存位置。

“联合”示例使用以下非托管函数（与其原始函数声明一同显示）：

- 从 PinvokeLib.dll 导出的 TestUnion。

    ```cpp
    void TestUnion(MYUNION u, int type);
    ```

[PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) 是一种自定义的非托管库，包含上述函数和两个联合（MYUNION 和 MYUNION2）的实现。 联合包含以下元素：

```cpp
union MYUNION
{
    int number;
    double d;
}

union MYUNION2
{
    int i;
    char str[128];
};
```

在托管代码中，将联合定义为结构。 `MyUnion` 结构包含两个值类型（整数和双精度值），将其作为成员。 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性设置为控制每个数据成员的精确位置。 <xref:System.Runtime.InteropServices.FieldOffsetAttribute> 属性提供联合的非托管表示形式中字段的物理位置。 请注意，这两个成员具有相同的偏移值，因此成员可以定义相同的内存块数。

`MyUnion2_1` 和 `MyUnion2_2` 分别包含值类型（整数）和字符串。 在托管代码中，值类型和引用类型不允许重叠。 此示例使用方法重载以使调用方在调用同一个非托管函数时能够使用这两种类型。 `MyUnion2_1` 的布局是显式的且具有准确的偏移值。 与此相反，`MyUnion2_2` 的布局是按顺序的，因为不允许引用类型使用显式布局。 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 属性将 <xref:System.Runtime.InteropServices.UnmanagedType> 枚举设置为 ByValTStr，它用于标识在联合的非托管表示形式中出现的定长内联字符数组。

`NativeMethods` 类包含 `TestUnion` 和 `TestUnion2` 方法的原型。 已重载 `TestUnion2` 以将 `MyUnion2_1` 或 `MyUnion2_2` 声明为参数。

### <a name="declaring-prototypes"></a>声明原型

[!code-cpp[Conceptual.Interop.Marshaling#28](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/unions.cpp#28)]
[!code-csharp[Conceptual.Interop.Marshaling#28](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/unions.cs#28)]
[!code-vb[Conceptual.Interop.Marshaling#28](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/unions.vb#28)]

### <a name="calling-functions"></a>调用函数

[!code-cpp[Conceptual.Interop.Marshaling#29](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/unions.cpp#29)]
[!code-csharp[Conceptual.Interop.Marshaling#29](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/unions.cs#29)]
[!code-vb[Conceptual.Interop.Marshaling#29](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/unions.vb#29)]

## <a name="platform-sample"></a>平台示例

在某些情况下，`struct` 和 `union` 布局会因目标平台而异。 例如，在 COM 场景中定义时，请考虑 [`STRRET`](/windows/win32/api/shtypes/ns-shtypes-strret) 类型：

```c++
#include <pshpack8.h> /* Defines the packing of the struct */
typedef struct _STRRET
    {
    UINT uType;
    /* [switch_is][switch_type] */ union
        {
        /* [case()][string] */ LPWSTR pOleStr;
        /* [case()] */ UINT uOffset;
        /* [case()] */ char cStr[ 260 ];
        }  DUMMYUNIONNAME;
    }  STRRET;
#include <poppack.h>
```

上述 `struct` 是通过影响类型的内存布局的 Windows 标头声明的。 在托管环境中定义时，需要使用这些布局详细信息来正确地与本机代码互操作。

在 32 位进程中，此类型的正确托管定义为：

``` CSharp
[StructLayout(LayoutKind.Explicit, Size = 264)]
public struct STRRET_32
{
    [FieldOffset(0)]
    public uint uType;

    [FieldOffset(4)]
    public IntPtr pOleStr;

    [FieldOffset(4)]
    public uint uOffset;

    [FieldOffset(4)]
    public IntPtr cStr;
}
```

在 64 位进程中，大小和字段偏移量不同。 正确布局是：

``` CSharp
[StructLayout(LayoutKind.Explicit, Size = 272)]
public struct STRRET_64
{
    [FieldOffset(0)]
    public uint uType;

    [FieldOffset(8)]
    public IntPtr pOleStr;

    [FieldOffset(8)]
    public uint uOffset;

    [FieldOffset(8)]
    public IntPtr cStr;
}
```

如果未能在互操作场景中正确考虑本机布局，会导致随机故障或更糟糕的情况 - 计算不正确。

默认情况下，.NET 程序集可以在 32 位和 64 位版本的 .NET 运行时中运行。 应用必须等待运行时，以确定要使用的前几个定义。

下面的代码片段演示了如何在运行时，在 32 位和 64 位定义之间进行选择的示例。

```csharp
if (IntPtr.Size == 8)
{
    // Use the STRRET_64 definition
}
else
{
    Debug.Assert(IntPtr.Size == 4);
    // Use the STRRET_32 definition
}
```

## <a name="systime-sample"></a>SysTime 示例

此示例演示了如何将指向类的指针传递到需要指向结构的指针的非托管函数。

SysTime 示例使用以下非托管函数（与其其原始函数声明一同显示）：

- 从 Kernel32.dll 导出的 GetSystemTime。

    ```cpp
    VOID GetSystemTime(LPSYSTEMTIME lpSystemTime);
    ```

传递至函数的原始结构包含以下元素：

```cpp
typedef struct _SYSTEMTIME {
    WORD wYear;
    WORD wMonth;
    WORD wDayOfWeek;
    WORD wDay;
    WORD wHour;
    WORD wMinute;
    WORD wSecond;
    WORD wMilliseconds;
} SYSTEMTIME, *PSYSTEMTIME;
```

在此示例中，`SystemTime` 类包含原始结构中表示为类成员的元素。 设置 <xref:System.Runtime.InteropServices.StructLayoutAttribute> 特性，以确保成员在内存中按照它们出现的顺序进行排列。

`NativeMethods` 类包含 `GetSystemTime` 方法的托管原型，此方法在默认情况下将 `SystemTime` 类作为 In/Out 参数传递。 此参数必须使用 <xref:System.Runtime.InteropServices.InAttribute> 和 <xref:System.Runtime.InteropServices.OutAttribute> 属性进行声明，因为作为引用类型的类默认传递为 In 参数。 为使调用方接收结果，这些[方向特性](/previous-versions/dotnet/netframework-4.0/77e6taeh(v=vs.100))必须显式应用。 `App` 类创建 `SystemTime` 类的新实例，并访问它的数据字段。

### <a name="code-samples"></a>代码示例

[!code-cpp[Conceptual.Interop.Marshaling#25](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/systime.cpp#25)]
[!code-csharp[Conceptual.Interop.Marshaling#25](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/systime.cs#25)]
[!code-vb[Conceptual.Interop.Marshaling#25](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/systime.vb#25)]

## <a name="outarrayofstructs-sample"></a>OutArrayOfStructs 示例

此示例显示如何将包含整数和字符串的结构数组作为 Out 参数传递到非托管函数。

此示例演示如何通过使用 <xref:System.Runtime.InteropServices.Marshal> 类和使用不安全代码调用本机函数。

此示例使用 [PinvokeLib.dll](marshaling-data-with-platform-invoke.md#pinvokelibdll) 中定义（且位于源文件）的包装器函数和平台调用。 它使用 `TestOutArrayOfStructs` 函数和 `MYSTRSTRUCT2` 结构。 结构包含以下元素：

```cpp
typedef struct _MYSTRSTRUCT2
{
   char* buffer;
   UINT size;
} MYSTRSTRUCT2;
```

`MyStruct` 类包含 ANSI 字符的字符串对象。 <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet> 字段指定 ANSI 格式。 `MyUnsafeStruct` 是一种包含 <xref:System.IntPtr> 类型（而非字符串）的结构。

`NativeMethods` 类包含已重载 `TestOutArrayOfStructs` 原型方法。 如果方法将指针声明为参数，则应使用 `unsafe` 关键字标记类。 因为 Visual Basic 无法使用不安全的代码，所以重载的方法、不安全的修饰符和 `MyUnsafeStruct` 结构不是必需的。

`App` 类实现 `UsingMarshaling` 方法，此方法执行传递数组所需的全部任务。 使用 `out`（Visual Basic 中的 `ByRef`）关键字标记数组，以指示数据从被调用方传递至调用方。 实现使用以下 <xref:System.Runtime.InteropServices.Marshal> 类方法：

- <xref:System.Runtime.InteropServices.Marshal.PtrToStructure%2A>，用于将数据从非托管缓冲区封送到托管对象。

- <xref:System.Runtime.InteropServices.Marshal.DestroyStructure%2A>用于释放为结构中的字符串保留的内存。

- <xref:System.Runtime.InteropServices.Marshal.FreeCoTaskMem%2A>用于释放为数组保留的内存。

正如上文所述，C# 允许使用不安全的代码，而 Visual Basic 不允许。 在 C# 示例中，`UsingUnsafePointer` 是一种替代性方法实现，使用指针（而非 <xref:System.Runtime.InteropServices.Marshal> 类）传递会包含 `MyUnsafeStruct` 结构的数组。

### <a name="declaring-prototypes"></a>声明原型

[!code-cpp[Conceptual.Interop.Marshaling#20](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/outarrayofstructs.cpp#20)]
[!code-csharp[Conceptual.Interop.Marshaling#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/outarrayofstructs.cs#20)]
[!code-vb[Conceptual.Interop.Marshaling#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/outarrayofstructs.vb#20)]

### <a name="calling-functions"></a>调用函数

[!code-cpp[Conceptual.Interop.Marshaling#21](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.interop.marshaling/cpp/outarrayofstructs.cpp#21)]
[!code-csharp[Conceptual.Interop.Marshaling#21](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.interop.marshaling/cs/outarrayofstructs.cs#21)]
[!code-vb[Conceptual.Interop.Marshaling#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.interop.marshaling/vb/outarrayofstructs.vb#21)]

## <a name="see-also"></a>请参阅

- [用平台调用封送数据](marshaling-data-with-platform-invoke.md)
- [封送处理字符串](marshaling-strings.md)
- [封送处理不同类型的数组](marshaling-different-types-of-arrays.md)
