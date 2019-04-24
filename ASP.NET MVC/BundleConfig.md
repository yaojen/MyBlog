#  ASP.NET MVC BundleConfig設定

之前在練習寫一些靜態網頁的時，通常要引入CSS或js檔案都必須一個一個手動加入頁面，如果參考的數量一多的時候，其實也有可能忘動忘西的。
在Asp.net MVC 可以透過Bundle(綁成一束?)的方式，先將相關的CSS或js檔案設定在BundleConfig當中，需要使用時再呼叫該BundleConfig，
即可將Bundl設定檔中所設定的CSS或JS，引用到該頁面當中。使用Bundle可將css與js檔案進行分類，需要時再使用；
另一個好處是，在正式發行時會將BundleConfig設定的檔案壓縮成同一個檔案，降低css與js檔案的大小。


在建立好MVC專案後，Bundle相關的實作會在App_Start資料夾下的BundleConfig.cs檔案，而這些Bundle的設定則會在專案啟動時進行註冊，也就是Global.asax檔案中的BundleConfig.RegisterBundles(BundleTable.Bundles0)這個方法。
![圖片一](https://az787680.vo.msecnd.net/user/卡萊爾/50779b22-2773-4594-9c6f-e3e2d22c3a77/1556096963_67107.PNG)
![圖片二](https://az787680.vo.msecnd.net/user/卡萊爾/50779b22-2773-4594-9c6f-e3e2d22c3a77/1556096969_12656.PNG)
    在使用上會透過Style.Render與Script.Render，這兩個方法分別產生CSS與js檔案的連結。

再來看看BundleConfig的實作，若是要加入的是js檔案，則需要使用ScriptBundle類別，而加入的是css檔案，則使用SytleBundle類別，這兩個類別都繼承自Bundle。
在建立ScriptBundle或Stylebundle的實體的時候，需要傳入一個字串參數，當成這個Bundle的虛擬路徑，雖然是字串變數，但測試後VS提示必須是"~/url"開頭才可以，
url可自訂，我想就自訂一些有意義的名稱就可以了。
後面的Include方法則是css/js的實體路徑。

![圖片三](https://az787680.vo.msecnd.net/user/卡萊爾/50779b22-2773-4594-9c6f-e3e2d22c3a77/1556096971_82196.PNG)



[參考](https://blog.darkthread.net/blog/aspnet-mvc4-bundle-and-minify/)