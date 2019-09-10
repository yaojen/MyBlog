# Autofac



一直在文章中看到DbContext本來就有實做Unit of Work，但一直搞不懂，今天跟朋友討論後突然懂了。

Unit of Work，也就是當我在處理某個request時候，如果我的需求需要撈到不同的表來進行資料處理，處理完後再寫入兩張不同的table，這些動作應該在一個連線中處理完，也就是unit of work概念，每次的工作單元，這些動作就是這次的工作單元，在執行DbContext.SaveChange之前都會被視為一個單元，

但因為關注點分離，我們把不同的table拆成不同的repository，有需要的時候就new出來，new同時注入這個dbcontext，但假設我有一個controller需要用到兩個repository，這時候透過di套件new出兩個repository，然後注入dbcontext，其實是不同的dbcontext，我在異動其中一個repository時候，另一個部會連動。這時我們可以在IOC套件指定，dbcontext new出來的頻率是perrequest。每當requeset近來的時候，new 出的repository 都式注入同一個dbcontext，

但這時候又會遇到一個問題，我們要如何操作這個dbContext，尤其是在SaveChange時候，Rollback時候，這些動作在同一個工作單元都應該一起動。所以在設計一個IUnitOfWork的介面，在實作這個介面的內容，

在repository可以注入跟unitofwork同樣的dbcontext，這時候我們再把每個controller建構子新增IUnitOfWork，所有的對db的save/rollerback動作都拉到unitofwok來做，或是repository注入unitofwork，透過unitofwork裡面的dbContext處理要處理的資料，

我的概念是傾向把共用的方法抽出來到unitofwork，例如SaveChanges，跟rollerback，當然那個unitofwork通常會繼承IDispose，這些部分。另外再controller時，同時注入IxxxService/IUnitOfwork



另一種做法是，設計一個unit of work，裡面有repository，所有的服務都包再一起，unit of work 裡面有customer/ order/detail/employee...的repository，

```csharp
因為都在同一unit of work的類別中，使用的都是同一個dbcontext，就可以把它視為同一個連線了
```



## 步驟



1. 建立ContainerBuilder
2. 透過ContainerBuilder註冊服務
3. 建立容器 ContainerBuilder.Builder();
4. 把容器設定給DependencyResolver



## 註冊服務

```csharp
public static class AutofacConfig
{
    public static void Run()
    {
        //1.建立容器
        var builder = new ContainerBuilder();
        
        //2.註冊服務
        
        //取得目前執行app的Assembly
        Assembly assembly = Assembly.GetExecutingAssembly();
        var service = Assembly.Load("Northwnd.Service");//載入其他組件
        
        
        // a. 直接註冊某個物件
		builder.RegisterType<MyService>().As<IMyService>();

		// b. 註冊所有名為Service結尾的物件
		builder.RegisterAssemblyTypes(assembly)
    			.Where(x => x.Name.EndsWith("Service",StringComparison.Ordinal))
        		.AsImplementedInterfaces();

		//C.註冊所有父類別為BaseMethod的物件
		builder.RegisterAssemblyTypes(assembly)
            	.Where(x => x.BaseType == typeof(BaseMethod)).AsImplementedInterfaces();

		//D.註冊實作某個介面的物件
		builder.RegisterAssemblyTypes(assembly)
  				.Where(x=>x.GetInterfaces().Contains(typeof(ICommonMethod)))
           		.AsImplementedInterfaces();

		//註冊controller與apiController
		builder.RegisterControllers(assembly);
		builder.RegisterApiControllers(assembly)
            
        //3.建立容器
        var container = builder.Build();
        
        //4.把容器設定給DependencyResolver
        var resolverApi = new AutofacWebApiDependencyResolver(container);
        GlobalConfiguration.Configuration.DependencyResolver = resolverApi;
        DependencyResolver.SetResolver(new AutofacDependencyResolver(container));
        
    }
}


```

## 在App啟動點Global 呼叫啟用Autofac

```csharp
public class WebApiApplication : System.Web.HttpApplication
{
    public void Application_Start()
    {
        AreaRegistration.RegisterAllAreas();
        GlobalConfiguration.Configure(WebApiConfig.Register);
        FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
        RouteConfig.RegisterRoutes(RouteTable.Routes);
        BundleConfig.RegisterBundles(BundleTable.Bundles);

        //啟用Autofac
        AutofacConfig.Run();
    }
}
```

[參考](https://dotblogs.com.tw/nigelhome/2017/09/22/autofac)

```csharp
public class AutofacConfig
{
    public static void Register
    {
        var builder =  new ContainerBuilder();
        //註冊controller
        builder.RegisterControllers(Assembly.GetExecutingAssembly());
        
        //註冊service
        builder.RegisterAssemblyTypes(Assembly.GetExecutingAssembly())
            	.Where(x => x.Name.EndWith("Service")).AsImplementedInterfaces();
        
        //註冊repository
        builder.RegisterAssemblyTypes(Assembly.GetExecutingAssembly())
               .Where(x => x.Name.EndsWith("Repository"))
               .AsImplementedInterfaces();
        
        // 建立容器
		IContainer container = builder.Build();
        
        // 解析容器內的型別
		AutofacDependencyResolver resolver = new AutofacDependencyResolver(container);
        
        // 建立相依解析器
		DependencyResolver.SetResolver(resolver);
    }
    
}
```



# Life Cycle

# LifeTime
autofac會依據lifetime Cycle設定每一個instance甚麼時候應該產生，有下列四種
* InstancePerDependency
* SingleInstance
* InstancePerLifetimeScope
* InstancePerRequest

## InstancePerDependency
每一次呼叫都產生一個新的實體，產生的實體(instance)隨著呼叫者的生命週期消滅。
以mvc的controller來說，假設用到兩個service，假設每個service在建構的時候都需要注入userInfo這個實體，那

如果 userinfo在設定時候是InstancePerDependency，就會產生兩個userInfo實體分別注入兩個service當中。

這個是預設。

## SingleInstance

也就是Singleton的模式，在啟動網站時候就會產生，也只有一個。生命週期隨著網站的開啟跟關閉。

## InstancePerLifetimeScope

跟PerRequest容易搞混，在controller中需要注入兩個instance，一個是A，一個是B，此時產生B實體時候還需要注入A，這時候如果使用的是，InstancePerDependency，會產生兩個A，一個是注入Controller，一個注入B。但如果我們希望注入Controller跟B的是一樣的A，就必須在註冊時候針對A設定InstancePerLifeTimeScope，。就字面上翻譯的意思就是每一個LifeTime的範圍，只會產生一個實體，以mvc網站的架構來說，每一個requet進來，他的LifeTime就是每個request。但如果對desttop的軟體可能就不是這樣。

## InstancePerRequest

就字面上翻譯，每一次web Request只會產生一個instance，這最常用在Unit of work設計模式中的dbContext，

我們每一次的request操作DB的實體應該都是同一個instance。