UnitTest
===
# 基本概念
測試應該包含邏輯程式碼，不管他看起來多簡單。為了加入測試通常會伴隨著重構。
書者提到，藝術的部分在於如何找到適當的地方加入或使用一個中間層來測試你的代碼。
## 命名方式
執行測試的類別應該是，測試類別的名稱Test，例如要測試的類別名稱為LogAnalyzer，那執行測試的類別應該是LogAnalyzerTest。
測試類別內的方法命名方式為，下面整理出兩種未來可以採用的。

1. 待測函數名稱加上測試狀態與預期行為
* MethodName_StateUnderTest_ExpectedBehavior，例如:isAdult_AgeLessThan18_False()，withdraw_invalidAccount_ExceptionThrown()

2. 預期行為加上測試狀態，方法名稱可能因為重構改名，所以拿掉方法名稱只有測試狀態跟預期結果。
* Should_ExpectedBehavior_When_StateUnderTest
    範例：Should_ThrowException_When_AgeLessThan18()、
         Should_FailToWithdrawMoney_When_InvalidAccount()
* When_StateUnderTest_Then_ExpectedBehavior
    範例：When_AgeLessThan18_Then_isAdultAsFalse()、
          When_InvalidAccount_Then_WithdrawMoneyToFail



# 手動測試
## 題目一
今天是不是聖誕節?

```csharp
public class Holiday
{
    public bool IsTodayXmas()
    {
        if (DateTime.Now.ToString("MMdd").Equals("1225"))
        {
            return true;
        }
        return false;
    }
}
```
通常會依據需求產生如上的程式碼，但若要針對該方法進行測試時會發現，其實無法針對這個方法測試，因為該方法跟DateTime 耦合度很高，
如果我們要可以進行測試應該可以控制DateTime這個參數，此時要專注的是日期是不是等於聖誕節的邏輯。

解決方法:把DateTime.Now，用方法抽出來。(也可以用prop，在new出實體的時候進行設定，但會異動原來程式碼太多)
可以注意到GetToday方法是protected virtulal。(protected:只有自己或繼承的類別可以使用，new出來的實體不能使用;virtual:代表可以override該方法)
```csharp
public class Holiday
{
    public bool IsTodayXmas()
    {
        DateTime dateTime = GetToday();
        if(dateTime.ToString("MMdd").Equals("1225"))
        {
            return true;
        }
        return false;
    }
    protected virtual DateTime GetToday()
    {
        return DateTime.Today;
    }
}
```

撰寫測試方法，要測試兩種狀況，一種輸入不是聖誕節預期回傳false，一種輸入聖誕節預期回傳true
但在這之前，我們必須可以控制GetToday的回傳內容，所以新增一個子類別繼承Holiday，
並新增一個屬性跟方法設定Today，並改寫GetToday方法，讓他回傳新增的屬性

我們的測試就是針對這個假的Holiday進行測試
```csharp

public class TestHoliday : Holiday
{
    private DateTime datetime;
    public void SetDatetime(int MM,int dd)
    {
        datetime = new DateTime(2019,MM,dd);
    }
    protected override DateTime GetToday()
    {
        return datetime;
    }
}

public class HolidayTest
{
    [TestMethod]
    public void IsTodayXmax_1224_false()
    {
        var testHoliday = new TestHoliday();    //針對TestHoliday進行測試
        testHoliday.SetDatetime(12,24);
        var result = testHoliday.IsTodayXmas();
        Assert.AreEqual(false,result);
    }
    [TestMethod]
    public void IsTodayXmax_1225_true()
    {
        var testHoliday  = new TestHoliday();
        testHoliday.SetDatetime(12,25);
        var result = testHoliday.IsTodayXmas();
        Assert.AreEqual(true,result);
    }
}

```

## 題目二
log檔案結尾是不是.SLF

```csharp
public class LogAnalyzer
    {
        public bool IsValidLogFileName(string name)
        {
            return name.EndsWith(".SLF");
        }
    }
}
```
測試代碼，因為檔案名稱可以用參數控制，就不再使用假物件。
```csharp
[TestClass]
public class LogAnalyzerTest
{
    private LogAnalyzer _logAnalyzer;
    public LogAnalyzerTest()
    {
        _logAnalyzer = new LogAnalyzer();
    }
    [TestMethod]
    public void When_FileNameEndWithTxt_then_false()
    {
        string filename = "13245.txt";
        TestFileName(filename, false);
    }
    [TestMethod]
    public void When_FileNameEndWithSLF_then_true()
    {
        string filename = "12345.SLF";
        TestFileName(filename,true);
    }
    private void TestFileName(string filename, bool expected)
    {
        var result = _logAnalyzer.IsValidLogFileName(filename);
        Assert.AreEqual(expected, result);
    }
}
```

# 虛設常式(stub)
適用情境，要測試的物件依賴另一個你無法控制的物件。這個物件可能是Web服務、系統時間、執行緒。
也就是在測試時無法決定相依物件的回傳值，也無法控制他的行為(例如想拋出一個例外)。虛設常式就派上用場。

虛設常式與模擬物件的差別，先了解在測試時我們不會針對stub進行測試，但我們會針對mock進行測試。虛設常式協助進行測試動作。
虛設常式會用到倚賴注入，這裡先總結幾個方法，1.建構式注入 2.prop注入 3. 在呼叫方法之前才注入假物件(工廠類別) 

# 模擬物件(Mock)

# 隔離框架
# 專有名詞
* Stub，虛設常式
* SUT，System Under Test
* Legacy Code，遺留代碼


老師各位同學 午安
這幾天想將目前專案加入測試,遇到一些問題想跟各位請教
測試 "是否允許登入"的邏輯，其方法內使用到其他類別的靜態方法
遇到靜態方法也一樣加入Interface，透過Substitute去模擬回傳結果嗎?

[參考](https://dotblogs.com.tw/hatelove/2015/11/26/unit-test-by-extract-and-override)