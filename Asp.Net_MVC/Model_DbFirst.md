Model_DbFirst
===
###### tags: `Asp.Net` `MVC` `C#` `Entity Framework` `DbFirst`

[參考資料](http://www.entityframeworktutorial.net/what-is-entityframework.aspx)
# DbFirst-建立edmx與資料庫的model
1.在Model資料夾右鍵-->新增項目
![](https://i.imgur.com/dYl1ust.png)

2.選擇ADO.NET實體資料模型，命名的名稱為DbContext名稱
![](https://i.imgur.com/jUNwzFB.png)

3.來自資料庫的EF Designer
![](https://i.imgur.com/lRuVzSL.png)

4.新增連結，若已存在可選擇已存在的ConnectionString
![](https://i.imgur.com/lL7JWxW.png)

5.Entity的名稱
![](https://i.imgur.com/ONkRWFt.png)

6.選擇在資料庫當中要Entity的table
![](https://i.imgur.com/maoFxfD.png)

7.可發現Modle資料夾下已產生EIP的edmx，當中包含相關的模型
![](https://i.imgur.com/AtyLCaw.png)

8.在Controller中，先將entity new出來，供後面的ActionResult使用
![](https://i.imgur.com/rxweih0.png)


# edmx介紹
1.在edmx上按下右鍵，選擇"開啟方式"
![](https://i.imgur.com/fVaJMj7.png)

2.選擇"XML(文字)編輯器"
![](https://i.imgur.com/9sSThsx.png)

3.縮排後，可以看到SSDL、CSDL、CS mapping Content
![](https://i.imgur.com/9BfdHQ8.png)

4.SSDL，資料庫中的table與column名稱
![](https://i.imgur.com/CH7cy72.png)

5.CSDL，Entity中的model
![](https://i.imgur.com/CfhV9Dj.png)

6.CS mapping Content，表示資料庫與Entity的對應方式
![](https://i.imgur.com/Yr0xrID.png)





# DbFirst-DB Schema調整，調整edmx
## 更新Entity的作法-一般欄位
1.在資料庫中新增一欄位address2
![](https://i.imgur.com/anIepRf.png)

2.可以在edmx當中看到，SSDL、CSDL、MSDL都沒有這個欄位(address2)
![](https://i.imgur.com/xarNx1W.png)

![](https://i.imgur.com/g3DH59m.png)

![](https://i.imgur.com/8Fv7GPN.png)


3.在edmx上面點選"右鍵"，"從資料庫更新模型"。
![](https://i.imgur.com/K1f5Bkp.png)

4.可以看到SSDL、CSDL、MSDL已經有address2的欄位
![](https://i.imgur.com/pNTrC3u.png)

![](https://i.imgur.com/8eoS5BF.png)

![](https://i.imgur.com/Dja9FuJ.png)

5.雖然更新了edmx，但已經產生的view要自己手動更新。


## 更新Entity的作法-DateTime欄位

1.在資料庫新增一欄位，資料型態為DateTime，且設定為getdate()，即新增資料時產生。
![](https://i.imgur.com/jsNZa89.png)

2.更新edmx時後，必須把該欄位的"StoreGeneratedPattern"屬性改為，Computed。
![](https://i.imgur.com/m7IFIiW.png)

3.該屬性相關可以查看如下
![](https://i.imgur.com/M6dkF5E.png)
測試後，Computed跟Identity都不會在更新時產生值。
[相關連結](https://msdn.microsoft.com/zh-tw/library/system.data.metadata.edm.storegeneratedpattern(v=vs.110).aspx)


## 更新Entity遇到的問題
1.資料庫中修改欄位名稱，等於刪除欄位再新增欄位，此會異動SSDL與mapping的刪除跟新增，但CSDL則不會有刪除的動作，只會有新增，我們必須自己動刪除該欄位。
但如果原來那個欄位我們在畫面上或其他的商業邏輯都已經在用那個欄位做運算呢?
可以直接改mapping
<ScalarProperty Name="Name" ColumnName="Name" />
第一個名稱為Entity的屬性，不變，第二個ColumnNmae為實體資料庫的欄位名稱，這個改成修改的名稱，這樣就不會調整程式

2.在資料庫中新增欄位，dataType為dateTime


[參考資料1](https://blog.miniasp.com/post/2011/05/02/Entity-Framework-Update-Model-from-Database.aspx)

[參考資料2](http://ithelp.ithome.com.tw/articles/10161127)