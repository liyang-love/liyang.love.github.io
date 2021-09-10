## Oracle 相关操作

---

```sql
--BBBH序列

drop SEQUENCE seq_t_ReportRecord_BBBH;//删除序列

create sequence seq_t_ReportRecord_BBBH  //序列名称

increment by 1   每次增量1

start with 1    开始增量1

nomaxvalue   

nominvalue

cache 20   高速缓存大小默认20

 

---触发器

create or replace trigger tr_t_ReportRecord_BBBH  //创建触发器

before insert on t_ReportRecord   //表名

for each row

begin

select seq_t_ReportRecord_BBBH.nextval into :new.BBBH from dual;   // 序列名，列名

end;

 

---查询日期

select to_char(字段名,'yyyy-mm-dd hh24:mi:ss') from 表名

 

 

---修改数据库临时时间格式

alter session set nls_date_format='yyyy-mm-dd hh24:mi:ss'
```

