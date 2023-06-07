---
title: 了解何时使用 Override 和 New 关键字 - C# 编程指南
description: 使用 C# 中的 new 和 override 关键字指定具有相同名称的基类和派生类中的方法的交互方式。
ms.date: 07/20/2015
helpviewer_keywords:
- override keyword [C#]
- new keyword [C#]
- polymorphism [C#], using override and new [C#]
ms.assetid: 323db184-b136-46fc-8839-007886e7e8b0
ms.openlocfilehash: 732a37c08b4c7bb998a7cc5dcfbd00d4e706b06c
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864535"
---
# <a name="knowing-when-to-use-override-and-new-keywords-c-programming-guide"></a>了解何时使用 Override 和 New 关键字（C# 编程指南）

在 C# 中，派生类中的方法可具有与基类中的方法相同的名称。 可使用 [new](../../language-reference/keywords/new-modifier.md) 和 [override](../../language-reference/keywords/override.md) 关键字指定方法的交互方式。 `override` 修饰符用于扩展基类 `virtual` 方法，而 `new` 修饰符用于隐藏可访问的基类方法 。 本主题中的示例阐释了这种差异。  
  
 在控制台应用程序中，声明以下两个类：`BaseClass` 和 `DerivedClass`。 `DerivedClass` 继承自 `BaseClass`。  
  
```csharp  
class BaseClass  
{  
    public void Method1()  
    {  
        Console.WriteLine("Base - Method1");  
    }  
}  
  
class DerivedClass : BaseClass  
{  
    public void Method2()  
    {  
        Console.WriteLine("Derived - Method2");  
    }  
}  
```  
  
 在 `Main` 方法中，声明变量 `bc`、`dc` 和 `bcdc`。  
  
- `bc` 为 `BaseClass` 类型，其值为 `BaseClass` 类型。  
  
- `dc` 为 `DerivedClass` 类型，其值为 `DerivedClass` 类型。  
  
- `bcdc` 为 `BaseClass` 类型，其值为 `DerivedClass` 类型。 需注意此变量。  
  
 由于 `bc` 和 `bcdc` 具有 `BaseClass` 类型，因此它们只能直接访问 `Method1`，除非使用强制转换。 变量 `dc` 可同时访问 `Method1` 和 `Method2`。 下面的代码演示了这些关系。  
  
```csharp  
class Program  
{  
    static void Main(string[] args)  
    {  
        BaseClass bc = new BaseClass();  
        DerivedClass dc = new DerivedClass();  
        BaseClass bcdc = new DerivedClass();  
  
        bc.Method1();  
        dc.Method1();  
        dc.Method2();  
        bcdc.Method1();  
    }  
    // Output:  
    // Base - Method1  
    // Base - Method1  
    // Derived - Method2  
    // Base - Method1  
}  
```  
  
 接着，将以下 `Method2` 方法添加到 `BaseClass`。 此方法的签名与 `DerivedClass` 中 `Method2` 方法的签名匹配。  
  
```csharp  
public void Method2()  
{  
    Console.WriteLine("Base - Method2");  
}  
```  
  
 由于 `BaseClass` 现在具有 `Method2` 方法，因此可以为 `BaseClass` 变量 `bc` 和 `bcdc` 添加第二个调用语句，如下面的代码所示。  
  
```csharp  
bc.Method1();  
bc.Method2();  
dc.Method1();  
dc.Method2();  
bcdc.Method1();  
bcdc.Method2();  
```  
  
 当生成项目时，你将看到在 `BaseClass` 中添加 `Method2` 方法将引发警告。 警告显示 `DerivedClass` 中的 `Method2` 方法隐藏了 `BaseClass` 中的 `Method2` 方法。 如果希望获得该结果，则建议使用 `Method2` 定义中的 `new` 关键字。 或者，可重命名 `Method2` 方法之一来消除警告，但这始终不实用。  
  
 添加 `new` 之前，请运行程序，查看其他调用语句生成的输出。 显示以下结果。  
  
```csharp  
// Output:  
// Base - Method1  
// Base - Method2  
// Base - Method1  
// Derived - Method2  
// Base - Method1  
// Base - Method2  
```  
  
 `new` 关键字可以保留生成该输出的关系，但它会禁止显示警告。 具有 `BaseClass` 类型的变量继续访问 `BaseClass` 的成员，而具有 `DerivedClass` 类型的变量首先继续访问 `DerivedClass` 中的成员，然后再考虑从 `BaseClass` 继承的成员。  
  
 若要禁止显示警告，请将 `new` 修饰符添加到 `DerivedClass` 中的 `Method2` 定义，如下面的代码所示。 可在 `public` 前后添加修饰符。  
  
```csharp  
public new void Method2()  
{  
    Console.WriteLine("Derived - Method2");  
}  
```  
  
 再次运行该程序，确认输出未发生更改。 此外，确认不再显示警告。 通过使用 `new`，断言你知道它修饰的成员将隐藏从基类继承的成员。 有关通过继承隐藏名称的详细信息，请参阅 [new 修饰符](../../language-reference/keywords/new-modifier.md)。  
  
 若要将此行为与使用 `override` 的效果进行对比，请将以下方法添加到 `DerivedClass`。 可在 `public` 前后添加 `override` 修饰符。  
  
```csharp  
public override void Method1()  
{  
    Console.WriteLine("Derived - Method1");  
}  
```  
  
 将 `virtual` 修饰符添加到 `BaseClass` 中的 `Method1` 定义。 可在 `public` 前后添加 `virtual` 修饰符。  
  
```csharp  
public virtual void Method1()  
{  
    Console.WriteLine("Base - Method1");  
}  
```  
  
 再次运行该项目。 尤其注意以下输出的最后两行。  
  
```csharp  
// Output:  
// Base - Method1  
// Base - Method2  
// Derived - Method1  
// Derived - Method2  
// Derived - Method1  
// Base - Method2  
```  
  
 使用 `override` 修饰符可使 `bcdc` 访问 `DerivedClass` 中定义的 `Method1` 方法。 通常，这是继承层次结构中所需的行为。 让具有从派生类创建的值的对象使用派生类中定义的方法。 可使用 `override` 扩展基类方法实现该行为。  
  
 下面的代码包括完整的示例。  
  
```csharp  
using System;  
using System.Text;  
  
namespace OverrideAndNew  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            BaseClass bc = new BaseClass();  
            DerivedClass dc = new DerivedClass();  
            BaseClass bcdc = new DerivedClass();  
  
            // The following two calls do what you would expect. They call  
            // the methods that are defined in BaseClass.  
            bc.Method1();  
            bc.Method2();  
            // Output:  
            // Base - Method1  
            // Base - Method2  
  
            // The following two calls do what you would expect. They call  
            // the methods that are defined in DerivedClass.  
            dc.Method1();  
            dc.Method2();  
            // Output:  
            // Derived - Method1  
            // Derived - Method2  
  
            // The following two calls produce different results, depending
            // on whether override (Method1) or new (Method2) is used.  
            bcdc.Method1();  
            bcdc.Method2();  
            // Output:  
            // Derived - Method1  
            // Base - Method2  
        }  
    }  
  
    class BaseClass  
    {  
        public virtual void Method1()  
        {  
            Console.WriteLine("Base - Method1");  
        }  
  
        public virtual void Method2()  
        {  
            Console.WriteLine("Base - Method2");  
        }  
    }  
  
    class DerivedClass : BaseClass  
    {  
        public override void Method1()  
        {  
            Console.WriteLine("Derived - Method1");  
        }  
  
        public new void Method2()  
        {  
            Console.WriteLine("Derived - Method2");  
        }  
    }  
}  
```  
  
 下列阐释了不同上下文中的类似行为。 该示例定义了三个类：一个名为 `Car` 的基类和两个由其派生的 `ConvertibleCar` 和 `Minivan`。 基类中包含 `DescribeCar` 方法。 该方法给出了对一辆车的基本描述，然后调用 `ShowDetails` 提供其他信息。 这三个类中的每一个类都定义了 `ShowDetails` 方法。 `new` 修饰符用于定义 `ConvertibleCar` 类中的 `ShowDetails`。 `override` 修饰符用于定义 `Minivan` 类中的 `ShowDetails`。  
  
```csharp  
// Define the base class, Car. The class defines two methods,  
// DescribeCar and ShowDetails. DescribeCar calls ShowDetails, and each derived  
// class also defines a ShowDetails method. The example tests which version of  
// ShowDetails is selected, the base class method or the derived class method.  
class Car  
{  
    public void DescribeCar()  
    {  
        System.Console.WriteLine("Four wheels and an engine.");  
        ShowDetails();  
    }  
  
    public virtual void ShowDetails()  
    {  
        System.Console.WriteLine("Standard transportation.");  
    }  
}  
  
// Define the derived classes.  
  
// Class ConvertibleCar uses the new modifier to acknowledge that ShowDetails  
// hides the base class method.  
class ConvertibleCar : Car  
{  
    public new void ShowDetails()  
    {  
        System.Console.WriteLine("A roof that opens up.");  
    }  
}  
  
// Class Minivan uses the override modifier to specify that ShowDetails  
// extends the base class method.  
class Minivan : Car  
{  
    public override void ShowDetails()  
    {  
        System.Console.WriteLine("Carries seven people.");  
    }  
}  
```  
  
 该示例测试被调用的 `ShowDetails` 版本。 下面的方法 `TestCars1` 为每个类声明了一个实例，并在每个实例上调用 `DescribeCar`。  
  
```csharp  
public static void TestCars1()  
{  
    System.Console.WriteLine("\nTestCars1");  
    System.Console.WriteLine("----------");  
  
    Car car1 = new Car();  
    car1.DescribeCar();  
    System.Console.WriteLine("----------");  
  
    // Notice the output from this test case. The new modifier is  
    // used in the definition of ShowDetails in the ConvertibleCar  
    // class.
  
    ConvertibleCar car2 = new ConvertibleCar();  
    car2.DescribeCar();  
    System.Console.WriteLine("----------");  
  
    Minivan car3 = new Minivan();  
    car3.DescribeCar();  
    System.Console.WriteLine("----------");  
}  
```  
  
 `TestCars1` 将生成以下输出。 请特别注意 `car2` 的结果，该结果可能不是你需要的内容。 对象的类型是 `ConvertibleCar`，但 `DescribeCar` 不会访问 `ConvertibleCar` 类中定义的 `ShowDetails` 版本，因为方法已声明包含 `new` 修饰符声明，而不是 `override` 修饰符。 因此，`ConvertibleCar` 对象与 `Car` 对象显示的说明相同。 比较 `car3` 的结果，这是一个 `Minivan` 对象。 在这种情况下，`Minivan` 类中声明的 `ShowDetails` 方法会替代 `Car` 类中声明的 `ShowDetails` 方法，显示的说明描述小型货车。  
  
```csharp  
// TestCars1  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Carries seven people.  
// ----------  
```  
  
 `TestCars2` 创建具有 `Car` 类型的对象列表。 对象的值由 `Car` 类、`ConvertibleCar` 类和 `Minivan` 类实例化所得。 对列表中的每个元素调用 `DescribeCar`。 以下代码显示 `TestCars2` 的定义。  
  
```csharp  
public static void TestCars2()  
{  
    System.Console.WriteLine("\nTestCars2");  
    System.Console.WriteLine("----------");  
  
    var cars = new List<Car> { new Car(), new ConvertibleCar(),
        new Minivan() };  
  
    foreach (var car in cars)  
    {  
        car.DescribeCar();  
        System.Console.WriteLine("----------");  
    }  
}  
```  
  
 显示以下输出。 请注意，它与 `TestCars1` 显示的输出相同。 不调用 `ConvertibleCar` 类的 `ShowDetails` 方法，不管该对象的类型是 `ConvertibleCar`（在 `TestCars1` 中）还是 `Car`（在 `TestCars2` 中）。 相反，在这两种情况下，`car3` 从 `Minivan` 调用 `ShowDetails` 方法，不管它拥有类型 `Minivan` 还是类型 `Car`。  
  
```csharp  
// TestCars2  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Standard transportation.  
// ----------  
// Four wheels and an engine.  
// Carries seven people.  
// ----------  
```  
  
 方法 `TestCars3` 和方法 `TestCars4` 完成示例。 这些方法直接调用 `ShowDetails`，先从声明具有类型 `ConvertibleCar` 和 `Minivan` (`TestCars3`) 的对象开始，然后再转到声明具有类型 `Car` (`TestCars4`) 的对象。 以下代码定义了这两种方法。  
  
```csharp  
public static void TestCars3()  
{  
    System.Console.WriteLine("\nTestCars3");  
    System.Console.WriteLine("----------");  
    ConvertibleCar car2 = new ConvertibleCar();  
    Minivan car3 = new Minivan();  
    car2.ShowDetails();  
    car3.ShowDetails();  
}  
  
public static void TestCars4()  
{  
    System.Console.WriteLine("\nTestCars4");  
    System.Console.WriteLine("----------");  
    Car car2 = new ConvertibleCar();  
    Car car3 = new Minivan();  
    car2.ShowDetails();  
    car3.ShowDetails();  
}  
```  
  
 这两种方法将产生以下输出，输出对应本主题第一个示例的结果。  
  
```csharp  
// TestCars3  
// ----------  
// A roof that opens up.  
// Carries seven people.  
  
// TestCars4  
// ----------  
// Standard transportation.  
// Carries seven people.  
```  
  
 以下代码显示了完整项目及其输出。  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
  
namespace OverrideAndNew2  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            // Declare objects of the derived classes and test which version  
            // of ShowDetails is run, base or derived.  
            TestCars1();  
  
            // Declare objects of the base class, instantiated with the  
            // derived classes, and repeat the tests.  
            TestCars2();  
  
            // Declare objects of the derived classes and call ShowDetails  
            // directly.  
            TestCars3();  
  
            // Declare objects of the base class, instantiated with the  
            // derived classes, and repeat the tests.  
            TestCars4();  
        }  
  
        public static void TestCars1()  
        {  
            System.Console.WriteLine("\nTestCars1");  
            System.Console.WriteLine("----------");  
  
            Car car1 = new Car();  
            car1.DescribeCar();  
            System.Console.WriteLine("----------");  
  
            // Notice the output from this test case. The new modifier is  
            // used in the definition of ShowDetails in the ConvertibleCar  
            // class.
            ConvertibleCar car2 = new ConvertibleCar();  
            car2.DescribeCar();  
            System.Console.WriteLine("----------");  
  
            Minivan car3 = new Minivan();  
            car3.DescribeCar();  
            System.Console.WriteLine("----------");  
        }  
        // Output:  
        // TestCars1  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Carries seven people.  
        // ----------  
  
        public static void TestCars2()  
        {  
            System.Console.WriteLine("\nTestCars2");  
            System.Console.WriteLine("----------");  
  
            var cars = new List<Car> { new Car(), new ConvertibleCar(),
                new Minivan() };  
  
            foreach (var car in cars)  
            {  
                car.DescribeCar();  
                System.Console.WriteLine("----------");  
            }  
        }  
        // Output:  
        // TestCars2  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Standard transportation.  
        // ----------  
        // Four wheels and an engine.  
        // Carries seven people.  
        // ----------  
  
        public static void TestCars3()  
        {  
            System.Console.WriteLine("\nTestCars3");  
            System.Console.WriteLine("----------");  
            ConvertibleCar car2 = new ConvertibleCar();  
            Minivan car3 = new Minivan();  
            car2.ShowDetails();  
            car3.ShowDetails();  
        }  
        // Output:  
        // TestCars3  
        // ----------  
        // A roof that opens up.  
        // Carries seven people.  
  
        public static void TestCars4()  
        {  
            System.Console.WriteLine("\nTestCars4");  
            System.Console.WriteLine("----------");  
            Car car2 = new ConvertibleCar();  
            Car car3 = new Minivan();  
            car2.ShowDetails();  
            car3.ShowDetails();  
        }  
        // Output:  
        // TestCars4  
        // ----------  
        // Standard transportation.  
        // Carries seven people.  
    }  
  
    // Define the base class, Car. The class defines two virtual methods,  
    // DescribeCar and ShowDetails. DescribeCar calls ShowDetails, and each derived  
    // class also defines a ShowDetails method. The example tests which version of  
    // ShowDetails is used, the base class method or the derived class method.  
    class Car  
    {  
        public virtual void DescribeCar()  
        {  
            System.Console.WriteLine("Four wheels and an engine.");  
            ShowDetails();  
        }  
  
        public virtual void ShowDetails()  
        {  
            System.Console.WriteLine("Standard transportation.");  
        }  
    }  
  
    // Define the derived classes.  
  
    // Class ConvertibleCar uses the new modifier to acknowledge that ShowDetails  
    // hides the base class method.  
    class ConvertibleCar : Car  
    {  
        public new void ShowDetails()  
        {  
            System.Console.WriteLine("A roof that opens up.");  
        }  
    }  
  
    // Class Minivan uses the override modifier to specify that ShowDetails  
    // extends the base class method.  
    class Minivan : Car  
    {  
        public override void ShowDetails()  
        {  
            System.Console.WriteLine("Carries seven people.");  
        }  
    }  
  
}  
```  
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类和结构](./index.md)
- [使用 Override 和 New 关键字进行版本控制](./versioning-with-the-override-and-new-keywords.md)
- [base](../../language-reference/keywords/base.md)
- [abstract](../../language-reference/keywords/abstract.md)
