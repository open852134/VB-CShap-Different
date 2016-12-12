# VB-CShap-Different
VB與C# 程式語法與物件用法的紀錄


<h1>變數變數宣告</h1>

一般變數

VB
``` VB
Dim int1 As Integer = 3
Dim string1 As String = "String"
Dim double1 As Doublue = 0.001
``` 
C#
``` C#
int int1 = 3;
string string1 = "String";
double double1 = 0.001;
```
陣列(<span style="color:orange;">注意括弧號 []  () </span>)

VB
``` VB
'宣告size為25的陣列的空陣列
Dim ary_string As String() = New String(24) {}
Dim ary_string = New String(24) {}

'宣告2維陣列 (8,3) 陣列
Private Dary_string As Integer(,) = New Integer(7, 2) {}
Private Dary_string As = New Integer(7, 2) {}
``` 
C#
``` C#
//宣告size為25的陣列的空陣列
string[] ary_string = new string[25];

//宣告2維陣列 (8,3) 陣列
string[,] bb = new string[8, 3];
``` 

<h1>Event事件建立與使用</h1>

建立 Event 與 RaiseEvent

VB
``` VB
'直接宣告Event事件
Public Event ErrorEvent(Code As Integer, Message As String)

'觸發RaiseEvent
Try
    'Some Code
Catch ex As Exception
    RaiseEvent ErrorEvent(-1, "錯誤訊息" + ex.Message)
End Try
```
C#
``` C#
//必須與委派(delegate)一起建立
public delegate void ErrorDelegate(int Code, string Message);
public event ErrorDelegate ErrorEvent;

//觸發RaiseEvent
try {
     //Some Code
} catch (Exception ex) {
     ErrorEvent(-1, "錯誤訊息:" + ex.Message);
}
```

主程式端使用

VB
``` VB
-宣告物件,加入Events
Private WithEvents ezTool As ezClass = New ezClass()

-建立Handles事件
Private Sub ezTool_ErrorEvent(Code As Integer, Message As String) Handles ezTool.ErrorEvent
     -Do something...
End Sub
```
C#
```C#
//宣告物件,加入Events
ezClass ezTool = newezClass();
//建立Handles事件(輸入完 += 按下tab自動建立)
ezTool += ezTool_ErrorEvent;
void myPLC_ErrorEvent(int Code, string Message) {
     //Do something...
}
```

監聽多個物件Event事件

VB
```V
Private Sub EzTextbox_Click(sender As Object, e As EventArgs) Handles EzTextbox1.Click, EzTextbox2.Click, EzTextbox3.Click, EzTextbox4.Click,
                                                                      EzTextbox5.Click, EzTextbox6.Click, EzTextbox7.Click, EzTextbox16.Click,
     'Do Something
End Sub
```
C#
```C#
//在 FormLoad 時加入Event監看
private void MainForm_Load(object sender, EventArgs e) {
     EzTextbox1.Click += new EzTextbox_Click();
     EzTextbox2.Click += new EzTextbox_Click();
     EzTextbox3.Click += new EzTextbox_Click();
     EzTextbox4.Click += new EzTextbox_Click();
     //略...
}

public void EzTextbox_Click(sender As Object, e As EventArgs){
     //Do Something
}
```

<h1>跨執行緒UI操作</h1>

VB
```VB
Me.Invoke(Sub()
     TextBox.Text = "This is Value"
End Sub)
```
C#
``` C#
this.Invoke((EventHandler)(delegate {
     TextBox.Text = "This is Value";
}));
```

<h1>字串處理</h1>

字串切割

VB
``` VB
Dim str As String = "aaa bbbbccccc"
Dim result As String = Mid(str, 5, 4)
Console.WriteLine(result)
'bbbb
'start index => 1
```
C#(不支援Mid語法)
``` C#
string str = "aaa bbbbccccc";
string result = str.Substring(4, 4);
Console.WriteLine(result);
//bbbb
//start index => 0
```

<h1>Dialog彈出視窗</h1>

MsaageBox

VB
``` VB
'MsgBox
sMessage As String = "This is Message"
MsgBox(sMessage)

'Inputbox
Dim mValue = InputBox("訊息", "標題", "Default")

'Button Dialog
Dim dialogResult As DialogResult = MsgBox("訊息", MsgBoxStyle.Exclamation + MsgBoxStyle.YesNo, "標題")
If dialogResult = MsgBoxResult.Yes Then
     'Do Somthing
End If

MsgBoxStyle.vbInformation
MsgBoxStyle.vbExclamation
MsgBoxStyle.vbQuestion
MsgBoxStyle.vbCritical
```

C#
``` C#
//MessageBox
string sMessage = "This is Message";
MessageBox.Show(sMessage);

//Inputbox (C#不支援Inputbox,可以載入VB Dll,或是自己設計)
//加入參考 Microsoft.VisualBasic
Microsoft.VisualBasic.Interaction.InputBox("訊息", "標題", "Default");

//Button Dialog

//確認、取消 帶Icon 
DialogResult dialogResult = MessageBox.Show("訊息", "標題", MessageBoxButtons.YesNo, MessageBoxIcon.Warning);
if(dialogResult == DialogResult.Yes){
     //Do something
}


MessageBoxIcon.Asterisk
MessageBoxIcon.Error
MessageBoxIcon.Exclamation
MessageBoxIcon.Information
```



<h1>物件繼承</h1>

VB
``` VB
Public Class ezClock
  Inherits Label '繼承物件Label
End Class
``` 
C#
``` C#
public partial class ezLabel: Label { //繼承物件Label
     public Component1() {
         InitializeComponent();
     }
}
``` 

<h1>Thread Sleep</h1>
``` C#
Thread.Sleep(500);
//可以改用SpinWait，減少CPU效能應用
SpinWait.SpinUntil(() => false, 500);
``` 

<h1>Function使用</h1>

VB
``` VB
Private Function DecToBit() As String
     DecToBit = "回傳直"
End Function
``` 
C# (純物件導向，不支援function語法)
``` C#
public class ezTool{
    private string DecToBit(){
        return "回傳直";
    }
}

//使用
ezTool tool = new ezTool();
tool.DecToBit();
``` 

<h1>Moduel使用</h1>

VB
``` VB
Module mainModule
     Public Sub main()     '程式進入點
          PLC.Create()
          PLC.Connect()          
          '啟動表單
          mainform = New MainForm
          Application.Run(mainform)
     End Sub
End Module
//設定APP程式進入點
``` 
C# (純物件導向，不支援Moduel語法)
``` C#
public class MainModule{
     static void Main(){     //程式進入點
          PLC.Create();
          PLC.Connect();
          //啟用表單
          MainForm mainform = new MainForm();
          Application.Run(mainform);
     }
}
//設定APP程式進入點
``` 
