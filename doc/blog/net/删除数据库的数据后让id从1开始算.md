## [删除数据库的数据后让id从1开始算](https://www.cnblogs.com/liyangLife/p/3622985.html)

---

```sql
delete from t_AttendanceRecord
dbcc checkident('t_AttendanceRecord',reseed,0)
```

