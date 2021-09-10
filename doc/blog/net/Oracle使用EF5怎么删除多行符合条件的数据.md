## Oracle 使用EF5怎么删除多行符合条件的数据

---

Oracle  使用EF删除多条数据

 ```c#
 using (var db = new Entities())
       {
 
 　　　　　　　　　　//TBS_JK_GIS. 表空间
 
 　　　　　　　　db.Database.ExecuteSqlCommand("DELETE FROM TBS_JK_GIS.T_REPORTOPERATINGREPORT WHERE RQ=:RQ and GS=:GS",
                          new Oracle.ManagedDataAccess.Client.OracleParameter[]{
                          new Oracle.ManagedDataAccess.Client.OracleParameter("RQ",info.RQ),
                          new Oracle.ManagedDataAccess.Client.OracleParameter("GS",info.GS)});
 
 }
 ```



 

注：红色部分  

1、 RQ=:RQ and GS=:GS  oracle里面使用的是“：” 而SQL里面使用的是“@”

2、ManagedDataAccess  要使用这个

3、OracleParameter("RQ",info.RQ)  不能写成OracleParameter("@RQ",info.RQ), 否则或报错  和SQL的不同

 