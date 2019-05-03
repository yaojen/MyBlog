Authorization 授權
asp.net mvc當中有一個Authorization屬性
在controller/action上掛這個屬性，代表該controller/action需要登入才可以使用

也可以透過繼承Authorization類別，實作IAuthorizationFilter介面的
改寫OnAuthorization方法
https://dotblogs.com.tw/ricochen/2010/03/19/14113
https://dotblogs.com.tw/jesperlai/2018/03/20/170705
http://fecbob.pixnet.net/blog/post/42740323-mvc%E4%B8%AD%E4%BD%BF%E7%94%A8authorizeattribute%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A0%85
https://blog.exfast.me/2016/07/c-asp-net-mvc5-inherit-authorizeattribute-to-implement-custom-validation/
https://www.itread01.com/p/630046.html