# 常見事件Event

* 點擊(onclick)，點下後鬆開。
* 雙擊(ondblclick)，點擊兩次。
* 按下(onmousedown)
* 放開(onmouseup)
* 移入(onmouseover)
* 移出(onmouseout)
* 移動(onmouseover)

```html
<div id="mydiv" class="active"></div>
    <script>
        var mydiv = document.getElementById("mydiv")
        mydiv.onclick = function () { alert("onclick"); }
        mydiv.ondblclick = function () { alert("dbclick"); }
        mydiv.onmousedown = function () { alert("onmousedown"); }
        mydiv.onmouseup = function () { alert("onmouseup"); }
        mydiv.onmouseover = function () { alert("onmouseover"); }
        mydiv.onmouseout = function () { alert("onmouseout"); }
        mydiv.onmouseover = function () { console.log("onmouseover") }
    </script>
```