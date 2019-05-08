# JavaScript Array 
js的陣列屬於複合的資料型態，也就是一個陣列可以存字串或數字。

**宣告方式**
```javascript
var arrayName = [item1,item2,item3,...];
```

**透過索引存取**
```javascript
ary[index];
```

**取得陣列長度**
```javascript
var fruits = ['Apple','Banana'];
console.log(fruits.length)
```

**新增元素**
使用push來新增元素或用ary[ary.length]方式
```javascript
var fruits = ['Apple','Banana'];
fruits.push('Orange');   //輸出: Apple / Banana / Orange

fruits[fruits.length] = 'Orange'; //上面這兩個方法都可以
//如過要加到陣列的最前面
fruits.unshift('Orange');  //輸出 Orange , Apple , Banana
```

**刪除元素**
```javascript
//使用pop，用來刪除陣列中最後一個元素，且會返回被刪除的元素
var fruits = ['Apple','Banana','Orange'];
var last = fruits.pop();  //刪除Orange，且返回Orange

var fruits = ['Apple','Banana'];
var first = fruits.shift(); //移除Apple，並返回Apple

//delete 可以刪除特定位置元素，但不會移除元素，只是將該位置的元素變成undefined

var fruits = ['Apple','Banana','Orange'];

delete fruits[0];  //[undefined,'Banana','Orange']


```


## Array物件內建的方法
### forEach()
forEach可遍歷陣列中每一個元素

**語法**
```javascript
array.forEach(callback[, thisArg])
```
* 參數callback是一個函數，這個函數會接收到三個參數
   * currentValue，代表目前處理的元素值。
   * index，代表目前處理的元素索引位置。
   * array，代表陣列本身
   * 根據callback處理結果，返回true表示通過，false則表示失敗。

```javascript
var myArray = [1,2,3,4,5,6];
var myArray2 = ['x','y','x','i'];

myArray.forEach(function(currentValue,index,array){
   alert(currentValue+ '  '+ index+' '+array.length);
});

myArray2.forEach(function(currentValue,index,array){
   alert(currentValue+ '  '+ index+' '+array.length);
});
```

**E 從 IE9 開始才有支援 forEach()。**

### concat
將兩個陣列合併成一個陣列

```javascript
var myArray = [1,2,3,4,5,6];
var myArray2 = ['x','y','x','i'];
var newarray = myArray.concat(myArray2);
```