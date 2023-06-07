---
title: 在 Visual Basic 中保持对象
ms.date: 07/20/2015
ms.assetid: f1d0b562-e349-4dce-ab5f-c05108467030
ms.openlocfilehash: 0b2fff171164a29e6066839371fc95ad41b452f1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086460"
---
# <a name="walkthrough-persisting-an-object-in-visual-studio-visual-basic"></a>演练：在 Visual Studio 中暂留对象 (Visual Basic)

虽然可在设计时将对象的属性设置为默认值，但销毁对象时，运行时输入的任何值都将丢失。 可使用序列化在实例之间保持对象的数据，以便可存储值并在下次实例化对象时检索这些值。  
  
> [!NOTE]
> 在 Visual Basic 中，要存储简单数据（如名称或编号），可以使用 `My.Settings` 对象。 有关详细信息，请参阅 [My.Settings 对象](../../../language-reference/objects/my-settings-object.md)。  
  
 本演练将创建一个简单的 `Loan` 对象，并将其值保留在文件中。 当重新创建该对象时，将检索文件中的数据。  
  
> [!IMPORTANT]
> 此示例在文件尚未存在时创建新文件。 如果应用程序必须创建文件，则该应用程序必须对文件夹具有 `Create` 权限。 可使用访问控制列表设置权限。 如果文件已存在，则该应用程序只需要 `Write` 权限（这是较弱的权限）。 如有可能，较安全的做法是在部署过程中创建文件并仅向单个文件授予 `Read` 权限（而不是授予文件夹的“创建”权限）。 此外，较安全的做法是将数据写入用户文件夹，而不是根文件夹或“Program Files”文件夹。  
  
> [!IMPORTANT]
> 此示例将数据存储为二进制格式。 不应将这些格式用于敏感数据，如密码或信用卡信息。  
  
> [!NOTE]
> 显示的对话框和菜单命令可能会与“帮助”中的描述不同，具体取决于你现用的设置或版本。 若要更改设置，请单击 **“工具”** 菜单上的 **“导入和导出设置”** 。 有关详细信息，请参阅[个性化设置 Visual Studio IDE](/visualstudio/ide/personalizing-the-visual-studio-ide)。  
  
## <a name="creating-the-loan-object"></a>创建 Loan 对象  

 第一步是创建 `Loan` 类和使用该类的测试应用程序。  
  
### <a name="to-create-the-loan-class"></a>创建 Loan 类  
  
1. 新建“类库”项目，并将其命名为“LoanClass”。 有关详细信息，请参阅[创建解决方案和项目](/visualstudio/ide/creating-solutions-and-projects)。  
  
2. 在“解决方案资源管理器”**** 中，打开 Class1 文件的快捷菜单，选择“重命名”****。 将文件重命名为 `Loan`，然后按 Enter。 重命名文件也会将类重命名为 `Loan`。  
  
3. 将以下公共成员添加到该类中：  
  
    ```vb  
    Public Class Loan  
        Implements System.ComponentModel.INotifyPropertyChanged  
  
        Public Property LoanAmount As Double  
        Public Property InterestRate As Double  
        Public Property Term As Integer  
  
        Private p_Customer As String  
        Public Property Customer As String  
            Get  
                Return p_Customer  
            End Get  
            Set(ByVal value As String)  
                p_Customer = value  
                RaiseEvent PropertyChanged(Me,  
                  New System.ComponentModel.PropertyChangedEventArgs("Customer"))  
            End Set  
        End Property  
  
        Event PropertyChanged As System.ComponentModel.PropertyChangedEventHandler _  
          Implements System.ComponentModel.INotifyPropertyChanged.PropertyChanged  
  
        Public Sub New(ByVal loanAmount As Double,  
                       ByVal interestRate As Double,  
                       ByVal term As Integer,  
                       ByVal customer As String)  
  
            Me.LoanAmount = loanAmount  
            Me.InterestRate = interestRate  
            Me.Term = term  
            p_Customer = customer  
        End Sub  
    End Class  
    ```  
  
 还需要创建一个使用 `Loan` 类的简单应用程序。  
  
### <a name="to-create-a-test-application"></a>创建测试应用程序  
  
1. 若要将 Windows 窗体应用程序项目添加到解决方案，请在“文件”**** 菜单上依次选择“添加”****、“新建项目”****。  
  
2. 在“添加新项目”**** 对话框中，选择“Windows 窗体应用程序”****，然后输入 `LoanApp` 作为项目名称，然后单击“确定”**** 关闭对话框。  
  
3. 在“解决方案资源管理器”**** 中，选择 LoanApp 项目。  
  
4. 在“项目”**** 菜单上，选择“设为启动项目”****。  
  
5. 在“项目”**** 菜单上，选择“添加引用” ****。  
  
6. 在“添加引用”**** 对话框中，选择“项目”**** 选项卡，然后选择 LoanClass 项目。  
  
7. 单击“确定”  关闭对话框。  
  
8. 在设计器中，向窗体添加四个 <xref:System.Windows.Forms.TextBox> 控件。  
  
9. 在代码编辑器中，添加以下代码：  
  
    ```vb  
    Private WithEvents TestLoan As New LoanClass.Loan(10000.0, 0.075, 36, "Neil Black")  
  
    Private Sub Form1_Load() Handles MyBase.Load  
        TextBox1.Text = TestLoan.LoanAmount.ToString  
        TextBox2.Text = TestLoan.InterestRate.ToString  
        TextBox3.Text = TestLoan.Term.ToString  
        TextBox4.Text = TestLoan.Customer  
    End Sub  
    ```  
  
