# SQL

### 系統值

#### ERROR_MESSAGE()

函數->系統

在CATCH區塊中呼叫時，ERROR_MESSAGE會傳回造成CATCH區塊之錯誤訊息的完整文字，也就是有用到CATCH的時候才有用

```sql
BEGIN TRY
	SELECT 1/0
END TRY
BEGIN CATCH
	SELECT ERROR_MESSAGE() AS ErrorMessage
END CATCH

(0 row(s) affected)
ErrorMessage
----------------------------------
Divide by zero error encountered.

(1 row(s) affected)
```

也可以搭配其他使用

```mssql
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  BEGIN TRY
	SELECT 1/0
END TRY
BEGIN CATCH
	SELECT ERROR_MESSAGE() AS ErrorMessage
END CATCH

(0 row(s) affected)
ErrorMessage
----------------------------------
Divide by zero error encountered.

(1 row(s) affected)
```

#### @@ERROR

如果先前的 Transact-SQL 陳述式沒有發現任何錯誤，便傳回 0。

如果先前的陳述式發現錯誤，便傳回錯誤號碼。 如果錯誤是 sys.messages 目錄檢視中的錯誤之一，@@ERROR 會包含這項錯誤在 sys.messages.message_id 資料行中的值。 您可以在 sys.messages 中檢視 @@ERROR 錯誤號碼的相關聯文字。

由於執行每個陳述式時，都會清除再重設 @@ERROR，因此，請在陳述式之後，立即檢查它，或將它儲存在稍後能夠檢查的本機變數中。

#### SCOPE_IDENTITY()

函數 -> 中繼資料 

跟 @@IDENTITY很像，都會回傳插入識別欄位的值，但兩者的作用領域不同

SCOPE_IDENTITY()會回傳目前這個store Procedue的值，而@@IDENTITY則不限制在這個領域內。