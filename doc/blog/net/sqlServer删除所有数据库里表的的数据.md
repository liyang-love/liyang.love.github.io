## sql Server 删除所有数据库里表的的数据

---

```sql
一、

执行以下存储过程


  CREATE PROCEDURE sp_DeleteAllData
AS
EXEC sp_MSForEachTable 'ALTER TABLE ? NOCHECK CONSTRAINT ALL'
EXEC sp_MSForEachTable 'ALTER TABLE ? DISABLE TRIGGER ALL'
EXEC sp_MSForEachTable 'DELETE FROM ?'
EXEC sp_MSForEachTable 'ALTER TABLE ? CHECK CONSTRAINT ALL'
EXEC sp_MSForEachTable 'ALTER TABLE ? ENABLE TRIGGER ALL'
EXEC sp_MSFOREACHTABLE 'SELECT * FROM ?'
GO

//执行存储过程
exec sp_DeleteAllData
```