10. 使用以下代码将 `PropertyChanged` 事件的事件处理程序添加到窗体：  
  
    ```vb  
    Public Sub CustomerPropertyChanged(  
          ByVal sender As Object,  
          ByVal e As System.ComponentModel.PropertyChangedEventArgs  
        ) Handles TestLoan.PropertyChanged  
  
        MsgBox(e.PropertyName & " has been changed.")  
    End Sub  
    ```  
  
 此时可以生成并运行应用程序。 请注意，`Loan` 类中的默认值将显示在文本框中。 尝试将利率值从 7.5 更改到 7.1，然后关闭该应用程序并再次运行 - 值会还原为默认值 7.5。  
  
 在现实生活中，利率会定期更改，但不必在每次运行应用程序时都更改利率。 与其让用户在每次运行应用程序时更新利率，不如在应用程序的实例之间保留最近的利率。 下一步是通过向 Loan 类添加序列化来执行此操作。  
  
## <a name="using-serialization-to-persist-the-object"></a>使用序列化保持对象  

 为了保持 Loan 类的值，必须首先使用 `Serializable` 属性标记该类。  
  
### <a name="to-mark-a-class-as-serializable"></a>将类标记为可序列化  
  
- 更改 Loan 类的类声明，如下所示：  
  
    ```vb  
    <Serializable()>  
    Public Class Loan  
    ```  
  
 `Serializable` 属性通知编译器可将类中的所有内容保持到文件中。 `PropertyChanged` 事件由 Windows 窗体对象处理，因此不能将其序列化。 可使用 `NonSerialized` 属性标记不应保持的类成员。  
  
### <a name="to-prevent-a-member-from-being-serialized"></a>阻止对成员进行序列化  
  
- 更改 `PropertyChanged` 事件的声明，如下所示：  
  
    ```vb  
    <NonSerialized()>  
    Event PropertyChanged As System.ComponentModel.PropertyChangedEventHandler _  
      Implements System.ComponentModel.INotifyPropertyChanged.PropertyChanged  
    ```  
  
 下一步是向 LoanApp 应用程序添加序列化代码。 为了将该类序列化并将其写入到文件，将使用 <xref:System.IO> 和 <xref:System.Xml.Serialization> 命名空间。 为了避免键入完全限定的名称，可以添加对必要类库的引用。  
  
### <a name="to-add-references-to-namespaces"></a>添加对命名空间的引用  
  
- 将下面的语句添加到 `Form1` 类的顶部：  
  
    ```vb  
    Imports System.IO  
    Imports System.Runtime.Serialization.Formatters.Binary  
    ```  
  
     在这种情况下，将使用二进制格式化程序以二进制格式保存对象。  
  
 下一步是添加代码，在创建对象时对文件中的对象进行反序列化。  
  
### <a name="to-deserialize-an-object"></a>反序列化对象  
  
1. 向序列化数据的文件名的类中添加一个常量。  
  
    ```vb  
    Const FileName As String = "..\..\SavedLoan.bin"  
    ```  
  
2. 修改 `Form1_Load` 事件过程中的代码，如下所示：  
  
    ```vb  
    Private WithEvents TestLoan As New LoanClass.Loan(10000.0, 0.075, 36, "Neil Black")  
  
    Private Sub Form1_Load() Handles MyBase.Load  
        If File.Exists(FileName) Then  
            Dim TestFileStream As Stream = File.OpenRead(FileName)  
            Dim deserializer As New BinaryFormatter  
            TestLoan = CType(deserializer.Deserialize(TestFileStream), LoanClass.Loan)  
            TestFileStream.Close()  
        End If  
  
        AddHandler TestLoan.PropertyChanged, AddressOf Me.CustomerPropertyChanged  
  
        TextBox1.Text = TestLoan.LoanAmount.ToString  
        TextBox2.Text = TestLoan.InterestRate.ToString  
        TextBox3.Text = TestLoan.Term.ToString  
        TextBox4.Text = TestLoan.Customer  
    End Sub  
    ```  
  
     请注意，首先必须检查该文件是否存在。 如果存在，则创建 <xref:System.IO.Stream> 类来读取二进制文件和 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 类，以转换该文件。 还需将流类型转换为 Loan 对象类型。  
  
 接下来，必须添加代码以将本文框中输入的数据保存到 `Loan` 类，然后必须将类序列化到文件中。  
  
### <a name="to-save-the-data-and-serialize-the-class"></a>保存数据并对类进行序列化  
  
- 将以下代码添加到 `Form1_FormClosing` 事件过程中：  
  
    ```vb  
    Private Sub Form1_FormClosing() Handles MyBase.FormClosing  
        TestLoan.LoanAmount = CDbl(TextBox1.Text)  
        TestLoan.InterestRate = CDbl(TextBox2.Text)  
        TestLoan.Term = CInt(TextBox3.Text)  
        TestLoan.Customer = TextBox4.Text  
  
        Dim TestFileStream As Stream = File.Create(FileName)  
        Dim serializer As New BinaryFormatter  
        serializer.Serialize(TestFileStream, TestLoan)  
        TestFileStream.Close()  
    End Sub  
    ```  
  
 此时可再次生成并运行应用程序。 最初，默认值在文本框中显示。 尝试更改这些值并在第四个文本框中输入名称。 关闭该应用程序，然后重新运行。 请注意，现在文本框中将显示新值。  
  
## <a name="see-also"></a>请参阅

- 序列化 (Visual Basic)[](index.md)
- [Visual Basic 编程指南](../../index.md)
