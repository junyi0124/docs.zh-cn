---
title: 在集合中执行不区分区域性的字符串操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CaseInsensitiveComparer class, using
- CollectionsUtil.CreateCaseInsensitiveHashtable method
- culture-insensitive string operations, collections
- collections [.NET], culture-insensitive string operations
- CaseInsensitiveHashCodeProvider class, using
- ArrayList.Sort method
- SortedList class, culture-insensitive string operations
- culture parameter
ms.assetid: 5cdc9396-a64b-4615-a1cd-b605db4c5983
ms.openlocfilehash: f0d932dcbb07253e3ea52238a81b416af148cf98
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829773"
---
# <a name="performing-culture-insensitive-string-operations-in-collections"></a>在集合中执行不区分区域性的字符串操作

<xref:System.Collections> 命名空间中包含默认提供区域性敏感型行为的类和成员。 <xref:System.Collections.CaseInsensitiveComparer> 和 <xref:System.Collections.CaseInsensitiveHashCodeProvider> 类的无参数构造函数使用 <xref:System.Threading.Thread.CurrentCulture%2A?displayProperty=nameWithType> 属性初始化新实例。 默认情况下，<xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType> 方法的所有重载都会使用 `Thread.CurrentCulture` 属性新建 <xref:System.Collections.Hashtable> 类的实例。 默认情况下，<xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType> 方法重载使用 `Thread.CurrentCulture` 执行区域性敏感型排序。 将字符串用作键时，<xref:System.Collections.SortedList> 中的排序和查找可能会受 `Thread.CurrentCulture` 影响。 请按本节提供的用法建议，在 `Collections` 命名空间中的这些类和方法中获取不区分区域性的结果。

> [!NOTE]
> 向比较方法传递 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 确实会执行非区域性敏感型比较。 但是，这不会导致对文件路径、注册表项、环境变量等进行非语义比较。 也不支持基于比较结果的安全决策。 若要进行非语义比较或支持基于结果的安全决策，应用应使用接受 <xref:System.StringComparison> 值的比较方法。 然后，应用应传递 <xref:System.StringComparison>。

## <a name="using-the-caseinsensitivecomparer-and-caseinsensitivehashcodeprovider-classes"></a>使用 CaseInsensitiveComparer 和 CaseInsensitiveHashCodeProvider 类

`CaseInsensitiveHashCodeProvider` 和 `CaseInsensitiveComparer` 的无参数构造函数使用 `Thread.CurrentCulture` 来初始化类的新实例，产生区域性敏感行为。 下面的代码示例演示区域性敏感 `Hashtable` 的构造函数，因为该构造函数使用 `CaseInsensitiveHashCodeProvider` 和 `CaseInsensitiveComparer` 的无参数构造函数。

```vb
internalHashtable = New Hashtable(CaseInsensitiveHashCodeProvider.Default, CaseInsensitiveComparer.Default)
```

```csharp
internalHashtable = new Hashtable(CaseInsensitiveHashCodeProvider.Default, CaseInsensitiveComparer.Default);
```

若要使用 `CaseInsensitiveComparer` 和 `CaseInsensitiveHashCodeProvider` 类创建非区域性敏感型 `Hashtable`，请使用接受 `culture` 参数的构造函数，初始化这些类的新实例。 对于 `culture` 参数，请指定 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>。 下面的代码示例演示不区分区域性的 `Hashtable` 的构造函数。

```vb
internalHashtable = New Hashtable(New
    CaseInsensitiveHashCodeProvider(CultureInfo.InvariantCulture),
    New CaseInsensitiveComparer(CultureInfo.InvariantCulture))
```

```csharp
internalHashtable = new Hashtable(new CaseInsensitiveHashCodeProvider
    (CultureInfo.InvariantCulture),
    new CaseInsensitiveComparer(CultureInfo.InvariantCulture));
```

## <a name="using-the-collectionsutilcreatecaseinsensitivehashtable-method"></a>使用 CollectionsUtil.CreateCaseInsensitiveHashTable 方法

若要为 `Hashtable` 类创建一个忽略字符串大小写的新实例，`CollectionsUtil.CreateCaseInsensitiveHashTable` 方法是一种十分有用的快捷方式。 但是，`CollectionsUtil.CreateCaseInsensitiveHashTable` 方法的所有重载都区分区域性，因为这些重载使用 `Thread.CurrentCulture` 属性。 不能使用此方法创建不区分区域性的 `Hashtable`。 若要创建不区分区域性的 `Hashtable`，请使用接受 `culture` 参数的 `Hashtable` 构造函数。 对于 `culture` 参数，请指定 `CultureInfo.InvariantCulture`。 下面的代码示例演示不区分区域性的 `Hashtable` 的构造函数。

