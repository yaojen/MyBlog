# this筆記
this指的是誰呼叫了這個對象。但如果在function當中呼叫了fun，this指的就是window這個對象

```html
<script>
   var fu = function () {
      console.log(this);
   };

fu(); // 在html的script中呼叫function，this 指的是windows

var mybtn = document.getElementById("bybtn");
mybtn.onclick = function(){ console.log(this);}  //這裡的this指的是觸發事件的button

mybtn.onclick = function(){
    fu();   //在事件中呼叫function    this指的是windows
}
</script>

```

## 將input都掛上點擊事件
**javascript方法**
   
```javascript
//將input掛上onclick事件
        for (var i = 0; i < btns.length; i++) {
            btns[i].onclick = function(){
            
                //先清除input的class
                for(var j=0;j<btns.length; j++)
                {
                    btns[i].className = '';
                }
                //再加上class
                this.className = 'active';
            }
        }
```


**用jQuery方法**
```html
<input type="button" value="按鈕一">
<input type="button" value="按鈕二">
<input type="button" value="按鈕三">

<script>
$(function () {
    $("input").on("click", function () {
        $(this).addClass('active');   //也是用this
    });
})
</script>
```