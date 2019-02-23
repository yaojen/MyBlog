# jQuery Selector 選擇器
***
## Basic 基本的選擇器
**All Selector**
選出所有的節點。但這個應該會跟其他的選擇器材合併使用。
> $.('*')

**Class Selector**
如下，選出具有Container這個class的節點
> $('.Container')

**Element Selector**
如下，選出html標籤為p的節點
> $('p') 

**ID Selector**
如下，選出ID為myTable的節點
> $('#myTable')

**Multiple Selector**
如下，選出ID為Id1，Id2，Id3的節點。
> $('#Id1,#Id2,#Id3')

*** 
## Hierarchy 選擇器

**$('父元素 子元素')**
只要是父元素下的子元素，且子元素的子元素都會被選到，以此類推。
>  $('#con2 p')

如下#con2下的兩個p元素都會被選到，加上.container class
```html
<div id="con2">
    <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Dolorum, ab!</p>
   <fieldset>
    <div>
        <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Ratione, alias!<p>
    </div>
      </fieldset>
 </div>
<button id="btn2">click</button>

<script>
 $('#con2 p').addClass('container');
</script>
```

**$('父元素 > 子元素')**

選出父元素下的子元素，子元素的子元素都不會被選到。跟$('父元素 子元素')不同
> $('#con2 > p')

***
## Attribute Selector
也就是字面上的意思，針對屬性來進行篩選。

**$('[attribute]')**
抓出具有該屬性的節點，如下只會抓出第二個p來新增container。
```html
<div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
    <p　id="123">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Digniss</p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Vero,</p>
</div>
<button id="mybtn">click</button>

<script>
$('#mybtn').click(function(){
    $('p[id]').addClass('container');
})

</script>
```


**$('[attribute="value"]')**
抓出屬性等於value的節點。下面例子，只會抓出id=p1的節點。

```html
<div>
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
  <p id="p1">Lorem ipsum dolor sit amet consectetur adipisicing elit. Distinctio, nisi?</p>
  <p id="p2">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dignissimos, excepturi.</p>
  <p id="p3">Lorem ipsum dolor sit amet consectetur adipisicing elit. Vero, ullam.</p>
</div>
<button id="mybtn">click</button>

<script>
$('#mybtn').click(function(){
  $('p[id = "p1"]').addClass('container');
})
<script>
```

**$('[attribute!="value"]')**
抓出屬性不等於value的節點，如果該節點不具有該屬性，也是會被抓出來的。
下面例子，就是除了id="p1"的節點不會被加上container類別外，其他都會被加入。
```html
<div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
    <p id="p1">Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
    <p id="p2">Lorem ipsum dolor sit amet, consectetur adipisicing elit</p>
    <p id="p3">Lorem ipsum dolor sit amet consectetur adipisicing elit</p>
</div>
<button id="mybtn">click</button>

<script>
$('#mybtn').click(function(){
    $('p[id != "p1"]').addClass('container');
})
</script>
```


**$('[attribute ^="value"]')**
抓出屬性的值為value開頭的節點。下面例子
```html
<div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
    <p id="2p1">Lorem ipsum dolor sit amet consectetur adipisicing elit. Distinctio, nisi?</p>
    <p id="3p2">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dignissimos, excepturi.</p>
    <p id="p3">Lorem ipsum dolor sit amet consectetur adipisicing elit. Vero, ullam.</p>
</div>
<button id="mybtn">click</button>

<script>
$('#mybtn').click(function(){
    $('p[id ^= "p"]').addClass('container');
})
</script>

```

**$('[attribute $="value"]')**
抓出屬性的值為value結尾的節點。下面例子，會抓出屬性id的值，結尾為2的節點。

```html
<div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
    <p id="2p1">Lorem ipsum dolor sit amet consectetur adipisicing elit. Distinctio, nisi?</p>
    <p id="3p2">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dignissimos, excepturi.</p>
    <p id="p3">Lorem ipsum dolor sit amet consectetur adipisicing elit. Vero, ullam.</p>
</div>
<button id="mybtn">click</button>

<script>
$('#mybtn').click(function(){
    $('p[id $= "2"]').addClass('container');
})
</script>
```


**$('[attribute *="value"]')**
抓出屬性的值包含value的節點，下面例子，會抓出id屬性中包含p的節點。

```html
<div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
    <p id="2p1">Lorem ipsum dolor sit amet consectetur adipisicing elit. Distinctio, nisi?</p>
    <p id="3p2">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dignissimos, excepturi.</p>
    <p id="p3">Lorem ipsum dolor sit amet consectetur adipisicing elit. Vero, ullam.</p>
</div>
<button id="mybtn">click</button>

<script>
$('#mybtn').click(function(){
    $('p[id *= "p"]').addClass('container');
})
</script>

```

**Multiple Attribute Selector [name=”value”][name2=”value2″]**
複合式的查詢，必須同時符合列出的條件，如下，必須抓出class=jj與id結尾為1的節點
```html
<div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.</p>
    <p id="2p1" class="jj">Lorem ipsum dolor sit amet consectetur adipisicing elit. Distinctio, nisi?</p>
    <p id="3p2" class="jj">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dignissimos, excepturi.</p>
    <p id="p3">Lorem ipsum dolor sit amet consectetur adipisicing elit. Vero, ullam.</p>
</div>
<button id="mybtn">click</button>

<script>
$('#mybtn').click(function(){
    $("p[class='jj'][id $= '1']").addClass('container');
})
</script>
```

## 表單　form Selector
注意這邊用的是:不是html上的=唷
**$(':input')**
抓到畫面上所有的input、select、textarea跟button。
如果這些都放在form裡面，其實效果跟$('from > * ')一樣

```html
<form>
  <input type="button" value="Input Button">
  <input type="checkbox">
  <input type="file">
  <input type="hidden">
  <input type="image">
  <input type="password">
  <input type="radio">
  <input type="reset">
  <input type="submit">
  <input type="text">
  <select>
    <option>Option</option>
    <option>Option</option>
    <option>Option</option>
  </select>
  <textarea></textarea>
  <button>Button</button>
</form>
<div id="messages"></div>

<script>
var allInputs = $(':input');

$('#messages').html(allInputs.length);
</script>

```

**$(':text')**
上面的表示法，會將畫面上所有的<input type="text">的節點都選到，因為它隱含*，
所以 $(':text') == $('*:text')的效果，所以原來的$('input:text')就被取代了。

下面例子，第一與第二選擇器都會抓到兩個type=text的節點，而第三個則只會抓到form裡面的。
```html
<form>
    <input type="button" value="Input Button">
    <input type="checkbox">
    <input type="file">
    <input type="hidden">
    <input type="image">
    <input type="password">
    <input type="radio">
    <input type="reset">
    <input type="submit">
    <input type="text">
    <select>
        <option>Option</option>
        <option>Option</option>
        <option>Option</option>
    </select>
    <textarea></textarea>
    <button>Button</button>
</form>
<div id="messages"></div>

<input type="text" />

<script>
    $(':text').addClass('container');
    $('input:text').addClass('container');
    $('form input:text').addClass('container');
</script>
```


[參考資料](https://oscarotero.com/jquery/)