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

**$('prev + next')**
抓出符合prev選擇器後的，第一個符合next的選擇器，注意不是在prev內層唷，是他之後，跟prev是同一層。這應該是常跟Form的input做搭配。
下面範例會抓出input後的第span，也就是Mary1。Mary因為屬於不同層，所以不會被抓出來。
```html
<div>
  <input type="checkbox" name="a">
  <span>Mary1</span>
  <p>Mary2</p>
  <p>Mary3</p>
  <span>Mary4</span>
</div>
<span>Mary5</span>

<script>
    $('input:not(:checked) + span').addClass('container');
</script>
```

**$('prev ~siblings')**
抓出符合prev選擇器後，所有符合siblings選擇器的元素。一樣不是prev內層，而是他之後。這應該是常跟Form的input做搭配。

如下範例，會抓到input後的所有的p，也就是Mary2 / Mary3。
```html
<div>
  <input type="checkbox" name="a">
  <span>Mary1</span>
  <p>Mary2</p>
  <p>Mary3</p>
  <span>Mary4</span>
</div>

<script>
    $('input:not(:checked) ~ p').addClass('container');
</script>

```

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
**$(':password')**  

**$(':radio')**

**$(':checkbox')**

**$(':submit')**

**$(':image')**

**$(':reset')**

**$(':button')**

**$(':file')**
意義同$(':text')方法。

**$(':disabled')**
**$(':enabled')**
抓取input欄位disabled或enabled的節點
下面第一個方法，抓到form裡面的input為disabled
第二個方法，抓到所有disabled的input
```html
<form >
  <input name="email" disabled="disabled">
  <input name="id">
</form>
 <input name="email" disabled="disabled">

<script>
  $('form :disabled').addClass('container');
  $(':disabled').addClass('container');
</script>

```


## Visibility Filters
**$(':hidden')**
下面這幾種狀況都被視為hidden，CSS的display的value為none，type="hidden"(這個是input標籤才有的屬性)，width跟height設定為0，父節點設定為Hidden，這些都是被視為hidden。

下面例子會讓hidden的部分顯示出來。
```html
<form action="">
  <input type="text" value="12345">
  <input type="hidden" value="99099" >
  <input type="text" value="kkkk">
  <input type="submit" value="button" id="submit">
</form>

<script>
  $('#submit').click(function(){
    $(':hidden').attr({'type':'text'});
  })
</script>
```

**$(':visible')**
應用如hidden

## Basic Filter 基本過濾器
**$(':eq(index)')**
選擇器當中的內容，本身就是一個集合，可以透過:eq(index)，抓出指定的節點。另外jQuery還有一個$('selector').eq(index)，這個的效能會比放在選擇器的效能好。
下面的範例，b跟d都會被選到，添加container屬性。
```html
<ul>
  <li>a</li>
  <li>b</li>
  <li>c</li>
  <li>d</li>
  <li>e</li>
</ul>

<script>
    $('ul > li:eq(1)').addClass('container');

    $('ul > li').eq(3).addClass('container');
</script>
```

另外也可以使用負數，下面範例，e會被選到。

```html
<ul>
  <li>a</li>
  <li>b</li>
  <li>c</li>
  <li>d</li>
  <li>e</li>
</ul>

<script>
    $('ul > li:eq(-1)').addClass('container');
</script>

```


**$(':first') / $(':last')**
跟:eq(index)使用方法一樣，選擇器本身就是一個集合，透過:first，抓出第一筆節點。這個方法也等於eq(0)。
另外jQuery本身也有.first()方法，功能一樣。
下面範例會抓出Row1的結點。

```html
<table>
  <tr><td>Row 1</td></tr>
  <tr><td>Row 2</td></tr>
  <tr><td>Row 3</td></tr>
</table>

<script>
/$('tr:first').addClass('container');
$('tr').first().addClass('container');
</script>
```

**$(':even') / $(':odd')**
抓出奇數/偶數的節點。這個就沒有像是eq/first有另外的jQuery方法。

```html
<table>
  <tr><td>Row 1</td></tr>
  <tr><td>Row 2</td></tr>
  <tr><td>Row 3</td></tr>
  <tr><td>Row 4</td></tr>
  <tr><td>Row 5</td></tr>
  <tr><td>Row 6</td></tr>
  <tr><td>Row 7</td></tr>
</table>

<script>
 $('tr:even').addClass('container');
</script>

```

**$(':gt(index)') / $(':lt(index)')**
gt是抓出大於index的節點，lt則是抓出小於index的節點。其中index也可以是負數。
下面範例會抓出，Row4~Row7
```html
<table>
  <tr><td>Row 1</td></tr>
  <tr><td>Row 2</td></tr>
  <tr><td>Row 3</td></tr>
  <tr><td>Row 4</td></tr>
  <tr><td>Row 5</td></tr>
  <tr><td>Row 6</td></tr>
  <tr><td>Row 7</td></tr>
</table>

<script>
    $('tr:gt(2)').addClass('container');
</script>
```

**$(':not(selector)')**
排除符合not當中的selector節點。
下列中，會軒到Mary跟Icm的span，
```html
<div>
  <input type="checkbox" name="a"><span>Mary</span>
</div>
<div>
  <input type="checkbox" name="b"><span>lcm</span>
</div>
<div>
  <input type="checkbox" name="c" checked="checked"><span>Peter</span>
</div>

<script>
    $('input:not(:checked) + span').addClass('container');
</script>

```

**$(':header')**
抓出所有的header，例如h1/h2/h3....
如下範例，會抓出h1/h2
```html
<h1>Header 1</h1>
<p>Contents 1</p>
<h2>Header 2</h2>
<p>Contents 2</p>

<script> 
    $(':header').addClass('container');
</script>

```

## Content Filter

**$(":contains('text')")**
抓出內容有包含text的節點

```html
<h1>Header 1</h1>
<p>Contents 1</p>
<h2>Header 2</h2>
<p>Contents 2</p>

<script>
    $("p:contains('1')").addClass('container');
</script>
```

**$(":empty")**
抓出過濾器內容是完全空的，沒有html tag，也沒有內容。
下面範例會抓出，第三個p，第一個p還有span這個tag，所以不會被抓到。

```html
<div>
  <p><span></span></p>
  <p>134</p>
  <p></p>
</div>

<script>
    $("div > p:empty").addClass('container');
</script>
```

**$(":has(selector)")**
抓除只要有has中的selector就可以。

```html
<div><p>Hello in a paragraph</p></div>
<div>Hello again! (with no paragraph)</div>

<script>
    $( "div:has(p)" ).addClass( "container" );
</script>
```


**$(":parent")**
抓出至少有一個子節點的元素


```html
<table>
  <tr><td></td><td></td></tr>
  <tr><td></td><td>12345</td></tr>
   <tr><td></td><td></td></tr>
   <tr><td></td><td></td></tr>
</table>

<script>
    $("td:parent").addClass('container');
</script>

```


[參考資料](https://oscarotero.com/jquery/)


