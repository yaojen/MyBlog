Http Modules and Http Handlers
===
[參考](http://vito-note.blogspot.com/2013/01/http-modules-and-http-handlers.html)

# 前言
當IIS 收到Http Request，會視這個Request類型，交由不同的Handler處理。例如IIS收到使用者提出一個網頁aspx需求，就會執行asp.net page handler以處理請求。
當收到asmx需求，IIS就會執行asp.net service handler以處理請求，若要自行處理特定的請求，必須針對這個特定的請求設計特定的Http Handler或者Http Module。

* Http Module，類似IIS早期版本中的ISAPI Filter，是用來將任何需求加上某些處理，例如驗證或壓縮回應。
* Http Handler，類似IIS早期版本中的ISAPI Extension，用來定義Http的處理程序，以處理特定型態的Http Web Request。
兩者的主要不同是，一般Handler所處理的需求，都會針對特定的檔案路徑，或是副檔名。所以，若要建立一個IIS的擴充功能，可以先判斷這個功能是否針對特定的url/extension，
來決定是要建立一個MModule或是Handler

**ISAPI**
ISAPI（Internet Server Application Programming Interface，Internet伺服器API），是可被微軟IIS加載和調用的DLL。用於擴展HTTP伺服器的功能。
由Process Software and Microsoft Corporation共同制定的一組Internet Service API，主要目的就是改進HTML的特性，可以讓你延伸伺服器特性，以發展各式各樣的服務。
ISAPI的程式分兩種。Filter與Extension，IIS 6.0的ISAPI Filter與Extension都是附掛在w3wp.exe中的一種延伸功能，並沒有管線化的設定，而是分別做不同用途。
**什麼是 ISAPI 伺服器擴充程式？**
ISAPI 擴充程式是一種特別的 DLL，其內部必須依照 ISAPI 的規範，並提供某些特定函式。 它主要是用來開發能掛載在 IIS 上的 Web 應用程式的模組，例如 ASP 的核心檔案 asp.dll 即是一種 ISAPI Extension； ASP.NET 的核心檔案 aspnet_isapi.dll 也是一個 ISAPI Extension。 當 ISAPI 擴充程式被安裝完成之後，若 IIS 收到該服務的請求，就會載入該擴充程式，並把請求交給該擴充程式來處理。
**什麼是 ISAPI 篩選器？**
ISAPI 篩選常式 (Filter) 也是在啟用 ISAPI 的 HTTP 伺服器上執行的 DLL，用來篩選出入伺服器的資料。 它主要是用來開發加強 IIS 現有功能的模組，像是處理驗證、壓縮以及資料加密等，都可以利用 ISAPI Filter 來實作。
到了 IIS7 開始，提供二種方便的方法： module 和 handler ，讓我們可以輕鬆撰寫伺服器的擴充功能。


# HttpModule
通常設計asp.net module，大多是用來撰寫web application共用功能，例如:權限判斷、錯誤處理等。在asp.net 中已有一些預設的模組，例如:FormsAuthenticationModule 、 RoleManagerModule 、 UrlRoutingModule 。
在Global.asax裡，我們通常也會做一些初始化的事情，例如urlRewrite或ApplicationErrorHandle，可以將這些程式碼獨立出來，由自訂模組中執行。
使用Module的好處是不用跟整個網站綁在一起。在不用更動任何程式碼的情況下，將特定的需求另外新訂模組來執行，且可以藉由web.config的設定，隨時停用或啟用。

底下是定義在machine.config中的預設模組
```csharp
<httpModules>
    <add name="OutputCache" type="System.Web.Caching.OutputCacheModule" />
    <add name="Session" type="System.Web.SessionState.SessionStateModule" />
    <add name="WindowsAuthentication" type="System.Web.Security.WindowsAuthenticationModule" />
    <add name="FormsAuthentication" type="System.Web.Security.FormsAuthenticationModule" />
    <add name="PassportAuthentication" type="System.Web.Security.PassportAuthenticationModule" />
    <add name="RoleManager" type="System.Web.Security.RoleManagerModule" />
    <add name="UrlAuthorization" type="System.Web.Security.UrlAuthorizationModule" />
    <add name="FileAuthorization" type="System.Web.Security.FileAuthorizationModule" />
    <add name="AnonymousIdentification" type="System.Web.Security.AnonymousIdentificationModule" />
    <add name="Profile" type="System.Web.Profile.ProfileModule" />
    <add name="ErrorHandlerModule" type="System.Web.Mobile.ErrorHandlerModule, System.Web.Mobile, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
    <add name="ServiceModel" type="System.ServiceModel.Activation.HttpModule, System.ServiceModel.Activation, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
    <add name="UrlRoutingModule-4.0" type="System.Web.Routing.UrlRoutingModule" />
    <add name="ScriptModule-4.0" type="System.Web.Handlers.ScriptModule, System.Web.Extensions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
</httpModules>
```


**建立HttpModule**
要建立自訂的HttpModule模組，可以實作IHttpModule介面的類別，或直接新增一個ASP.NET模組項目。必須複寫Init方法，並在這個方法訂閱HttpApplication事件

自訂一個ErrorModule，用來處理Application.Error事件
```csharp
public class MyErrorModule : IHttpModule
{
    public void Init(HttpApplication application)
    {
        application.Error += new EventHandler(ApplicationOnError);
    }
 
    //錯誤處理程式
    public void ApplicationOnError(Object source, EventArgs e)
    {
        //取得目前 HTTP 要求的 System.Web.HttpContext 物件
        HttpContext ctx = HttpContext.Current;
 
        //由 HttpContext 物件取得例外狀況
        Exception exception = ctx.Server.GetLastError();
        string errInfo = exception.InnerException.StackTrace.ToString();
 
        //取得目前 HTTP 的回應和要求物件
        HttpResponse response = ctx.Response;
        HttpRequest request = ctx.Request;
 
        //取出要求物件中的 QueryString 資訊
        NameValueCollection QuertStrings = request.QueryString;
 
        //回應要求
        response.Write(errInfo.Replace("\r\n", ""));
 
        ctx.Server.ClearError();
    }
 
    public void Dispose()
    {
        //清除程式碼置於此。
    }
}

```

**註冊HttpModule**
要使用自訂模組，必須先在web.config檔案中註冊這個模組，不同版的IIS，會有點不同。

* IIS6/IIS7
```csharp
<system.web>
    <httpModules>
        <add name="CustomErrorModule" type = "MyErrorModule" />
    </httpModules>
</system.web>
```
若這個檔案是使用 Web Application 網站建立，或是使用類別庫開發的，這時候的型別設定，必須加上命名空間，如下範例：
type = "命名空間.類別名稱, 組件名稱"
```csharp
<system.web>
    <httpModules>
        <add name="CustomErrorModule" type = "MyPractice.Common.MyErrorModule, MyPractice" />
    </httpModules>
</system.web>
```
若是使用 IIS7 整合管線模式，必須將以上設定，由 <system.web> 區段改成 <system.webServer> 區段，如下範例。

```csharp
<system.webServer>
    <modules>
        <add name="CustomErrorModule" type = "MyPractice.Common.MyErrorModule, MyPractice" />
    </modules>
</system.webServer>
```
目前使用的
```csharp
  <system.webServer>
    <modules runAllManagedModulesForAllRequests="true">
      <remove name="TelemetryCorrelationHttpModule" />
      <add name="TelemetryCorrelationHttpModule" type="Microsoft.AspNet.TelemetryCorrelation.TelemetryCorrelationHttpModule, Microsoft.AspNet.TelemetryCorrelation" preCondition="integratedMode,managedHandler" />
      <add name="loggerModule" type="LiveWeb.App_Start.IpBlockModule, LiveWeb" />
    </modules>
  </system.webServer>
```

# HttpHandler
http Handler就是處理用戶端的Http request，asp.net 本身就是一個http handler，專門來處理.aspx .ascx .asmx等檔案，
因此如果我們如果想要處理特殊的類型請求，就必須建立自己的常式。
通常如果要自訂http handler，大部分的目的要處理特別的檔案類型，幾個常見的應用，如:URL rewrite、防止圖檔的盜連，產生驗證圖檔等等。
handler 就類似過去 IIS 中的 ISAPI extension ，是真正處理特定請求的主體程式。 它會處理用戶端提出的請求，並將處理過後的訊息回應給用戶端。 ASP.NET 中的 Page 類別本身就是實作 IHttpHandler 。

## 建立HttpHandler
步驟如下:
1. 建立一個類別繼承IHttpHandler，命名方式，XXXHandler。
2. 註冊HttpHandler

**建立HttpHandler**
要建立 HttpHandler 處理常式，你可以自訂一個實作 IHttpHandler 介面的類別，或者直接新增一個「ASP.NET 處理常式」項目。 
並且完成 IsReusable 屬性和 ProcessRequest 方法。

IsReusable ：表示 IHttpHandler 執行個體是否可重複使用，可重複使用則為 true，否則為 false。
ProcessRequest ： 這個方法用來處理 HTTP request 提出的要求，當自訂的處理常式被執行時，就會執行這個方法。 你必須在這個方法中撰寫 response 的程式碼。

以下範例示範自訂一個 EmpPhotoHandler 處理常式，用來取得儲存在資料庫中的員工照片資料。
```csharp
public class EmpPhotoHandler : IHttpHandler
{
    public void ProcessRequest(HttpContext context)  //HttpContext就包含request / response
    {
        string EmpID = context.Request.QueryString["ID"];
        byte[] photo = GetEmpPhoto(EmpID);
 
        if (photo != null)
        {
            context.Response.ContentType = "image/jpeg";
            context.Response.BinaryWrite(photo);
        }
        else
        {
            context.Response.Write("./images/noimage.gif");
        }
    }
 
    public bool IsReusable
    {
        get { return false; }
    }
}
```

**註冊HttpHandler**
可以分成mvc架構跟先前的webform架構。

**MVC架構**
mvc架構不再web.config註冊handler，則是在RouteConfig.cs當中註冊rotue，指定路徑走哪一個Handler
寫RouteConfig
```csharp
//先寫routeConfig，繼承IRouteHandler
public class IosManagerRouteConfig : IRouteHandler
    {
        public IHttpHandler GetHttpHandler(RequestContext requestContext)
        {
            return new IosManagerHandler(requestContext);    //指向客製的Handler
        }
    }

```
```csharp
//將剛剛的routeConfig註冊到App_start中的RouteConfig類別
public class RouteConfig
    {
        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

            var iosManagerRoute = new Route("{controller}/{action}", new IosManagerRouteConfig());   
            iosManagerRoute.Constraints = new RouteValueDictionary() { { "Controller", "IosManager" }, { "Action", "Index" } };  
            routes.Add(iosManagerRoute);  //將routeConfig加入倒route當中
        
            routes.MapRoute(
                name: "Default",
                url: "{controller}/{action}/{id}",
                defaults: new { controller = "ModifySetting", action = "Index", id = UrlParameter.Optional }
    
            );
        }
    }
```

**webform架構**
跟HttpHandler和註冊HttpModule一樣，需要依據IIS版本選擇設定區段，下面是IIS6的版本
```csharp
<system.system.web>
    <httpHandlers>
      <add path="*.Photo" verb="*" type="Practice70515.Common.EmpPhoto" />
    </httpHandlers>
</system.system.web>
```
**設定IIS6與IIS7注意事項**
若要兼顧 IIS6 及 IIS7，可在 web.config 中同時保留 httpHandlers(for IIS6) 及 handlers(for IIS7) 裡的相同定義，
但記得要加上<validation validateIntegratedModeConfiguration="false" />，不然IIS7會因為定義重複出現而發生錯誤
```csharp
<system.webServer>
    <validation validateIntegratedModeConfiguration="false" />
  </system.webServer>
```



# 範例
參考資料直接把Module跟Handler寫成類別檔，然後將dll掛載至IIS。