Model_CodeFirst
===
###### tags: `Asp.Net` `MVC` `C#` `Entity Framework` `CodeFirst`



# CodeFirst-新增Model後如何更新回資料庫
1.在Models Floder新增一個Model
![](https://i.imgur.com/41ea1br.png)

2.在繼承DbContext的Class中，新增一個prop，型態為DbSet<T/>，T是剛剛新增的那個Model名稱，後面的為更新回資料庫的名稱。
public DbSet<Car> Cars { get; set; }，例如這個例子，Car就是剛剛建立的類別，Cars是建立到資料庫的資料表名稱．
![](https://i.imgur.com/jLrXdo1.png)

3.開啟"套件管理器主控台"，
指令:add-migration acccs3
![](https://i.imgur.com/4c8x0Nm.png)

4.可以看到Migrations Folder會新增剛剛指令的Migration內容
![](https://i.imgur.com/AGXmfER.png)

5."套件管理器主控台"
指令:update-database
![](https://i.imgur.com/8FCInek.png)

# CodeFirst-空的migration 新增SQL指令

# CodeFirst-其他指令與概念 
 - 概念
     - EF針對各類別Primary Key的自動剖析規則如下
         - 屬性名稱為Id或類別名稱Id將被視為主鍵(Key)
         - Key型態被設定為數字或GUID時，將被自動設定為Identity欄位
         - 若要避免Key被自動設定成為Identity欄位，可透過
         [DatabaseGenerated(DatabaseGeneratedOption.None)]標籤調整
     - EF針對各類別Foreign Key的自動剖析規則如下
         - 屬性名稱 = 導覽屬性名稱 +　參考主鍵屬性名稱，將被視為Foreign Key
         - 屬性名稱 = 參考主鍵屬性名稱,將被視為Foreign Key
         實務上只為依循一個規則來設計所有Model
     - 每一個Model都要有Key，最好命名為ID，型態為Int
     - 
 - 指令
     - -force
     - 已經add-migration怎麼恢復
     - foregin key怎麼用
     - 一對多的關係如何建立 多對多關係

# migration downgrade
[參考資料](http://www.huanlintalk.com/2015/08/entity-framework-migration-gotchas_29.html)
[參考資料](https://msdn.microsoft.com/en-us/data/dn481501.aspx)

### 其他
像是Datetime這樣的value Type如何在資料庫設定是可null的呢？
``` c#=
public Datetime? updateTime {get; set;}
```
加上問號


# 做錯了怎麼辦
*  指定migration
1.指令:  Get-Migrations
會列出所有的migration
2. Update-Database –TargetMigration: "要使用的migration"


# 更新現在的migration，這個migration還沒有update-database唷
1. Add-Migration [existingname] -Force   強制更新migration檔案，會依照目前的情況去更新，
這個方法跟刪除migration後再add-migration的意思是一樣的





# Code First 
## 指令：
* 啟用資料庫移轉
```shell=
    enable-migrations
```
* 新增執行資料庫移轉
```shell=
    add-migration Init(這個名稱可自行定義)
```
* 列出執行的指令碼（跟DBA合作）
```shell=
Update-Database -Script -Verbose
```
* 執行更新資料庫移轉，
```shell=
    update-database
```


## 來自資料庫的Code First
[我的點部落](https://dotblogs.com.tw/blog/preview/f12fe4bc-12cb-4f3c-8aad-5a5fec123d28)

## 空的Code First模型
### 流程
1. 建立Model(類別)，包含相關驗證與Model關係。
2. 建立ADO.NET 實體資料模型(空的Code First模型)，包含可能要建立connectionString
3. 調整第二步驟建立的模型的內容，包含加入第一步驟的Model跟使用Fluent API建立table間的關係。
4. 指令

**手動建立**
1. 在Models資料夾建立Model(類別)，包含相關驗證與Model之間的關係
2. 在Models資料夾建立DbContext，該類別繼承Dbcontext，base要傳入connectionString
3. 在web.config建立connectionString，這個connectiongstrgin的名稱就是傳入base的名稱
4. 指令

### 實作範例


1. 在Models資料夾下建立類別檔案，若有屬性要引入ComponentModel與ComponentModel.DataAnnotations兩個命名空間
![1234](https://i.imgur.com/UhP1q7I.png)


2. 在Models資料夾下面建立Context檔案，需要繼承DbContext類別。引入Data.Entity。
在建構式裡呼叫的基底類別所傳入的值，是在Web.Config裡建立的資料庫連結字串。
在這個DbContext類別當中，還要加入DbSet<T/>泛型屬性，如：public DbSet<Car/> Cars {get; set;}，要注意的是單數形態的car代表是專案中Model底下的類別名稱（通常我們會取單數，因為類別啊），Cars為資料庫的table名稱，在後面執行database-update後，cars就會更新到資料庫的table中．

![1234](https://i.imgur.com/QMdCl8t.png)

3. 開啟工具-->Nuget套件管理員-->套件管理主控台，在CodeFirst第一次輸入
  enable-migrations 
  但若是我們有多個繼承DbContext的類別，就必須指定要用哪個DbContext，語法如下
  enable-migrations –ContextTypeName WINSSON.Models.SampleContext
  -ContextTypeNmae:固定，後面的則是指定的DbContext


![12345](https://i.imgur.com/I4sAWmi.png)

4. 新增執行資料庫移轉
語法:add-migration 移轉名稱
通常希望移轉名稱可具有意義，第一次通常是Init
![](https://i.imgur.com/G8V0JGj.png)

5. 此時會建立一個Migration資料夾，並產生一指令檔案
![12345](https://i.imgur.com/iMXiJoS.png)

6. 執行更新資料庫移轉
語法:update-database
成功後會在目標的資料庫新增table

![234123](https://i.imgur.com/yfIOV1x.png)


[參考資料一](https://dotblogs.com.tw/supershowwei/2016/04/11/000015)
[參考資料二](http://kevintsengtw.blogspot.tw/2013/10/aspnet-mvc-entity-framework-code-first.html)
[參考資料三](http://blog.csdn.net/ayanamireizero/article/details/41038557)
[參考資料四](https://dotblogs.com.tw/eaglewolf/2013/03/01/94728)
[參考資料五](https://dotblogs.com.tw/johnny/2013/10/15/124293)
[CodeFirst Existing DB](http://blogs.uuu.com.tw/Articles/post/2014/08/27/%E4%BD%BF%E7%94%A8EntityFramework-6-Code-First%E8%88%87%E6%97%A2%E6%9C%89%E8%B3%87%E6%96%99%E5%BA%AB%E5%BB%BA%E7%AB%8B%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F.aspx)


### 設定一對多、多對多關係
[我的點部落](https://dotblogs.com.tw/blog/preview/2e8002a9-39ee-4c5a-8449-41db1c626790)

### 一對一
兩個類別之間，各自包含一個引用屬性，就會被當成一對一的關係，只是哪一張是主表，需要利用Fluent API來設定

主表
```csharp=
public class TableA
{
    [Key]
    public int TabTd { get; set; }
    
    public int C1 { get; set;}
    
    public virtual TableB tableB { get; set;}

}
```

副表
```csharp=
public class TableB
{
    public int TabId { get; set;}
    public int CC1 { get; set; }
    public virtual TableA TableA { get; set; }

}

```
設定關聯 A事主表，更新回資料庫後，可以到table的定義查看

```csharp=
public partial class DemoContext : DbContext
{
    public DemoContext()
        :base("name = DemoContext")
    {}
    
    public virtual DbSet<TableA> TableA {get; set;}
    public virtual DbSet<TableB> TableB {get; set;}
    
    protexted override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<TableA>()
            .HasOptional(x => x.TableB)
            .WithRequired(x => x.TableA)
    }
}
```




### Complex type 用來重複使用的欄位
1. 不能有主索引
2. 一個類別中只能有一個實體
3. 只能是引用屬性，不能是集合屬性

```csharp=
public class Table1
{
    public int Table1Id { get; set;}
    public Address Address { get; set;}
}


public class Table2
{
    public int Table2Id { get; set; }
    public Address Address { get; set; }

}

public class Address
{
    public string Addr1 { get; set;}
    public string Addr2 { get; set;}

}

```

若是依上面的方式做update-database 
Table1會有 
Table1Id
Address_Addr1
Address_Addr2的欄位，但這並不是我們想要，
我們希望
Address_Addr1 -> Addr1
Address_Addr2 -> Addr2

所以我們調整欄位屬性

System.ComponentModel.DataAnnotations.Schema 要using這個namespace
如果只加上這個做update-database也會更新到table column
```csharp=
public class Address
{
    [Column("Addr1")]
    public string Addr1 { get; set;}
    [Column("Addr2")]
    public string Addr2 { get; set;}

}
```
若只是設定欄位名稱，用屬性或API兩個擇一個應該可以
```csharp=

protested override void onModelCreating(DbModelBuilder modelbuilder)
{
    modelBuilder.configurations.Add(new AddressConfiguration());
}

private class AddressConfiguration : ComplexTypeConfiguration<Address>
{
    public AddressConfiguration()
    {
        this.Property(x => x.Addr1).HasColumnName("Addr1");
        this.Property(x => x.Addr2).HasColumnName("Addr2");
    
    }

}

```

### 繼承的對應設定
基底類別跟衍伸類別都對應到同樣一張表(TPH，Table Per Hierarchy)
```csharp=
using System.ComponentModel.DataAnnotations;

public class Table1
{    
    [Key]
    public int Table1Id {get; set;}
    public int C1 {get; set;}
    public int C2 {get; set;}

}

public class Table2: Table1
{    

    public int C3 {get; set;}
    public int C4 {get; set;}

}

#先新增資料
class Program
{
    static void Main(string[] args)
    {
        using(DemoContext db = new Democontext())
        {
            db.Table1.Add(new Table1 {C1=1, C2 =2});
            db.Table2.Add(new Table2 {C1=1, C2 =2, C3 =3,C4 =4});
        }
    }
}

```
table1、table2都寫入到同一張表當中，但用表當中的另一個欄位去判斷

透過Flunet API設定辨識的欄位，和辨識資料的值，如果不設定也是會新增一個欄位(Discriminator，然後欄位的值用Table1/Table2去判斷)

```csharp=
IList<Student> studentsToRemove = new List<Student>() {
                                    new Student() { StudentId = 1, StudentName = "Steve" };
                                    new Student() { StudentId = 2, StudentName = "Bill" };
                                    new Student() { StudentId = 3, StudentName = "James" };
                                };
using (var context = new SchoolDBEntities())
{
    context.Students.AddRange(newStudents);
    context.SaveChanges();
}    
    
using (var context = new SchoolDBEntities())
{
    context.Students.RemoveRange(studentsToRemove);
    context.SaveChanges();
}
```


```csharp=
public DbSet<Table1> Table1 {get; set;}
public DbSet<Table2> Table2 {get; set;}
protected orverride void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Entity<Table1>()
    .Map( x => {
            x.ToTable("Table1");
            x.Requires("QQ").HasValue("AA");
    })
    .Map<Table2>(x =>{
        x.Requires("QQ").HasValue("BB");
    });
}

```
設定後，欄位名稱改為QQ，然後Table1改用AA，Tabel2改用BB去判斷


* 把資料各自映射到各自的資料表當中(基底/衍伸)，在衍伸類別的資料表生成外鍵來關聯基底類別資料表(TPT，Table Per Type)
跟一般的建立方式一樣

```csharp=
public DbSet<Table1> Table1 {get; set;}
public DbSet<Table2> Table2 {get; set;}

protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Entity<Table1>.ToTable("Table1");
    modelBuilder.Entity<Table2>.ToTable("Table2");

}
```
在資料庫就會生成兩個Table，分別是Table1(Table1Id,C1,C2)與Table2(Table1Id,C3,C4)，其中Table2的Table1Id會去Table1串


    * 各自映射到資料表，衍伸類別的資料表中還會包含基底類別的欄位(TPC，table Per concreate Type)也就是各自一張表
透過FluentAPI設定MAP
```csharp=
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Entity<Table1>()
    .Map(x =>
    {
        x.ToTable("Table1");
    })
    .Map<Table2>(x =>
    {
        x.ToTable("Table2");
        x.MapInheritedProperties();
    });
}
```
    
#### FluentAPI
[參考](http://xpower2888.pixnet.net/blog/post/222218921-entity-framework%E5%AF%A6%E4%BE%8B%E8%A9%B3%E8%A7%A3---nele)
[參考](http://blog.developer.idv.tw/2014/11/entity-framework-code-first_25.html)

## 其他
* c#的value type在程式當中一定要有值，但資料庫是用not null的設定來判斷，所以在寫Model時候可以在資料型態後加上?來表示可為null
* 設計時可能會用到enum資料型態，enum是設計在C#當中，在update-database時候會把它變成int的型態

## 問答
Q:將物件名稱複數化或單數化，資料庫的資料表複數，類別名稱單數，撈出資料類型為陣列串列類型，變數名稱宣告呈複數，撈出單一資料，變數名稱，單數．
Q:當執行add-migration xxx的時候，如果做壞了，可否直接在Migrations的資料夾做刪除
        A:可以。



MVC - Model - Entity Framework Code First with SQL Compact(sqlce)
===
預設的code first方式是產生localDB(sql express)，這邊要紀錄如何改成sqlCE，sqlCE更小型的資料庫，可直接附加在服務中，不需要SQL SERVER。

1. nuget下載，EntityFramework.SqlServerCompact套件。
2. 在web.Config新增connectionString
```xml=
<add name="ProjectDb" connectionString="data source=|DataDirectory|ProjectDb.sdf"  providerName="System.Data.SqlServerCe.4.0" />
<!-- 說明
name:連線字串名稱
connectionString:
    DataDirectory:新增的目的，這會新增在App_Data，
    ProjectDb.sdf:資料庫名稱，sdf就是sqlce的附檔名
providerName:提供連線的dll，在下載SqlServerCompact之後就會有在web.Config當中
-->
```
3. 在Models 資料夾中新增Models。
4. 在Models資料夾中新增類別繼承DbContext，建構式的base傳入字串則為在Web.Config當中的connectionStrgin中的name;建構式也可以新增資料;新增Dbset屬性
5. code first指令
:::success
* Enable-Migrations
* Add-Migration Initial
* Update-Database
:::