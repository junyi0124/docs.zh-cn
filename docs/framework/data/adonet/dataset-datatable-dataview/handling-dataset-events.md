---
title: 处理数据集事件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 54edefe0-bc38-419b-b486-3d8a0c356f13
ms.openlocfilehash: cc425f3217409a154fd319acb8b1555895cbda54
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183359"
---
# <a name="handling-dataset-events"></a>处理数据集事件

<xref:System.Data.DataSet> 对象提供三个事件： <xref:System.ComponentModel.MarshalByValueComponent.Disposed>、 <xref:System.Data.DataSet.Initialized>和 <xref:System.Data.DataSet.MergeFailed>。  
  
## <a name="the-mergefailed-event"></a>MergeFailed 事件  

 `DataSet` 对象的最常用事件是 `MergeFailed`，当要合并的 `DataSet` 对象的架构发生冲突时，会引发该事件。 当目标和源 <xref:System.Data.DataRow> 有相同的主键值，且 <xref:System.Data.DataSet.EnforceConstraints%2A> 属性设置为 `true`时会发生这种情况。 例如，如果所合并表的主键列与两个 `DataSet` 对象中的表的相同，则将发生异常并引发 `MergeFailed` 事件。 传递给 <xref:System.Data.MergeFailedEventArgs> 事件的 `MergeFailed` 对象具有 <xref:System.Data.MergeFailedEventArgs.Conflict%2A> 属性（标识两个 `DataSet` 对象之间的架构冲突）和 <xref:System.Data.MergeFailedEventArgs.Table%2A> 属性（标识发生冲突的表的名称）。  
  
 下面的代码段演示如何为 `MergeFailed` 事件添加事件处理程序。  
  
```vb  
AddHandler workDS.MergeFailed, New MergeFailedEventHandler( _  
  AddressOf DataSetMergeFailed)  
  
Private Shared Sub DataSetMergeFailed(  _  
  sender As Object,args As MergeFailedEventArgs)  
  Console.WriteLine("Merge failed for table " & args.Table.TableName)  
  Console.WriteLine("Conflict = " & args.Conflict)  
End Sub  
```  
  
```csharp  
workDS.MergeFailed += new MergeFailedEventHandler(DataSetMergeFailed);  
  
private static void DataSetMergeFailed(  
  object sender, MergeFailedEventArgs args)  
{  
  Console.WriteLine("Merge failed for table " + args.Table.TableName);  
  Console.WriteLine("Conflict = " + args.Conflict);  
}  
```  
  
## <a name="the-initialized-event"></a>初始化事件  

 在 <xref:System.Data.DataSet.Initialized> 构造函数初始化 `DataSet` 的新实例后会发生 `DataSet`事件。  
  
 如果 <xref:System.Data.DataSet.IsInitialized%2A> 已完成初始化， `true` 属性会返回 `DataSet` ；否则，返回 `false`。 <xref:System.Data.DataSet.BeginInit%2A> 方法，它开始初始化 `DataSet`，将 <xref:System.Data.DataSet.IsInitialized%2A> 设置为 `false`。 <xref:System.Data.DataSet.EndInit%2A> 方法（用于结束 `DataSet`的初始化）将它设置为 `true`。 Visual Studio 设计环境使用这些方法初始化 `DataSet` 其他组件使用的。 通常不会在代码中使用这些方法。  
  
## <a name="the-disposed-event"></a>释放事件  

 `DataSet` 派生自 <xref:System.ComponentModel.MarshalByValueComponent> 类，该类可公开 <xref:System.ComponentModel.MarshalByValueComponent.Dispose%2A> 方法和 <xref:System.ComponentModel.MarshalByValueComponent.Disposed> 事件。 <xref:System.ComponentModel.MarshalByValueComponent.Disposed>事件添加事件处理程序以侦听组件上已释放的事件。 <xref:System.ComponentModel.MarshalByValueComponent.Disposed> `DataSet` 如果要在调用方法时执行代码，则可以使用的事件 <xref:System.ComponentModel.MarshalByValueComponent.Dispose%2A> 。 <xref:System.ComponentModel.MarshalByValueComponent.Dispose%2A> 释放由使用的资源 <xref:System.ComponentModel.MarshalByValueComponent> 。  
  
> [!NOTE]
> `DataSet`和 `DataTable` 对象从继承 <xref:System.ComponentModel.MarshalByValueComponent> 并且支持 <xref:System.Runtime.Serialization.ISerializable> 用于远程处理的接口。 这两个对象是唯一可远程处理的 ADO.NET 对象。 有关详细信息，请参阅 [.Net 远程处理](/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))。  
  
 有关使用时的其他可用事件的信息 `DataSet` ，请参阅 [处理 DataTable 事件](handling-datatable-events.md) 和 [处理 DataAdapter 事件](../handling-dataadapter-events.md)。  
  
## <a name="see-also"></a>请参阅

- [数据集、数据表和数据视图](index.md)
- [验证数据](/previous-versions/visualstudio/visual-studio-2013/t3b36awf(v=vs.120))
- [在 ADO.NET 中检索和修改数据](../retrieving-and-modifying-data.md)
- [ADO.NET 概述](../ado-net-overview.md)
