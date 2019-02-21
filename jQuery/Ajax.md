# jQuery - Ajax
紀錄jQuery Ajax的相關方法

---
## $.ajax()
```javascript
$.ajax({
        url: url,
        type: "post",
        datatype: "json",
        success: function(data){...},
        error: function() {...},
        complete: function() {...}
});
```
**參數說明**
* url:要呼叫服務的網址。
* type:資料的傳送形式，可以是post / get / delete等
* data:要傳送到server的資料。
* datatype:預期server會回傳的資料型態。
* success:成功時要執行的函式。
* error:失敗時要執行的函式。
* complete:完成時要執行的函式。
* async：是否使用非同步，默認式true。

## $.get()

## $.post()

## $.load()

## $.getJSON(url [, data ] [, success ])



[參考資料](https://api.jquery.com/category/ajax/)
[參考資料](http://www.w3school.com.cn/jquery/jquery_ajax_intro.asp)