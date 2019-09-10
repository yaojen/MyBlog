MongoDb基礎
===
1. 文件導向式資料庫，跟傳統資料庫相比，他不再有row觀念，而是document。

MongoDB資料結構
```javascript
[
    {name:"Mark",age:23},
    {name:"JS",age:23},
    {name:"RRR",age:23}
]
```
關聯式資料庫

Name          | age  |
--------------|:-----:|
"Mark"        | 22 |
"JS"          | 22 |
"RRR"         | 22 |

**優點**
* Schema-less : MongoDB擁有非常彈性的Schema，這對RDBMS來說非常的難以高效能的方法來實現。
* 易於擴展 : MongoDB的設計採用橫向擴展，它的document的數據模型使寫能很容易在多台伺服器之間進行數據分割。
* 優透的性能 : MongoDB能預分配，以利用額外的空間換取穩定，同時盡可能把多的內存用作cache，試圖為每次查詢自動選擇正確的索引。
**缺點**
* 不支援事務操作 : 所以通常不適合應用在銀行或會計這種系統上，因為不包證一致性。
* 占用比較多空間 : 主要是有兩個原因，首先是它會預分配空間，為了提高效能，而第二個原因是欄位所占用的空間。

**MongoDb的組成**
MongoDb =  Document + Collection 
Document就是key:value組合
```javascript
{
    name:"kevin",
    age:12
    title:"CTO"
}
```
key區分大小寫，在同一份document key不能相同
value可以存多種類型。

Collection就是一組Document，如果拿關聯式資料庫比喻，就是Table當中有多個ROW
Colection是動態的，也就是Collection的document可以是各種類型。
下面的document都可存在同一個collection當中
```javascript
{id:1,name:"mark"},
{age:100}
```