```vb
internalHashtable = New Hashtable(New
    CaseInsensitiveHashCodeProvider(CultureInfo.InvariantCulture),
    New CaseInsensitiveComparer(CultureInfo.InvariantCulture))
```

```csharp
internalHashtable = new Hashtable(new CaseInsensitiveHashCodeProvider
    (CultureInfo.InvariantCulture),
    new CaseInsensitiveComparer(CultureInfo.InvariantCulture));
```

<a name="cpconperformingculture-insensitivestringoperationsincollectionsanchor1"></a>

## <a name="using-the-sortedlist-class"></a>使用 SortedList 类

`SortedList` 表示键值对的集合，这些键值对按键排序，并可按照键和索引进行访问。 在使用以字符串作为键的 `SortedList` 时，排序和查找会受 `Thread.CurrentCulture` 属性的影响。 若要从 `SortedList` 获取不区分区域性的行为，请使用一个接受 `comparer` 参数的构造函数来创建 `SortedList`。 `comparer` 参数指定要在比较键时使用的 <xref:System.Collections.IComparer> 实现。 对于该参数，请指定使用 `CultureInfo.InvariantCulture` 来比较键的自定义比较器类。 下面的示例说明一个不区分区域性的自定义比较器类，可将该比较器类指定为 `SortedList` 构造函数的 `comparer` 参数。

```vb
Imports System.Collections
Imports System.Globalization

Friend Class InvariantComparer
    Implements IComparer
    Private m_compareInfo As CompareInfo
    Friend Shared [Default] As New InvariantComparer()

    Friend Sub New()
        m_compareInfo = CultureInfo.InvariantCulture.CompareInfo
    End Sub

    Public Function Compare(a As Object, b As Object) As Integer _
            Implements IComparer.Compare
        Dim sa As String = CType(a, String)
        Dim sb As String = CType(b, String)
        If Not (sa Is Nothing) And Not (sb Is Nothing) Then
            Return m_compareInfo.Compare(sa, sb)
        Else
            Return Comparer.Default.Compare(a, b)
        End If
    End Function
End Class
```

```csharp
using System;
using System.Collections;
using System.Globalization;

internal class InvariantComparer : IComparer
{
    private CompareInfo m_compareInfo;
    internal static readonly InvariantComparer Default = new
        InvariantComparer();

    internal InvariantComparer()
    {
        m_compareInfo = CultureInfo.InvariantCulture.CompareInfo;
    }

    public int Compare(Object a, Object b)
    {
        String sa = a as String;
        String sb = b as String;
        if (sa != null && sb != null)
            return m_compareInfo.Compare(sa, sb);
        else
            return Comparer.Default.Compare(a,b);
    }
}
```

一般而言，如果对字符串使用 `SortedList` 而不指定自定义固定比较器，填充列表后，更改 `Thread.CurrentCulture` 会使列表失效。

## <a name="using-the-arraylistsort-method"></a>使用 ArrayList.Sort 方法

默认情况下，`ArrayList.Sort` 方法的重载使用 `Thread.CurrentCulture` 属性来执行区分区域性的排序。 由于排序顺序不同，结果可能会因区域性而异。 若要消除区分区域性的行为，请使用接受 `IComparer` 实现的此方法的重载。 对于 `comparer` 参数，请指定使用 `CultureInfo.InvariantCulture` 的自定义固定比较器类。 [使用 SortedList 类](#cpconperformingculture-insensitivestringoperationsincollectionsanchor1)主题中提供了自定义固定比较器类的示例。

## <a name="see-also"></a>另请参阅

- <xref:System.Collections.CaseInsensitiveComparer>
- <xref:System.Collections.CaseInsensitiveHashCodeProvider>
- <xref:System.Collections.ArrayList.Sort%2A?displayProperty=nameWithType>
- <xref:System.Collections.SortedList>
- <xref:System.Collections.Hashtable>
- <xref:System.Collections.IComparer>
- [执行不区分区域性的字符串操作](performing-culture-insensitive-string-operations.md)
- <xref:System.Collections.Specialized.CollectionsUtil.CreateCaseInsensitiveHashtable%2A?displayProperty=nameWithType>
