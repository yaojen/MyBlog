解構Static
===
```csharp
public class AuthService
{
    public bool IsValid(string account, string password)
    {
        string passwordFromDao = ProfileDao.GetPassword(account);
        string token = ProfileDao.GetToken(account);
 
        var validPassword = passwordFromDao + token;
 
        var isValid = validPassword == password;
        return isValid;
    }
}
```

```csharp
public interface IProfileDao
{
    string GetPassword(string account);
 
    string GetToken(string account);
}
```

```csharp
public static class ProfileDao
{
    private static IProfileDao _profileDao;
 
    //add internal setter for unit test to inject stub object
    internal static IProfileDao MyProfileDao
    {
        get
        {
            if (_profileDao == null)
            {
                // add a ProfileDaoImpl class and extract the original static function content to this class
                _profileDao = new ProfileDaoImpl();
            }
 
            return _profileDao;
        }
        set { _profileDao = value; }
    }
 
    internal static string GetPassword(string account)
    {
        // replace _profileDao filed to static property
        return MyProfileDao.GetPassword(account);
    }
 
    internal static string GetToken(string account)
    {
        // replace _profileDao filed to static property
        return MyProfileDao.GetToken(account);
    }
}
```

```csharp
public class ProfileDaoImpl : IProfileDao
{
    //just simulate data source
    private static Dictionary<string, string> fakePasswordDataset = new Dictionary<string, string>
    {
        {"joey","1234"},
        {"demo","!@#$"},
    };
 
    public string GetPassword(string account)
    {
        if (!fakePasswordDataset.ContainsKey(account))
        {
            throw new Exception("account not exist");
        }
 
        return fakePasswordDataset[account];
    }
 
    public string GetToken(string account)
    {
        //just for demo, it's insecure
        var seed = new Random((account.GetHashCode() + (int)DateTime.Now.Ticks) & 0x0000FFFF);
        var result = seed.Next(0, 999999);
        return result.ToString("000000");
    }
}


```

```csharp
[TestClass]
public class AuthServiceTest
{
    [TestMethod]
    public void Test_IsValid_joey_1234666666_Should_Return_True()
    {
        var target = new AuthService();
        var account = "joey";
        var password = "1234666666";
 
        //add IProfileDao stub
        IProfileDao stubProfileDao = Substitute.For<IProfileDao>();
        stubProfileDao.GetPassword("joey").ReturnsForAnyArgs("1234");
        stubProfileDao.GetToken("joey").ReturnsForAnyArgs("666666");
 
        //inject to ProfileDao static class
        ProfileDao.MyProfileDao = stubProfileDao;
 
        var actual = target.IsValid(account, password);
 
        var expected = true;
 
        //because of random, it should almost always assert failed
        actual.ShouldBe(expected);
    }
}

```