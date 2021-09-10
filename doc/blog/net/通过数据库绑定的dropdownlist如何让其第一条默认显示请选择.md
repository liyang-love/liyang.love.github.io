## 通过数据库绑定的dropdownlist，如何让其第一条默认显示"--请选择--"

---

第一种方法

 

```c#
DropDownList1.Items.Insert(0,"请选择XXX"); 
```





第二种方法


在第一个位置插入一个项就可以

```c#
DropDownList1.Items.Insert(0, new ListItem("---请选择---", "0")); 
```

