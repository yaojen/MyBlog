# on off
On用來繫結事件，Off則是用來解除繫結事件。

```javascript 
$(function(){
   
   //畫面上mybtn細節click事件
   $("#mybtn").click(function(){
       alert("first click event");
   });

   //解除繫結click事件
   $("#mybtn").off("click");

   //在解除繫結之後 重新繫結click事件，並自行定義事件內容
   $("#mybtn").on(function(){
      alert("second click event");
   });
})
```